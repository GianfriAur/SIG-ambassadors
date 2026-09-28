# Research reports

The results of the raw-data extractions, one file per research question.

> These are assumed metrics and should not be treated as final. The queries behind them still need to be reasoned and refined.

| Report | Question | Built from |
|---|---|---|
| [PHP's share of Stack Overflow questions](php-share-of-stackoverflow.md) | How has the volume of PHP questions, and PHP's share of all questions, moved over time? | [`stackoverflow-sede`](../raw-data/extractions/stackoverflow-sede.md), [`stackoverflow-bigquery`](../raw-data/extractions/stackoverflow-bigquery.md) |
| [Composer adoption across public PHP repositories](composer-adoption.md) | How many public PHP repositories take part in the Composer ecosystem, and how do they lay their projects out? | [`github-repos`](../raw-data/extractions/github-repos.md) |
| [PHP on the public web](php-on-the-public-web.md) | What share of the sites the HTTP Archive crawls is served by PHP, and which way is it moving? | [`httparchive-crawl`](../raw-data/extractions/httparchive-crawl.md) |

Each report lists the extractions it was built from, and each extraction links back to the reports that use it.

## Related

* [Raw data sources](../raw-data/sources.md): the tools, the datasets and the queries
* [PHP usage resource compilation](../php-usage-resource-compilation.md): published research and surveys by others
* [Open questions](../questions.md)
