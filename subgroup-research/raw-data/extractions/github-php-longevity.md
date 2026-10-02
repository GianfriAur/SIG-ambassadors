# PHP projects by how long they have been running

**Source:** [GitHub REST API](../sources.md#15-github-rest-api) through [GitHub PHP repositories](github-php-repositories.md), optionally corrected with [Google BigQuery](../sources.md#11-google-bigquery).

**data freshness: live, inherited from the population it reads**

Takes the 1,553 repositories gathered by [GitHub PHP repositories](github-php-repositories.md) and answers which PHP projects have been running longest and are still running now. It keeps the top 1,000 by score, which is everything that survives the filters bar the last 70; the report quotes only the first 100 of them. Age on its own would return a graveyard, and stars on their own would return whatever is fashionable, so the ranking has to hold both.

### Eligibility

Three filters, applied before anything is scored.

| Filter | Why |
|---|---|
| not `archived` | an archived repository is finished, not long running |
| not `fork` | a fork inherits its parent's history and would duplicate it |
| `pushed_at` within the last 12 months | still running has to mean something |

### Score

```
score = months × log10(stars)
```

`months` runs from the project's first date to today. `stars` is `stargazers_count`.

The logarithm is doing necessary work. Across this population stars span three orders of magnitude while months span less than one, so a plain product is decided by stars alone and the ranking stops being about longevity: `coollabsio/coolify`, created in 2021, outranks `drupal/drupal` and `phpmyadmin/phpmyadmin` on `months × stars`. Compressing the star scale puts age back in charge and leaves stars as a weight, which is what the question asks for.

The constant in a formula of the shape `months × (k × stars)` cannot change a ranking, whatever `k` is, since it scales every row alike.

### The first date

`created_at` from the API is when the repository appeared on GitHub. For anything that predates GitHub it is wrong, and wrong in the direction that matters:

| Project | `created_at` | Project actually began |
|---|---|---|
| `phpmyadmin/phpmyadmin` | 2012-01-19 | 1998 |
| `drupal/drupal` | 2009-01-04 | 2001 |

Conversions from CVS and SVN carried their history into git, so the earliest commit is a much better floor than the repository creation date. It is not in the API, but it is in BigQuery.

> [!important]
> this query has been run and its export feeds the ranking, but it covers only part of the population. See Results.

```sql
-- Earliest credible commit per repository.
-- Author dates in github_repos.commits are self-reported and contain junk:
-- the Unix epoch, dates in the future, and dates before git existed.
SELECT
  repo_name,
  MIN(author_date) AS first_commit,
  MAX(author_date) AS last_commit,
  COUNT(*)         AS commits
FROM (
  SELECT
    repo_name,
    TIMESTAMP_SECONDS(author.time_sec) AS author_date
  FROM `bigquery-public-data.github_repos.commits`, UNNEST(repo_name) AS repo_name
)
WHERE author_date BETWEEN TIMESTAMP '1995-01-01' AND CURRENT_TIMESTAMP()
GROUP BY repo_name
HAVING COUNT(*) >= 10   -- a repo with a handful of commits tells us nothing
ORDER BY first_commit;
```
> [!important]
> result ~160MB

The 1995 floor is deliberately looser than git itself, which dates from 2005, because an import can legitimately carry a 1998 commit date. What it excludes is the epoch and the obvious nonsense. The scoring script drops anything landing exactly on that bound as well, since a date sitting on the filter is the filter showing through.

Export the result as CSV and put it next to the script as `first_commits.csv`. The join is on `full_name`, and the earlier of the two dates wins, so a repository the export does not cover simply keeps its GitHub date and says so in the output.

### Scoring

`score.php`, run with `php score.php` once the population has been collected.

```php
<?php
declare(strict_types=1);

/**
 * Ranks the collected population by how long each project has been running
 * and is still running.
 *
 *   score = months * log10(stars)
 *
 * The logarithm is load-bearing. Stars span three orders of magnitude across
 * this population while months span less than one, so a plain months * stars
 * is decided by the star count and stops measuring longevity.
 *
 * Months run from the earliest date available for the project. The GitHub
 * creation date is a poor floor for anything older than GitHub, so the first
 * commit from bigquery-public-data.github_repos.commits is preferred wherever
 * the export covers the repository.
 */

const ACTIVE_WITHIN_MONTHS = 12;
const DAYS_PER_MONTH       = 30.44;
const KEEP                 = 1000;

const POPULATION   = 'php_repos.json';
const FIRST_COMMIT = 'first_commits.csv';   // the BigQuery export, optional
const OUTPUT       = 'php_longevity_ranked.json';

// The query already excludes the epoch and the future, but a commit date sitting
// exactly on its lower bound is the bound showing through, not a real date.
const FLOOR = '1995-01-02';

$utc   = new DateTimeZone('UTC');
$today = new DateTimeImmutable('now', $utc);

function monthsBetween(DateTimeImmutable $from, DateTimeImmutable $to): float
{
    return (float) $from->diff($to)->days / DAYS_PER_MONTH;
}

/** Streams the export so a 160 MB file never lands in memory whole. */
function loadFirstCommits(string $path, array $wanted): array
{
    if (!is_readable($path)) {
        fwrite(STDERR, "no {$path}, falling back to GitHub creation dates
");
        return [];
    }

    $handle  = fopen($path, 'r');
    $columns = fgetcsv($handle, escape: '');
    $found   = [];

    while (($row = fgetcsv($handle, escape: '')) !== false) {
        $record = array_combine($columns, $row);
        if (isset($wanted[$record['repo_name']])) {
            $found[$record['repo_name']] = $record['first_commit'];
        }
    }
    fclose($handle);

    return $found;
}

$population = json_decode(file_get_contents(POPULATION), true);
$byName     = array_column($population, null, 'full_name');
$firstCommit = loadFirstCommits(FIRST_COMMIT, $byName);

$ranked    = [];
$archived  = 0;
$forks     = 0;
$quiet     = 0;
$corrected = 0;

foreach ($population as $repo) {
    // Archived is finished, not long running.
    if ($repo['archived']) {
        $archived++;
        continue;
    }
    // A fork inherits its parent's history and would duplicate it.
    if ($repo['fork']) {
        $forks++;
        continue;
    }

    $pushed = new DateTimeImmutable($repo['pushed_at']);
    if (monthsBetween($pushed, $today) > ACTIVE_WITHIN_MONTHS) {
        $quiet++;
        continue;
    }

    $start  = new DateTimeImmutable($repo['created_at']);
    $source = 'github';

    $commit = $firstCommit[$repo['full_name']] ?? null;
    if ($commit !== null && substr($commit, 0, 10) > FLOOR) {
        $candidate = new DateTimeImmutable($commit, $utc);
        if ($candidate < $start) {
            $start  = $candidate;
            $source = 'commit';
            $corrected++;
        }
    }

    $months = monthsBetween($start, $today);
    $stars  = (int) $repo['stargazers_count'];

    $ranked[] = $repo + [
        'start'        => $start->format('Y-m-d'),
        'start_source' => $source,
        'months'       => (int) round($months),
        'score'        => round($months * log10((float) $stars), 1),
    ];
}

usort($ranked, static fn (array $a, array $b): int => $b['score'] <=> $a['score']);

file_put_contents(OUTPUT,
    json_encode(array_slice($ranked, 0, KEEP), JSON_PRETTY_PRINT | JSON_UNESCAPED_SLASHES));

fwrite(STDERR, sprintf(
    "collected %d | archived %d | forks %d | quiet over %d months %d | eligible %d
",
    count($population), $archived, $forks, ACTIVE_WITHIN_MONTHS, $quiet, count($ranked)
));
fwrite(STDERR, sprintf(
    "start date from first commit for %d of them, from GitHub creation for %d
",
    $corrected, count($ranked) - $corrected
));
```

Swap `$repo['created_at']` for the first commit once the BigQuery query above has been run.

## Results

**collected: 2026-10-02**

1,553 collected, of which 135 archived and 348 without a push in the last 12 months, leaving 1,070 eligible. The top 1,000 are written to `php_longevity_ranked.json`.

The BigQuery export covers 640 of the 1,553 collected. An earlier first commit was found for **202 of the 1,070 eligible**, 19%, and those rows move a long way: `phpmyadmin/phpmyadmin` goes from 176 months to 305 and from 67th place to 1st. The other 868 keep their GitHub creation date, so the ranking mixes two bases and each row carries a `start_source` saying which one it used.

`bigquery-public-data.github_repos` only covers repositories GitHub classifies as open source through its License API, and the gaps are not random. `drupal/drupal`, `WordPress/WordPress`, `phpbb/phpbb`, `PrestaShop/PrestaShop` and `smarty-php/smarty` are all absent, and all five are understated as a result.

The first 100 are in [Top 10 long running PHP](../../reports/top-long-running-php.md).

Back to the [source list](../sources.md), the [list of extractions](../sources.md#2-extractions) or the [reports index](../../reports/README.md).
