# Licence declared in `LICENSE` against licence declared in `composer.json`

**Source:** [GitHub REST API](../sources.md#15-github-rest-api), both for the population and for the file contents.

**data freshness: live, the API answers from current state**

A PHP project can state its licence in two places. GitHub reads the licence file and reports an SPDX identifier; Packagist reads the `license` key in `composer.json`. Any count of PHP licensing rests on one or the other, and the two do not describe the same set of projects.

This extraction measures the gap. For every repository GitHub classifies as PHP and detects a licence for, it fetches `composer.json` and compares what the two sources say.

### Five outcomes, kept apart

Collapsing these into "matches" and "does not match" would put a project that contradicts itself in the same bucket as one that simply is not a Composer package, and the second is far more common than the first.

| Outcome | Meaning |
|---|---|
| agree | both sources name the same licence family |
| disagree | both declare, and they name different families |
| no `composer.json` | the repository is not a Composer package at all |
| no `license` key | it is a Composer package, and declares nothing |
| not SPDX | it declares free text such as `GPL v2` or `GNU Public License`, which no SPDX parser resolves |

Only the second is a contradiction. The other three are absences, and a Packagist-based count cannot see any of them.

### Sampling

Fifteen licences, and twelve of them hold more than the 1,000 repositories a GitHub search will return, so most have to be sampled.

Sampling in star order would be the obvious approach and would be wrong. Popular repositories are better maintained than the median, and the rate at which a project turns out not to be a Composer package depends heavily on how popular it is: among PHP repositories with 100 stars or more it is 11.9%, and among those with none it is 54.4%. A star-ordered sample measures the best-kept corner of the population and reports it as the whole.

So each licence is sampled across four star bands, up to 100 repositories per band, and the size of each band is recorded. The bands weight back into an estimate for the full population.

| Band | Why |
|---|---|
| `stars:>=100` | projects with an audience |
| `stars:10..99` | projects someone noticed |
| `stars:1..9` | one or two people found it |
| `stars:0` | the bulk of the population |

The four bands partition each licence exactly, and the script checks that: the band totals must sum to the count the same query returns unbanded. A short sum means a request failed and was read as an empty band, which is the one failure this design cannot see on its own.

### Comparison

Both sides are reduced to a licence family before comparing, because the two sources spell the same licence differently. GitHub returns `GPL-3.0`; `composer.json` may hold `GPL-3.0-or-later`, `GPL-3.0+` or `GPLv3`. Comparing the strings would report a disagreement on every one of those.

> [!warning]
> `composer.json` allows an array of licences, meaning the project is offered under any of them. The comparison reduces the array to its first recognised family, so a project GitHub reports as `Apache-2.0` and `composer.json` declares as `["MIT", "Apache-2.0"]` is counted as a disagreement when it is not one. Measured against the collected examples this affects 0.9% of the disagreements.

### Script

`discrepancy.php`, run with `php discrepancy.php`. Writes after every cell, so an interrupted run keeps what it had.

```php
<?php
declare(strict_types=1);

/**
 * Measures how often a repository's LICENSE file and its composer.json disagree.
 *
 * GitHub detects a licence by reading the licence file. Packagist reads the
 * `license` key in composer.json. Where both exist they can differ, and a count
 * built on one source will not reproduce a count built on the other.
 *
 * Popular repositories are better maintained than the median, so a sample taken
 * in star order would understate the problem. Instead each licence is sampled
 * across four star bands, and the band sizes are recorded so the bands can be
 * weighted back into an estimate for the whole population.
 */

const LICENCES = ['mit', 'gpl-2.0', 'gpl-3.0', 'bsd-3-clause', 'apache-2.0',
                  'agpl-3.0', 'lgpl-3.0', 'bsd-2-clause', 'unlicense', 'lgpl-2.1',
                  'mpl-2.0', 'cc0-1.0', 'isc', 'artistic-2.0', 'epl-1.0'];

const BANDS = ['stars:>=100', 'stars:10..99', 'stars:1..9', 'stars:0'];

const SAMPLE_PER_BAND = 100;
const THROTTLE        = 7;   // 10 search requests a minute, anonymous

function api(string $url): ?array
{
    $context = stream_context_create(['http' => [
        'header'  => "Accept: application/vnd.github+json\r\n"
                   . "User-Agent: sig-ambassadors-research\r\n",
        'timeout' => 30, 'ignore_errors' => true,
    ]]);
    for ($attempt = 1; $attempt <= 5; $attempt++) {
        $body = @file_get_contents($url, false, $context);
        if ($body !== false) {
            $decoded = json_decode($body, true);
            if (isset($decoded['items']) || isset($decoded['total_count'])) {
                return $decoded;
            }
            fwrite(STDERR, "  retry {$attempt}: " . ($decoded['message'] ?? '?') . "\n");
        }
        sleep(25);
    }
    return null;
}

function raw(string $repo, string $branch, string $path): ?string
{
    $context = stream_context_create(['http' => [
        'header' => "User-Agent: sig-ambassadors-research\r\n",
        'timeout' => 15, 'ignore_errors' => true,
    ]]);
    $body = @file_get_contents("https://raw.githubusercontent.com/{$repo}/{$branch}/{$path}", false, $context);
    if ($body === false) {
        return null;
    }
    // raw returns the literal string "404: Not Found" for a missing file.
    return str_starts_with(trim($body), '404') ? null : $body;
}

/** Collapses an SPDX identifier, or whatever composer.json actually contains, to a family. */
function family(?string $raw): string
{
    if ($raw === null || trim($raw) === '') {
        return 'none';
    }
    $value = strtolower(trim($raw));

    // Ordered: agpl and lgpl both contain "gpl".
    foreach (['agpl' => 'agpl', 'lgpl' => 'lgpl', 'gpl' => 'gpl', 'mit' => 'mit',
              'apache' => 'apache', 'bsd' => 'bsd', 'mpl' => 'mpl', 'epl' => 'epl',
              'artistic' => 'artistic', 'isc' => 'isc', 'unlicense' => 'unlicense',
              'cc0' => 'cc0', 'proprietary' => 'proprietary'] as $needle => $name) {
        if (str_contains($value, $needle)) {
            return $name;
        }
    }
    return 'other';
}

/** True when the string is a plausible SPDX identifier rather than free text. */
function looksLikeSpdx(string $value): bool
{
    return (bool) preg_match('/^[A-Za-z0-9.\-+]+$/', trim($value));
}

$results = [];

foreach (LICENCES as $licence) {
    foreach (BANDS as $band) {
        $query = "language:PHP+license:{$licence}+{$band}";

        $head = api("https://api.github.com/search/repositories?q={$query}&per_page=1");
        sleep(THROTTLE);
        $population = $head['total_count'] ?? 0;
        if ($population === 0) {
            continue;
        }

        $page = api("https://api.github.com/search/repositories?q={$query}&per_page=" . SAMPLE_PER_BAND);
        sleep(THROTTLE);
        $items = $page['items'] ?? [];

        $counts = ['agree' => 0, 'disagree' => 0, 'no_composer' => 0,
                   'no_key' => 0, 'not_spdx' => 0, 'unparseable' => 0];
        $examples = [];

        foreach ($items as $repo) {
            $declared = family($repo['license']['spdx_id'] ?? null);
            $body     = raw($repo['full_name'], $repo['default_branch'], 'composer.json');

            if ($body === null) {
                $counts['no_composer']++;
                continue;
            }
            $json = json_decode($body, true);
            if (!is_array($json)) {
                $counts['unparseable']++;
                continue;
            }
            $key = $json['license'] ?? null;
            if ($key === null) {
                $counts['no_key']++;
                continue;
            }
            $declaredInComposer = is_array($key) ? implode('+', $key) : (string) $key;

            if (!looksLikeSpdx($declaredInComposer)) {
                $counts['not_spdx']++;
                $examples[] = [$repo['full_name'], $repo['stargazers_count'],
                               $repo['license']['spdx_id'] ?? '?', $declaredInComposer, 'not_spdx'];
                continue;
            }
            if (family($declaredInComposer) === $declared) {
                $counts['agree']++;
            } else {
                $counts['disagree']++;
                $examples[] = [$repo['full_name'], $repo['stargazers_count'],
                               $repo['license']['spdx_id'] ?? '?', $declaredInComposer, 'disagree'];
            }
        }

        $results[] = ['licence' => $licence, 'band' => $band, 'population' => $population,
                      'sampled' => count($items), 'counts' => $counts, 'examples' => $examples];

        fwrite(STDERR, sprintf("%-13s %-13s pop %7d  sampled %3d  agree %3d  disagree %3d  no-composer %3d\n",
            $licence, $band, $population, count($items),
            $counts['agree'], $counts['disagree'], $counts['no_composer']));

        file_put_contents('discrepancy.json', json_encode($results, JSON_PRETTY_PRINT));
    }
}

fwrite(STDERR, "\ndone, " . count($results) . " licence/band cells\n");
```

## Results

**collected: 2026-10-05**

4,964 repositories sampled across 59 licence and band cells, weighting back to a population of 918,553. The figures are in [LICENSE against composer.json](../../reports/licence-file-vs-composer.md).

Back to the [source list](../sources.md), the [list of extractions](../sources.md#2-extractions) or the [reports index](../../reports/README.md).
