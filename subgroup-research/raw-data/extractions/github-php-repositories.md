# GitHub PHP repositories with 1,000 stars or more

**Source:** [GitHub REST API](../sources.md#15-github-rest-api). [Open the search endpoint documentation](https://docs.github.com/en/rest/search/search#search-repositories).

**data freshness: live, the API answers from current state**

The population every question about PHP projects needs: which repositories GitHub classifies as PHP, how old each one is, how many stars it carries, and whether anyone has touched it lately. This extraction only gathers. Ranking and filtering happen in [GitHub PHP longevity](github-php-longevity.md), which reads this output.

The star floor is set at 1,000. Below that the list fills with abandoned experiments and one-file demos, and no question this group is asking is answered by them.

### Method

`GET /search/repositories` with `q=language:PHP stars:>=1000`. A search returns at most 1,000 results no matter how many matched, and 1,553 repositories qualify, so the population is collected in two windows that each stay under the cap:

| Window | Matches |
|---|---:|
| `language:PHP stars:1000..1999` | 800 |
| `language:PHP stars:2000..2999` | 263 |
| `language:PHP stars:3000..4999` | 223 |
| `language:PHP stars:>=5000` | 267 |
| **total** | **1,553** |

The windows sum to exactly the 1,553 that `stars:>=1000` reports on its own, and the collector ends with 1,553 distinct names. That is the check worth running: a short sum means a window hit the cap, and a long one means the boundaries overlap.

Each window is paged at 100 per request. Anonymous callers get 10 search requests a minute, so the collector waits between calls.

Two windows would be enough today, at 800 and 753. Four leaves room: these counts only go up, and a window that reaches 1,000 drops rows without reporting an error.

`language:PHP` is GitHub Linguist's verdict, the same classifier behind `bigquery-public-data.github_repos.languages`. It picks the language with the most bytes, so a project with a large JavaScript front end and a PHP back end can land outside this population.

### Fields kept

| Field | Meaning |
|---|---|
| `full_name` | `owner/name` |
| `stargazers_count` | stars at the time of collection |
| `created_at` | when the repository was created **on GitHub**, not when the project started |
| `pushed_at` | last push to any branch |
| `archived` | marked read-only by its owner |
| `fork` | a copy of another repository |
| `license` | SPDX identifier, `null` where GitHub could not detect one |
| `description`, `html_url`, `forks_count`, `open_issues_count` | context |

> [!warning]
> `created_at` understates every project that existed before GitHub. `phpmyadmin/phpmyadmin` reports 2012-01-19 against a project that began in 1998. Anything derived from it is a floor, and the floor is lowest for exactly the projects a longevity question is about. See the first-commit query in [GitHub PHP longevity](github-php-longevity.md).

### Collector

`collect.php`, run with `php collect.php`. No extensions beyond what ships by default.

```php
<?php
declare(strict_types=1);

/**
 * Collects every repository GitHub classifies as PHP with at least 1,000 stars.
 *
 * A search returns at most 1,000 results however many matched, so the population
 * is gathered in star windows. Keep each one well under the cap: these counts only
 * go up, and a window that reaches 1,000 drops rows without reporting an error.
 */

const WINDOWS = [
    'language:PHP stars:1000..1999',
    'language:PHP stars:2000..2999',
    'language:PHP stars:3000..4999',
    'language:PHP stars:>=5000',
];

const FIELDS = [
    'full_name', 'stargazers_count', 'created_at', 'pushed_at',
    'archived', 'fork', 'language', 'description', 'html_url',
    'forks_count', 'open_issues_count',
];

const PER_PAGE   = 100;
const THROTTLE   = 7;  // anonymous callers get 10 search requests a minute
const MAX_RETRIES = 4;

function fetch(string $url): array
{
    $context = stream_context_create(['http' => [
        'header'        => "Accept: application/vnd.github+json\r\n"
                         . "User-Agent: sig-ambassadors-research\r\n",
        'timeout'       => 30,
        'ignore_errors' => true,
    ]]);

    for ($attempt = 1; $attempt <= MAX_RETRIES; $attempt++) {
        $body = @file_get_contents($url, false, $context);
        if ($body !== false) {
            $decoded = json_decode($body, true);
            if (isset($decoded['items'])) {
                return $decoded;
            }
            // Rate limited or otherwise refused: $http_response_header holds the status.
            fwrite(STDERR, sprintf("  attempt %d: %s\n", $attempt,
                $decoded['message'] ?? 'unexpected response'));
        } else {
            fwrite(STDERR, sprintf("  attempt %d: request failed\n", $attempt));
        }
        sleep(20);
    }

    fwrite(STDERR, "gave up on {$url}\n");
    exit(1);
}

$rows   = [];
$totals = [];

foreach (WINDOWS as $window) {
    $page = 1;
    while (true) {
        $url = 'https://api.github.com/search/repositories?' . http_build_query([
            'q'        => $window,
            'sort'     => 'stars',
            'order'    => 'desc',
            'per_page' => PER_PAGE,
            'page'     => $page,
        ]);

        $result = fetch($url);
        $items  = $result['items'];
        $totals[$window] = $result['total_count'];

        fwrite(STDERR, sprintf("%s page %d: %d of %d\n",
            $window, $page, count($items), $result['total_count']));

        foreach ($items as $repo) {
            $row = [];
            foreach (FIELDS as $field) {
                $row[$field] = $repo[$field] ?? null;
            }
            $row['license'] = $repo['license']['spdx_id'] ?? null;

            // Keyed by name, so a repository gaining a star mid-collection and
            // appearing in two windows is stored once rather than twice.
            $rows[$repo['full_name']] = $row;
        }

        if (count($items) < PER_PAGE) {
            break;
        }
        $page++;
        sleep(THROTTLE);
    }
    sleep(THROTTLE);
}

file_put_contents('php_repos.json',
    json_encode(array_values($rows), JSON_PRETTY_PRINT | JSON_UNESCAPED_SLASHES));

$sum = array_sum($totals);
fwrite(STDERR, sprintf("\ncollected %d distinct repositories, windows sum to %d\n",
    count($rows), $sum));

// The windows partition the population, so these two must agree. A short sum
// means a window hit the 1,000 cap; a long one means the boundaries overlap.
if (count($rows) !== $sum) {
    fwrite(STDERR, "WARNING: the windows do not partition the population\n");
    exit(1);
}
```

Rows are keyed by `full_name` so that a repository gaining a star mid-collection, and so appearing in two windows, is stored once rather than twice. The script then checks that the distinct count equals the sum of the window totals and exits non-zero if it does not, which is the only way a silently truncated window would show up.

## Results

**collected: 2026-10-02**

1,553 repositories. The counts are in [Top 10 long running PHP](../../reports/top-long-running-php.md), which ranks them by how long they have been running.

Back to the [source list](../sources.md), the [list of extractions](../sources.md#2-extractions) or the [reports index](../../reports/README.md).
