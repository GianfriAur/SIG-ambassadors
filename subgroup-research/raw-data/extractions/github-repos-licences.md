# `bigquery-public-data.github_repos`, licences

**Source:** [Google BigQuery](../sources.md#11-google-bigquery). [Open the dataset in the console](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=github_repos&t=licenses&page=table).

**data freshness: 2025-10-14 05:11:35.999 UTC, last check 2026-09-01 16:00:00.000 UTC**

The same dataset as [`bigquery-public-data.github_repos`](github-repos.md), asked a different question: under what licence is PHP published? The `licenses` table carries one row per repository and `languages` carries the byte counts Linguist produced, so joining them gives the licence distribution across every PHP repository in the snapshot.

> [!warning]
> The dataset contains only repositories GitHub classifies as open source **through its License API**. A project with no licence file is not in it. Every figure this extraction produces is therefore a share of licensed projects, and the question "how many PHP projects have no licence" cannot be answered here at all.

### Method

Two definitions of "a PHP repository" are reported side by side, because they disagree by a third and the difference is not noise.

`php_is_the_main_language` takes the language holding the most bytes, which is the rule Linguist applies and the one GitHub reports as a repository's language. `contains_any_php` counts every repository with a line of PHP in it, which sweeps in projects whose licence was chosen for a JavaScript or Python codebase that happens to ship a `.php` file.

Both tables are small, so no partition filter is needed and the query is cheap.

> [!important]
> the query reported here needs to be reasoned and refined

```sql
WITH languages AS (
  SELECT
    repo_name,
    -- Linguist's own choice: the language holding the most bytes.
    (SELECT name FROM UNNEST(language) ORDER BY bytes DESC LIMIT 1) AS top_language,
    EXISTS(SELECT 1 FROM UNNEST(language) WHERE name = 'PHP')       AS contains_php
  FROM `bigquery-public-data.github_repos.languages`
)

SELECT
  l.license,
  COUNTIF(g.top_language = 'PHP') AS php_is_the_main_language,
  COUNTIF(g.contains_php)         AS contains_any_php,
  ROUND(100 * COUNTIF(g.top_language = 'PHP')
        / SUM(COUNTIF(g.top_language = 'PHP')) OVER (), 2) AS pct_of_main
FROM `bigquery-public-data.github_repos.licenses` AS l
JOIN languages AS g USING (repo_name)
WHERE g.contains_php
GROUP BY l.license
ORDER BY php_is_the_main_language DESC;
```

The licence strings are lowercase short names such as `mit` and `gpl-2.0`, not the SPDX identifiers the GitHub API returns.

### Families

Fifteen distinct licences come back, few enough to group every one by name. A rule matching prefixes would be shorter and would quietly absorb anything new that appeared later.

| Family | Licences |
|---|---|
| permissive | `mit`, `bsd-3-clause`, `apache-2.0`, `bsd-2-clause`, `unlicense`, `cc0-1.0`, `isc` |
| copyleft | `gpl-2.0`, `gpl-3.0`, `agpl-3.0`, `lgpl-3.0`, `lgpl-2.1`, `mpl-2.0`, `epl-1.0`, `artistic-2.0` |

`cc0-1.0` is a public domain dedication rather than a licence, and sits with the permissive family because that is how it behaves for anyone reusing the code. `artistic-2.0` and `epl-1.0` both impose conditions on redistributing modified versions, so both sit with the copyleft family.

## Results

**query run: 2026-10-02**

237,274 repositories where PHP is the main language, 339,426 containing any PHP. The figures are in [The licences of the PHP ecosystem](../../reports/php-licences.md).

The second figure matches the PHP repository total in [`bigquery-public-data.github_repos`](github-repos.md) exactly, which is a useful check that the join is counting what it claims.

Back to the [source list](../sources.md), the [list of extractions](../sources.md#2-extractions) or the [reports index](../../reports/README.md).
