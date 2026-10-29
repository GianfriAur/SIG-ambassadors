# `httparchive.crawl`, CMS versions

**Source:** [HTTP Archive](../sources.md#14-http-archive) through [Google BigQuery](../sources.md#11-google-bigquery). [Open the dataset in the console](https://console.cloud.google.com/bigquery?p=httparchive&d=crawl&t=pages&page=table).

**data freshness: 2026-08-17 23:14:05.000 UTC, last check 2026-09-01 16:00:00.000 UTC**

The `technologies` record carries a third field alongside the name and the categories: `info`, which holds whatever extra Wappalyzer could read, version numbers included. So the crawl can say not only that a site runs WordPress, but which WordPress.

That matters for a question the other extractions cannot touch: PHP applications are blamed for being out of date, and this is the one source that can measure it across millions of sites instead of asking people.

### Which platforms expose a version

Wappalyzer only fills `info` when its detection rule carries a `\;version:` suffix. Reading the [HTTP Archive fork of the definitions](https://github.com/HTTPArchive/wappalyzer/tree/main/src/technologies):

| Technology | Version rule | Read from |
|---|---|---|
| `WordPress` | yes | `meta` |
| `Drupal` | yes | `meta`, `headers` |
| `Joomla` | yes | `meta`, `headers` |
| `Magento` | yes | `scriptSrc`, `dom` |
| `WooCommerce` | yes | `meta`, `scriptSrc` |
| `phpMyAdmin` | yes | `html` |
| `TYPO3 CMS` | no | |
| `PrestaShop` | no | |
| `Moodle` | no | |

WordPress is read from the generator tag:

```
"generator": "^WordPress(?: ([\\d.]+))?\;version:\\1"
```

which matches `<meta name="generator" content="WordPress 6.4.2">`.

> [!warning]
> That tag is removable, and removing it is standard security advice: announcing the version tells a scanner which exploits to try. Security plugins and managed hosts strip it by default, and Drupal's `X-Generator` header goes the same way.
>
> So `info` will be empty for a substantial share of sites, and **the missing share is not random**: it is the carefully administered installations. Whatever version distribution comes back is biased toward the sites nobody is looking after, which are also the ones most likely to be behind. A report that ignores this would overstate how out of date the ecosystem is.

The share of sites that do expose a version is worth reporting on its own, as a measure of how the ecosystem handles a basic hardening step.

### Method

One crawl, mobile, root pages only, matching the conventions of the other HTTP Archive extractions so the site counts line up with [`httparchive.crawl`, PHP applications](httparchive-php-applications.md).

`info` is an array, so the version is pulled as the first element that starts with a digit. Wappalyzer sometimes puts other metadata in there.

## Query 1: how much of the version data exists

Run this first. If coverage is low the second query answers a different question than it appears to.

```sql
WITH detections AS (
  SELECT
    page,
    t.technology,
    -- info is an array and does not only hold versions, so take the first
    -- element that looks like one.
    (SELECT i FROM UNNEST(t.info) AS i WHERE REGEXP_CONTAINS(i, r'^\d') LIMIT 1) AS version
  FROM `httparchive.crawl.pages`, UNNEST(technologies) AS t
  WHERE date = '2025-07-01'   -- partitioning column, each extra date costs a full scan
    AND client = 'mobile'     -- one config is enough for a share, both would double-count
    AND is_root_page          -- without this every origin counts twice
    AND t.technology IN ('WordPress', 'Drupal', 'Joomla', 'Magento',
                         'WooCommerce', 'phpMyAdmin')
)

SELECT
  technology,
  COUNT(*)                     AS sites,
  COUNTIF(version IS NOT NULL) AS with_version,
  ROUND(100 * COUNTIF(version IS NOT NULL) / COUNT(*), 2) AS pct_with_version
FROM detections
GROUP BY technology
ORDER BY sites DESC;
```

## Query 2: the version distribution

```sql
-- Highest major version each project had actually published by the crawl date.
-- Anything above its ceiling is a spoofed or mis-parsed generator string, and is
-- flagged rather than dropped so the report can say how much of it there is.
DECLARE ceilings ARRAY<STRUCT<technology STRING, max_major INT64>> DEFAULT [
  STRUCT('WordPress'   AS technology, 7  AS max_major),
  STRUCT('Drupal',      11),
  STRUCT('Joomla',      5),
  STRUCT('Magento',     2),
  STRUCT('WooCommerce', 10)
];

WITH detections AS (
  SELECT
    page,
    t.technology,
    (SELECT i FROM UNNEST(t.info) AS i WHERE REGEXP_CONTAINS(i, r'^\d') LIMIT 1) AS version
  FROM `httparchive.crawl.pages`, UNNEST(technologies) AS t
  WHERE date = '2025-07-01'
    AND client = 'mobile'
    AND is_root_page
    AND t.technology IN ('WordPress', 'Drupal', 'Joomla', 'Magento', 'WooCommerce')
),

parsed AS (
  SELECT
    technology,
    -- Drupal publishes its major alone, as `Drupal 10`, so the minor has to be
    -- optional. Requiring `\d+\.\d+` discards almost every Drupal site.
    SAFE_CAST(REGEXP_EXTRACT(version, r'^(\d+)')      AS INT64) AS major,
    SAFE_CAST(REGEXP_EXTRACT(version, r'^\d+\.(\d+)') AS INT64) AS minor
  FROM detections
  WHERE version IS NOT NULL
)

SELECT
  p.technology,
  p.major,
  p.minor,                                  -- null where only a major was published
  p.major <= c.max_major AS plausible,
  COUNT(*) AS sites
FROM parsed AS p
JOIN UNNEST(ceilings) AS c ON c.technology = p.technology
WHERE p.major IS NOT NULL
GROUP BY technology, major, minor, plausible
ORDER BY technology, sites DESC;
```

Three things changed from the obvious version of this query, each because the first run got them wrong.

**The minor is optional.** Drupal's `X-Generator` header reads `Drupal 10 (https://www.drupal.org)`, so the version Wappalyzer records is `10` with no minor at all. A pattern requiring `\d+\.\d+` matched 26 Drupal sites out of 132,886.

**Implausible versions are flagged, not dropped.** The crawl contains `WordPress 99.99`, `999.9` and `13.37`, which are generator tags edited to mislead scanners, and `WooCommerce 438.3301` and `1163.3832`, which are not versions at all. Dropping them silently would hide how common the practice is; it is 0.004% for WordPress and far higher for WooCommerce.

**Nothing is filtered to a single detection source.** WooCommerce's contamination comes from its `scriptSrc` rule picking up a `?ver=` query string belonging to some other asset, and `info` does not record which rule produced it, so the source cannot be filtered in SQL. The ceiling is the only lever available, and it is a blunt one: a junk value that happens to land under the ceiling is indistinguishable from a real version.

Patch releases are left out of the grouping. Between 6.4.2 and 6.4.3 there is no decision by whoever runs the site.

## Results

**crawl used: 2025-07-01, mobile, root pages only. latest crawl available: 2026-08-17. query run: 2026-10-27**

The figures are in [Which version of WordPress, Drupal and Magento is actually running](../../reports/cms-versions.md).

Coverage turned out far better than the removable generator tag suggested: 84.69% for WooCommerce, 81.23% for Drupal, 67.96% for Magento and 64.17% for WordPress. Joomla is the exception at 3.76%, and that is a property of how Joomla is detected rather than of how its administrators behave.

The crawl carries version numbers but not release dates, so calling a version current or expired means bringing those in from the projects themselves. The report does that by hand for the handful of branches that matter.

Back to the [source list](../sources.md), the [list of extractions](../sources.md#2-extractions) or the [reports index](../../reports/README.md).
