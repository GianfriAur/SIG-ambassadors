# Lists of possible sources for raw data extraction

> This is a first draft and should not be considered an exhaustive list.

## Scope

This document aims to list the possible data sources and the ways to access them and the extractions that can be made for the various sources.

## 1 Tools/Sources

* Google BigQuery: [Google BigQuery](https://cloud.google.com/bigquery)
* Stack Exchange [Data Explorer](https://data.stackexchange.com)

### 1.1 Google BigQuery

Google BigQuery is usage-based: you pay only for the volume of data your queries actually read, and the first terabyte per month is free, which is enough for exploratory work and for analyses of moderate size.

### 1.2 Stack Exchange Data Explorer

Stack Exchange Data Explorer is a web interface that allows you to query a database of public Stack Exchange data, such as questions, answers, comments, and user activity. This database consists of a wide range of data from various Stack Exchange sites, such as Stack Overflow, Server Fault, Super User, and others. Data Explorer is free and requires no subscription. It's also the primary source of this information, and updates are logged weekly.

## 2 Extractions

One file per extraction, in [`extractions/`](extractions/). Each one describes the dataset, its table structure and the query run against it. The numbers those queries returned are in the [reports](#3-reports).

| Extraction | Tool | Data freshness | Covers |
|---|---|---|---|
| [`bigquery-public-data.github_repos`](extractions/github-repos.md) | [BigQuery](#11-google-bigquery) | 2025-10-14 | Contents, commit history and metadata for millions of public GitHub repositories, including full source files for a licensed subset. Open-licensed repositories only, not a complete mirror of GitHub. |
| [`bigquery-public-data.stackoverflow`](extractions/stackoverflow-bigquery.md) | [BigQuery](#11-google-bigquery) | 2022-11-25 | Full archive of Stack Overflow questions, answers, comments, tags and user activity, with timestamps. Loaded as a periodic dump and not refreshed since late 2022, so recent years are missing. |
| [`httparchive.crawl`](extractions/httparchive-crawl.md) | [BigQuery](#11-google-bigquery) | 2026-08-17 | The monthly HTTP Archive crawl: `pages`, one row per page tested, carrying detected technologies, Lighthouse results and a page-level summary; and `requests`, one row per resource loaded. Tens to hundreds of terabytes per crawl, so queries must filter on the `date` partition and select only the fields needed. |
| [`stack-exchange-data-explorer.stackoverflow`](extractions/stackoverflow-sede.md) | [Data Explorer](#12-stack-exchange-data-explorer) | 2026-08-29 | The same archive as the BigQuery copy above, but updated from Stack Exchange every week, so it should be considered the aligned source. |

Note that `bigquery-public-data.stackoverflow` and `stack-exchange-data-explorer.stackoverflow` cover the same data. The Data Explorer one is the current copy; the BigQuery one is kept because a lot of published analysis is built on it.

## 3 Reports

The results, one file per research question, in [`../reports/`](../reports/). The figures are a first draft: the queries behind them still need to be reasoned and refined.

| Report | Question | Built from |
|---|---|---|
| [PHP's share of Stack Overflow questions](../reports/php-share-of-stackoverflow.md) | How has the volume of PHP questions, and PHP's share of all questions, moved over time? | [`stackoverflow-sede`](extractions/stackoverflow-sede.md), [`stackoverflow-bigquery`](extractions/stackoverflow-bigquery.md) |
| [Composer adoption across public PHP repositories](../reports/composer-adoption.md) | How many public PHP repositories take part in the Composer ecosystem, and how do they lay their projects out? | [`github-repos`](extractions/github-repos.md) |
| [PHP on the public web](../reports/php-on-the-public-web.md) | What share of the sites the HTTP Archive crawls is served by PHP, and which way is it moving? | [`httparchive-crawl`](extractions/httparchive-crawl.md) |
