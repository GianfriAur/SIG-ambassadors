# Lists of possible sources for raw data extraction

> This is a first draft and should not be considered an exhaustive list.

## Scope

This document aims to list the possible data sources and the ways to access them and the extractions that can be made for the various sources.

## 1 Tools/Sources

* Google BigQuery: [Google BigQuery](https://cloud.google.com/bigquery)
* Stack Exchange [Data Explorer](https://data.stackexchange.com)
* Ecosyste.ms: [Repos](https://repos.ecosyste.ms/)
* HTTP Archive: [httparchive.org](https://httparchive.org/)
* GitHub: [REST API](https://docs.github.com/en/rest)

### 1.1 Google BigQuery

Google BigQuery is usage-based: you pay only for the volume of data your queries actually read, and the first terabyte per month is free, which is enough for exploratory work and for analyses of moderate size.

### 1.2 Stack Exchange Data Explorer

Stack Exchange Data Explorer is a web interface that allows you to query a database of public Stack Exchange data, such as questions, answers, comments, and user activity. This database consists of a wide range of data from various Stack Exchange sites, such as Stack Overflow, Server Fault, Super User, and others. Data Explorer is free and requires no subscription. It's also the primary source of this information, and updates are logged weekly.

### 1.3 Ecosyste.ms Repos

Ecosyste.ms Repos is an open API of repository metadata that covers many code hosts and not only GitHub: checked on 2026-09-28 it indexed 348 million repositories across 2,037 host instances, among them 1,098 separate GitLab installations, Bitbucket, Gitea and Forgejo. Alongside the repositories it holds their owners, tags, releases and 413 million parsed dependency manifests. It is free, needs no account and no key, and is rate limited to 5,000 requests an hour for anonymous callers. The code is AGPL-3 and the data CC BY-SA 4.0, so published figures need attribution.

What it adds next to BigQuery is that it parses manifests instead of file paths, so `/api/v1/usage/packagist` returns a dependent count per Composer package without any query over file contents, and that it reaches the hosts `bigquery-public-data.github_repos` cannot see.

The bulk downloads are not a substitute for the API. The open data page lists three releases, the most recent from 2023-08-30 at roughly 168 million repositories against the 348 million the API reports now. Anything current has to be paged out of the API rather than queried in SQL, which makes it slower to work with.

### 1.4 HTTP Archive

HTTP Archive is the project that produces the `httparchive.crawl` dataset queried in §2. Every month it loads millions of sites with WebPageTest, in a desktop and a mobile configuration, and records what each one serves: resources, response headers, Lighthouse results, Blink features and the technologies Wappalyzer detects. The list of sites comes from CrUX, so it is the origins real Chrome users visit and not a ranking of anything. Code and data are both free and open.

Three different things come out of the project, and it helps to keep them apart.

* The BigQuery dataset, which is where our own queries go. See [`httparchive.crawl`](extractions/httparchive-crawl.md).
* The [reports](https://httparchive.org/reports) on the site, which are ready-made time series, and the [Core Web Vitals Technology Report](https://httparchive.org/reports/techreport/tech), which compares adoption and performance per technology without writing any SQL.
* The [Web Almanac](https://almanac.httparchive.org/), a book-length analysis published once a year with the SQL behind every figure.

#### Web Almanac chapters that bear on PHP

The CMS and Ecommerce chapters are the two worth reading, because most of the platforms they count are PHP applications. There was no 2023 edition, and the 2022 edition has no Ecommerce chapter.

| Year | CMS | Ecommerce |
|---|---|---|
| 2025 | [CMS](https://almanac.httparchive.org/en/2025/cms) | [Ecommerce](https://almanac.httparchive.org/en/2025/ecommerce) |
| 2024 | [CMS](https://almanac.httparchive.org/en/2024/cms) | [Ecommerce](https://almanac.httparchive.org/en/2024/ecommerce) |
| 2022 | [CMS](https://almanac.httparchive.org/en/2022/cms) | not published |
| 2021 | [CMS](https://almanac.httparchive.org/en/2021/cms) | [Ecommerce](https://almanac.httparchive.org/en/2021/ecommerce) |
| 2020 | [CMS](https://almanac.httparchive.org/en/2020/cms) | [Ecommerce](https://almanac.httparchive.org/en/2020/ecommerce) |
| 2019 | [CMS](https://almanac.httparchive.org/en/2019/cms) | [Ecommerce](https://almanac.httparchive.org/en/2019/ecommerce) |

The 2025 edition reports that CMS platforms serve over 54% of the sites observed, that WordPress alone takes 64.3% of CMS usage and around 35.6% of all sites, and that Joomla and Drupal each sit below 2%. On the commerce side, 19.2% of mobile sites are shops, WooCommerce leads with 35.4% of them against Shopify at 21.5%, and PrestaShop takes 3.2%. Of the five platforms the chapter names, WooCommerce and PrestaShop are PHP and the other three are not.

Every figure in those chapters has its query published in the almanac repository under `sql/<year>/<chapter>/`, which is where the extraction in §2.5 started from.

### 1.5 GitHub REST API

The GitHub REST API answers without an account or a key, which is what makes it usable here. The two endpoints that matter are `/search/repositories`, which filters and sorts the whole of GitHub, and `/repos/{owner}/{name}`, which returns one repository in full. Both carry `stargazers_count`, `created_at`, `pushed_at`, `archived`, `fork` and the licence, so a population and its star counts come out of the same call.

Anonymous callers get 10 search requests a minute, which is the binding constraint, and 60 a minute on the other endpoints. A search returns at most 1,000 results however many matched, so a population larger than that has to be collected in windows narrow enough to stay under the cap. PHP repositories with at least 1,000 stars are 1,553, and splitting them at 2,000 stars gives 800 and 753.

It covers ground BigQuery cannot. `bigquery-public-data.github_repos` holds no star counts and its snapshot is a year old, while the API is live. What it cannot do is look inside a repository at scale: for file contents and commit history the BigQuery dataset is still the only practical route.

> [!warning]
> `created_at` is the date the repository was created **on GitHub**, not the date the project started. `phpmyadmin/phpmyadmin` reports 2012-01-19 against a project that began in 1998, because the code lived in CVS and then SourceForge first. Every project that predates GitHub is understated the same way, and those are exactly the projects a question about longevity is asking about. The first commit in `bigquery-public-data.github_repos.commits` is the better floor, since conversions from CVS and SVN carry their history with them.

Packagist dates are worse rather than better. Through [Ecosyste.ms](#13-ecosystems-repos), `phpmyadmin/phpmyadmin` reports `first_release_published_at` of 2019-07-09, which is when the Composer package was registered and says nothing at all about the project.

## 2 Extractions

One file per extraction, in [`extractions/`](extractions/). Each one describes the dataset, its table structure and the query run against it. The numbers those queries returned are in the [reports](#3-reports).

| Extraction | Tool | Data freshness | Covers |
|---|---|---|---|
| [`bigquery-public-data.github_repos`](extractions/github-repos.md) | [BigQuery](#11-google-bigquery) | 2025-10-14 | Contents, commit history and metadata for millions of public GitHub repositories, including full source files for a licensed subset. Open-licensed repositories only, not a complete mirror of GitHub. |
| [`bigquery-public-data.stackoverflow`](extractions/stackoverflow-bigquery.md) | [BigQuery](#11-google-bigquery) | 2022-11-25 | Full archive of Stack Overflow questions, answers, comments, tags and user activity, with timestamps. Loaded as a periodic dump and not refreshed since late 2022, so recent years are missing. |
| [`httparchive.crawl`](extractions/httparchive-crawl.md) | [BigQuery](#11-google-bigquery) | 2026-08-17 | The monthly HTTP Archive crawl: `pages`, one row per page tested, carrying detected technologies, Lighthouse results and a page-level summary; and `requests`, one row per resource loaded. Tens to hundreds of terabytes per crawl, so queries must filter on the `date` partition and select only the fields needed. |
| [`stack-exchange-data-explorer.stackoverflow`](extractions/stackoverflow-sede.md) | [Data Explorer](#12-stack-exchange-data-explorer) | 2026-08-29 | The same archive as the BigQuery copy above, but updated from Stack Exchange every week, so it should be considered the aligned source. |
| [`httparchive.crawl`, PHP applications](extractions/httparchive-php-applications.md) | [BigQuery](#11-google-bigquery) | 2026-08-17 | The same crawl table again, counting the sites that run a named PHP application (WordPress, WooCommerce, Drupal, Magento and the rest) instead of the bare `PHP` flag. |
| [GitHub PHP repositories](extractions/github-php-repositories.md) | [GitHub REST API](#15-github-rest-api) | live | Every repository GitHub classifies as PHP with at least 1,000 stars, 1,553 of them, with star counts and the creation and last-push dates. |
| [GitHub PHP longevity](extractions/github-php-longevity.md) | [GitHub REST API](#15-github-rest-api) | live | The population above, filtered to what is still active and ranked by `months × log10(stars)`. |

Note that `bigquery-public-data.stackoverflow` and `stack-exchange-data-explorer.stackoverflow` cover the same data. The Data Explorer one is the current copy; the BigQuery one is kept because a lot of published analysis is built on it.

## 3 Reports

The results, one file per research question, in [`../reports/`](../reports/). The figures are a first draft: the queries behind them still need to be reasoned and refined.

| Report | Question | Built from |
|---|---|---|
| [PHP's share of Stack Overflow questions](../reports/php-share-of-stackoverflow.md) | How has the volume of PHP questions, and PHP's share of all questions, moved over time? | [`stackoverflow-sede`](extractions/stackoverflow-sede.md), [`stackoverflow-bigquery`](extractions/stackoverflow-bigquery.md) |
| [Composer adoption across public PHP repositories](../reports/composer-adoption.md) | How many public PHP repositories take part in the Composer ecosystem, and how do they lay their projects out? | [`github-repos`](extractions/github-repos.md) |
| [PHP on the public web](../reports/php-on-the-public-web.md) | What share of the sites the HTTP Archive crawls is served by PHP, and which way is it moving? | [`httparchive-crawl`](extractions/httparchive-crawl.md) |
| [The PHP applications behind the web](../reports/php-applications-on-the-web.md) | Half the web runs PHP, but running what? Which named applications account for it, and how much of the CMS and ecommerce layer do they hold? | [`httparchive-php-applications`](extractions/httparchive-php-applications.md) |
| [Top 10 long running PHP](../reports/top-long-running-php.md) | Which PHP projects have been running longest and are still running now? | [`github-php-repositories`](extractions/github-php-repositories.md), [`github-php-longevity`](extractions/github-php-longevity.md) |
