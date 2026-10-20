# The first two years of a Packagist package

**Source:** [Ecosyste.ms](../sources.md#13-ecosystems-repos) for the population, Packagist for the download history.

**data freshness: live, both APIs answer from current state**

Collects the weekly download series for the first 104 weeks of a package's life, graded by how much the package ended up being used, so that the early weeks of adopted and unadopted packages can be compared.

t=0 is the package's **first release on Packagist**, not the repository's first commit. The question is when adoption starts, and adoption cannot start before there is something to install.

### Grading

Success is measured on two axes and a package takes the better of the two, because a library and a tool are used in different ways and no field in the registry tells them apart.

The obvious candidates both fail. `type` is declared by convention and the conventions disagree: `composer/composer` says `library`, `laravel/pint` says `project`, `friendsofphp/php-cs-fixer` says `application`. A `bin` entry looks more promising, since it declares an executable, but plenty of libraries ship an auxiliary command: among the 230 packages with a `bin` in the thousand most downloaded are `symfony/yaml`, `nikic/php-parser`, `symfony/var-dumper` and `league/commonmark`, none of which anyone installs as a tool. The ratio of downloads to dependents fails too, because a deeply transitive library and a directly installed tool produce the same signature: `monolog/monolog` sits above `composer/composer` on it.

Dependent counts do not separate them either, since a dev tool collects dependents from every `require-dev` that names it: `phpunit/phpunit` has 122,193.

So no label is assigned. A package counts as adopted if it clears either threshold, and the analysis asks whether early behaviour differs by where a package sits on the two axes rather than by a category imposed beforehand.

| Grade | Rule |
|---|---|
| A, adopted | 25 or more direct dependents **or** 100,000 downloads a month |
| B, partly adopted | 3 to 24 dependents **or** 1,000 to 99,999 downloads a month |
| C, not adopted | no dependents **and** under 100 downloads a month **and** at least 10 releases |

Download rates are per month over the package's whole life, so a package from 2014 is not credited for having had longer to accumulate.

The gaps between the grades are deliberate. A package with one or two dependents belongs to neither group, and C requires at least 10 releases so that the comparison is against projects that were maintained rather than against abandoned experiments.

> [!warning]
> Grade C turned out to be less useful than expected. Its median package has **87 of 104 weeks at zero downloads**, and 277 of 368 are at zero for at least half the period. These are not projects that tried and failed; they are projects nobody ever installed. Comparing them against adopted packages proves little. The live comparison is A against B.

### Eligibility

| Filter | Why |
|---|---|
| first release between 2013-01-01 and 2024-10-01 | Packagist daily download history starts in 2013, and 104 observable weeks must fit before today |
| has a `repository_url` | needed to join with commit and star history later |
| not a platform requirement | `php`, `ext-*`, `lib-*`, `composer-*` are not packages |
| not `bower-asset/*` or `npm-asset/*` | bridges to other ecosystems |

### Building the cohort

The grades cannot all be sampled the same way. A random page of the registry reaches grade C easily, since 81% of Packagist packages have no dependents at all and the median package is downloaded twice a month. It almost never reaches grade A: in a random sample of 3,900 packages the highest dependent count observed was 62, while `monolog/monolog` has 6,809.

So the top grades are taken from the registry sorted by each axis, and the bottom grade from random pages.

### Script

`cohort.php`, run with `php -d memory_limit=2G cohort.php`. Requests run twelve at a time through `curl_multi`, and each response is consumed and discarded rather than accumulated: holding the whole registry page set in memory exhausts the default 128 MB limit.

> [!warning]
> The depth of the sorted read is a parameter that changes the result, not just its precision. Taking only the first ranks of each axis fills grade A with the extreme top of the registry, where opening download levels are an order of magnitude above the rest, and every gap the analysis reports widens accordingly. This script reads 10,000 ranks per axis, which is roughly the rank at which a direct dependent count falls to 25, so that grade A holds ordinary adopted packages rather than only famous ones. Anyone changing `$page <= 100` should expect the conclusions to move with it.

```php
<?php
declare(strict_types=1);

/**
 * Builds the study cohort and collects the first 104 weeks of downloads.
 *
 * Success is measured on two axes rather than assigned by category, because no
 * field in the registry reliably separates a library from a tool: PHPUnit is a
 * tool with 122,193 dependents, and symfony/var-dumper is a library that ships
 * a binary. Each package is placed on both axes and graded by the better of the
 * two, so a project adopted as a dependency and one installed directly can both
 * count as used.
 */

const PKG_API  = 'https://packages.ecosyste.ms/api/v1/registries/packagist.org/packages';
const STATS    = 'https://packagist.org/packages/%s/stats/all.json?average=daily&from=%s&to=%s';
const UA       = 'sig-ambassadors-research';

const WINDOW_FROM = '2013-01-01';   // Packagist daily download history starts here
const WINDOW_TO   = '2024-10-01';   // leaves 104 observable weeks
const WEEKS       = 104;
const PER_GRADE   = 400;
const PARALLEL    = 8;

// Platform requirements and bridges to other ecosystems, not PHP packages.
const PSEUDO = '/^(php$|ext-|lib-|composer-|bower-asset\/|npm-asset\/)/';

/**
 * Fetches urls in parallel and hands each decoded response to $consume.
 * Nothing is accumulated: a full pass over the registry does not fit in memory.
 */
function fetchEach(array $urls, callable $consume, int $parallel = PARALLEL): void
{
    foreach (array_chunk($urls, $parallel, true) as $chunk) {
        $multi = curl_multi_init();
        $handles = [];
        foreach ($chunk as $key => $url) {
            $h = curl_init($url);
            curl_setopt_array($h, [
                CURLOPT_RETURNTRANSFER => true,
                CURLOPT_USERAGENT      => UA,
                CURLOPT_TIMEOUT        => 30,
                CURLOPT_FOLLOWLOCATION => true,
            ]);
            curl_multi_add_handle($multi, $h);
            $handles[$key] = $h;
        }
        do {
            curl_multi_exec($multi, $running);
            curl_multi_select($multi, 0.5);
        } while ($running > 0);

        foreach ($handles as $key => $h) {
            $body = curl_multi_getcontent($h);
            $consume($key, $body === false ? null : json_decode($body, true));
            curl_multi_remove_handle($multi, $h);
            curl_close($h);
            unset($body);
        }
        curl_multi_close($multi);
        gc_collect_cycles();
    }
}

/** Only the fields the study uses, so the pool stays small. */
function slim(array $p): array
{
    return [
        'name'                     => $p['name'],
        'first_release_published_at' => $p['first_release_published_at'] ?? null,
        'repository_url'           => $p['repository_url'] ?? null,
        'dependent_packages_count' => $p['dependent_packages_count'] ?? 0,
        'downloads'                => $p['downloads'] ?? 0,
        'versions_count'           => $p['versions_count'] ?? 0,
    ];
}

function ageMonths(string $firstRelease): float
{
    $start = new DateTimeImmutable(substr($firstRelease, 0, 10));
    $now   = new DateTimeImmutable('2026-10-20');
    return max(1.0, (float) $start->diff($now)->days / 30.44);
}

function eligible(array $p): bool
{
    $first = $p['first_release_published_at'] ?? null;
    if ($first === null || preg_match(PSEUDO, (string) $p['name'])) {
        return false;
    }
    $date = substr($first, 0, 10);
    return $date >= WINDOW_FROM && $date <= WINDOW_TO && !empty($p['repository_url']);
}

/** A on either axis wins; C requires failing both and still having been maintained. */
function grade(array $p): ?string
{
    $dep  = (int) ($p['dependent_packages_count'] ?? 0);
    $rate = (int) (($p['downloads'] ?? 0) / ageMonths($p['first_release_published_at']));
    $vers = (int) ($p['versions_count'] ?? 0);

    if ($dep >= 25   || $rate >= 100000) return 'A';
    if ($dep >= 3    || $rate >= 1000)   return 'B';
    if ($dep === 0 && $rate < 100 && $vers >= 10) return 'C';
    return null;   // the gaps between grades are left out on purpose
}

// ---------------------------------------------------------------- cohort
fwrite(STDERR, "building the pool\n");
$pool = [];
foreach (['dependent_packages_count', 'downloads'] as $sort) {
    $urls = [];
    for ($page = 1; $page <= 15; $page++) {
        $urls[] = PKG_API . "?sort={$sort}&order=desc&per_page=100&page={$page}";
    }
    fetchEach($urls, function ($_, $batch) use (&$pool) {
        foreach ($batch ?? [] as $p) {
            $pool[$p['name']] = slim($p);
        }
    });
    fwrite(STDERR, "  after {$sort}: " . count($pool) . "\n");
}
// The low grade is dense everywhere, so random pages reach it.
$urls = [];
for ($i = 0; $i < 25; $i++) {
    $urls[] = PKG_API . '?per_page=100&page=' . random_int(1, 5180);
}
fetchEach($urls, function ($_, $batch) use (&$pool) {
    foreach ($batch ?? [] as $p) {
        $pool[$p['name']] = slim($p);
    }
});
fwrite(STDERR, '  pool: ' . count($pool) . "\n");

$graded = ['A' => [], 'B' => [], 'C' => []];
foreach ($pool as $p) {
    if (!eligible($p)) continue;
    $g = grade($p);
    if ($g !== null && count($graded[$g]) < PER_GRADE) {
        $p['grade'] = $g;
        $graded[$g][] = $p;
    }
}
foreach ($graded as $g => $list) {
    fwrite(STDERR, sprintf("  grade %s: %d\n", $g, count($list)));
}

// ---------------------------------------------------------------- downloads
$cohort = array_merge($graded['A'], $graded['B'], $graded['C']);
fwrite(STDERR, 'collecting ' . count($cohort) . " download series\n");

$urls = [];
foreach ($cohort as $i => $p) {
    $from = new DateTimeImmutable(substr($p['first_release_published_at'], 0, 10));
    $to   = $from->add(new DateInterval('P' . (WEEKS * 7) . 'D'));
    $urls[$i] = sprintf(STATS, $p['name'], $from->format('Y-m-d'), $to->format('Y-m-d'));
}

$result = [];
fetchEach($urls, function ($i, $payload) use (&$result, $cohort) {
    $p = $cohort[$i];
    $daily = $payload['values'] ?? null;
    if ($daily === null) return;
    $values = array_values($daily)[0] ?? [];
    $weekly = [];
    for ($w = 0; $w < WEEKS; $w++) {
        $slice = array_slice($values, $w * 7, 7);
        if (count($slice) < 7) break;
        $weekly[] = array_sum($slice);
    }
    $result[] = [
        'name'    => $p['name'],
        'grade'   => $p['grade'],
        't0'      => substr($p['first_release_published_at'], 0, 10),
        'repo'    => $p['repository_url'],
        'deps'    => (int) ($p['dependent_packages_count'] ?? 0),
        'rate'    => (int) (($p['downloads'] ?? 0) / ageMonths($p['first_release_published_at'])),
        'versions'=> (int) ($p['versions_count'] ?? 0),
        'weekly'  => $weekly,
    ];
});
file_put_contents('cohort_downloads.json', json_encode($result));
fwrite(STDERR, 'collected ' . count($result) . " series with " . WEEKS . " weeks each\n");
```

A second script, `vendors.php`, fetches how many packages each vendor in the cohort has published, one call per distinct vendor.

> [!important]
> That count is today's, not the count at the package's first release. A vendor that grew because of the package under study is credited retroactively, so the vendor figures carry a reverse-causality problem that this extraction does not solve.

## Results

**collected: 2026-10-20**

7,259 series of 104 complete weeks: 2,475 grade A, 2,441 grade B, 2,343 grade C. The figures are in [The first two years of a PHP package](../../reports/package-first-two-years.md).

Of the 2,475 grade A packages, 1,857 qualify on dependents alone with a download rate below the threshold. That subgroup is what makes the analysis non-circular: its label never looks at the download series it is used to explain.

Back to the [source list](../sources.md), the [list of extractions](../sources.md#2-extractions) or the [reports index](../../reports/README.md).
