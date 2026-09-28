# `bigquery-public-data.github_repos`

**Source:** [Google BigQuery](../sources.md#11-google-bigquery). [Open the dataset in the console](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=github_repos&t=files&page=table).

**data freshness: 2025-10-14 05:11:35.999 UTC, last check 2026-09-01 16:00:00.000 UTC**

A snapshot of the open source contents of GitHub, loaded into BigQuery and queryable in SQL. It is not a complete mirror of GitHub: only repositories that GitHub classifies as open source through its License API are included, and for file contents only text files under 10 MB.

It is suited to questions such as "how many projects import this library", "how is this API actually used in real code", "what is the licence distribution by language".

### Table structure

| Table | Granularity | Main fields |
|---|---|---|
| `files` | one row per file path | `repo_name`, `ref`, `path`, `mode`, `id`, `symlink_target` |
| `contents` | one row per unique content | `id`, `size`, `content`, `binary`, `copies` |
| `commits` | one row per commit | `commit`, `tree`, `parent`, `author`, `committer`, `subject`, `message`, `difference`, `repo_name` |
| `languages` | one row per repository | `repo_name`, `language` (array of `{name, bytes}`) |
| `licenses` | one row per repository | `repo_name`, `license` |
| `sample_*` | reduced subsets of the tables above | same schemas |

> [!important]
> the query reported here needs to be reasoned and refined

```sql
WITH
-- All repos GitHub Linguist tags as containing PHP.
php_repos AS (
  SELECT DISTINCT repo_name
  FROM `bigquery-public-data.github_repos.languages`, UNNEST(language) AS lang
  WHERE lang.name = 'PHP'
),

-- Single scan of the files table: keep only Composer-related paths.
-- vendor/autoload.php is the reliable marker of a committed vendor directory.
composer_files AS (
  SELECT
    repo_name,
    path,
    (path = 'composer.json' OR ENDS_WITH(path, '/composer.json')) AS is_manifest,
    (STARTS_WITH(path, 'vendor/') OR CONTAINS_SUBSTR(path, '/vendor/')) AS is_in_vendor
  FROM `bigquery-public-data.github_repos.files`
  WHERE path = 'composer.json'
     OR ENDS_WITH(path, '/composer.json')
     OR ENDS_WITH(path, 'vendor/autoload.php')
),

-- Collapse to one row per repo with the three signals we care about.
repo_signals AS (
  SELECT
    repo_name,
    LOGICAL_OR(path = 'composer.json')             AS has_root_manifest,
    LOGICAL_OR(is_manifest AND NOT is_in_vendor)   AS has_own_manifest,
    LOGICAL_OR(is_in_vendor)                       AS has_vendor_committed
  FROM composer_files
  GROUP BY repo_name
),

-- Left join so repos with no Composer footprint at all survive as NULLs,
-- then normalise those NULLs to FALSE so every repo lands on one side.
classified AS (
  SELECT
    p.repo_name,
    IFNULL(s.has_root_manifest,    FALSE) AS has_root_manifest,
    IFNULL(s.has_own_manifest,     FALSE) AS has_own_manifest,
    IFNULL(s.has_vendor_committed, FALSE) AS has_vendor_committed
  FROM php_repos AS p
  LEFT JOIN repo_signals AS s ON s.repo_name = p.repo_name
)

SELECT
  COUNT(*) AS total_php_repos,

  -- Bottom line: does the project ship a manifest of its own?
  COUNTIF(has_own_manifest)       AS uses_composer,
  COUNTIF(NOT has_own_manifest)   AS no_composer,

  -- Manifest present but not at the repo root: likely a layout problem,
  -- or a legitimate monorepo. Cannot be told apart from paths alone.
  COUNTIF(has_own_manifest AND NOT has_root_manifest) AS manifest_not_at_root,

  -- The classic mistake: vendor/ committed to version control.
  COUNTIF(has_vendor_committed)                          AS vendor_committed,
  COUNTIF(has_vendor_committed AND NOT has_own_manifest) AS vendor_committed_without_manifest,

  ROUND(100 * COUNTIF(has_own_manifest) / COUNT(*), 2)                          AS pct_uses_composer,
  ROUND(100 * COUNTIF(has_vendor_committed) / NULLIF(COUNTIF(has_own_manifest), 0), 2)
                                                                                AS pct_vendor_among_users
FROM classified
```

## Results

The numbers this query returned are in [Composer adoption across public PHP repositories](../../reports/composer-adoption.md).

Back to the [source list](../sources.md), the [list of extractions](../sources.md#2-extractions) or the [reports index](../../reports/README.md).
