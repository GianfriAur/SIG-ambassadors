# PHP on the public web

**crawls: 2023-06-01, 2024-06-01, 2025-06-01, mobile, root pages only. data freshness: 2026-08-17 23:14:05.000 UTC. query run: 2026-09-28**

> These are assumed metrics and should not be treated as final. The query behind them still needs to be reasoned and refined.

**Question:** what share of the sites the HTTP Archive crawls is served by PHP, and which way is it moving?

**Extractions:** [`httparchive.crawl`](../raw-data/extractions/httparchive-crawl.md)

## Results

| Crawl | Total sites | Sites with PHP | Share % |
|---|---:|---:|---:|
| 2023-06 | 16,563,413 | 9,099,650 | 54.94 |
| 2024-06 | 16,129,455 | 8,773,007 | 54.39 |
| 2025-06 | 15,545,137 | 8,208,463 | 52.80 |

The columns:

* **Crawl**: the month of the HTTP Archive crawl the row was taken from, one crawl a year.
* **Total sites**: how many origins that crawl tested in the mobile configuration, root pages only.
* **Sites with PHP**: how many of those origins Wappalyzer flagged as serving PHP.
* **Share %**: sites with PHP divided by total sites, as a percentage.

## How to read this

Including `desktop` as well as `mobile`, or the secondary pages alongside the root ones, would count most origins twice.

"Sites with PHP" means Wappalyzer detected PHP while the page was loading. It works from response headers, cookie names and URL patterns, and a site can hide all three, so the real figure is higher than the one shown here.

The total shrinks from one year to the next, 16.6M origins down to 15.5M. The set of sites crawled is not fixed, so the column to look at is the share and not the counts.

The same crawl is broken down by application in [The PHP applications behind the web](php-applications-on-the-web.md), which carries real query output rather than assumed metrics.

Back to the [reports index](README.md) or the [source list](../raw-data/sources.md).
