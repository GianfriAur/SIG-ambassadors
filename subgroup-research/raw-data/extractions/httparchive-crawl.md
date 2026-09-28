# `httparchive.crawl`

**Source:** [Google BigQuery](../sources.md#11-google-bigquery). [Open the dataset in the console](https://console.cloud.google.com/bigquery?p=httparchive&d=crawl&t=pages&page=table).

**data freshness: 2026-08-17 23:14:05.000 UTC, last check 2026-09-01 16:00:00.000 UTC**

The main HTTP Archive dataset, from the project that loads millions of sites with WebPageTest every month and archives their performance, detected technologies, structure and resources. It is the data behind the Web Almanac and the standard source for analyses of the state of the web.

The `crawl` dataset is the current structure: it replaces the old dated tables (`httparchive.pages.2023_06_01_desktop` and similar) and the `runs`, `har` and `all` datasets. Pre-2024 queries found online no longer work as written.

**Note the project**: this is not in `bigquery-public-data` but in the separate `httparchive` project. It is still part of the Public Dataset Program, so storage is free and only queries are billed.

### The two main tables

| Table | Granularity | Monthly size | History since |
|---|---|---|---|
| `crawl.pages` | one row per page tested | ~30 TB | June 2011 |
| `crawl.requests` | one row per resource loaded | ~199 TB | June 2011 |

Both are partitioned on `date`. `pages` is clustered on `client`, `is_root_page`, `rank`, `page`; `requests` on `client`, `is_root_page`, `type`, `rank`.

Each page is tested in two configurations, desktop and mobile, and since April 2022 both the origin's root page and one secondary page are tested.

### `crawl.pages` schema

| Field | Type | Contents |
|---|---|---|
| `date` | DATE | Monthly crawl date, always the first of the month |
| `client` | STRING | `desktop` or `mobile` |
| `page` | STRING | URL of the page tested |
| `is_root_page` | BOOLEAN | Whether the page is the root of the origin |
| `root_page` | STRING | URL of the root page, the origin followed by `/` |
| `rank` | INTEGER | Site popularity bucket, from CrUX |
| `wptid` | STRING | Identifier of the WebPageTest results |
| `payload` | JSON | Full WebPageTest results |
| `summary` | JSON | Summarisation of page-level metrics |
| `technologies` | repeated RECORD | Technologies detected by Wappalyzer |
| `custom_metrics` | RECORD | Custom metrics collected during the test |
| `lighthouse` | JSON | Lighthouse report |
| `features` | repeated RECORD | Blink features used by the page |
| `metadata` | JSON | Additional metadata about the test |

> [!important]
> the query reported here needs to be reasoned and refined

```sql
WITH sites AS (
  SELECT
    date,
    root_page,
    -- EXISTS avoids UNNEST in the FROM clause, which would emit one row
    -- per technology and break the site count.
    EXISTS(
      SELECT 1 FROM UNNEST(technologies) AS t
      WHERE t.technology = 'PHP'
    ) AS uses_php
  FROM `httparchive.crawl.pages`
  -- Each extra date multiplies the cost: date is the partitioning column.
  WHERE date IN ('2023-06-01', '2024-06-01', '2025-06-01')
    AND is_root_page       -- without this every origin counts twice
    AND client = 'mobile'  -- one config is enough for a share; both would double-count
)

SELECT
  date,
  COUNT(*) AS total_sites,
  COUNTIF(uses_php) AS php_sites,
  ROUND(100 * COUNTIF(uses_php) / COUNT(*), 2) AS share_pct
FROM sites
GROUP BY date
ORDER BY date;
```

## Results

The numbers this query returned are in [PHP on the public web](../../reports/php-on-the-public-web.md).

Back to the [source list](../sources.md), the [list of extractions](../sources.md#2-extractions) or the [reports index](../../reports/README.md).
