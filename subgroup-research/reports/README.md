# Research reports

The results of the raw-data extractions, one file per research question.

> Most of these are assumed metrics and should not be treated as final. The queries behind them still need to be reasoned and refined. Where a report carries real query output it says so at the top, with the date of the run.

| Report | Question | Built from |
|---|---|---|
| [PHP's share of Stack Overflow questions](php-share-of-stackoverflow.md) | How has the volume of PHP questions, and PHP's share of all questions, moved over time? | [`stackoverflow-sede`](../raw-data/extractions/stackoverflow-sede.md), [`stackoverflow-bigquery`](../raw-data/extractions/stackoverflow-bigquery.md) |
| [Composer adoption across public PHP repositories](composer-adoption.md) | How many public PHP repositories take part in the Composer ecosystem, and how do they lay their projects out? | [`github-repos`](../raw-data/extractions/github-repos.md) |
| [PHP on the public web](php-on-the-public-web.md) | What share of the sites the HTTP Archive crawls is served by PHP, and which way is it moving? | [`httparchive-crawl`](../raw-data/extractions/httparchive-crawl.md) |
| [The PHP applications behind the web](php-applications-on-the-web.md) | Half the web runs PHP, but running what? Which named applications account for it, and how much of the CMS and ecommerce layer do they hold? | [`httparchive-php-applications`](../raw-data/extractions/httparchive-php-applications.md) |
| [Top 10 long running PHP](top-long-running-php.md) | Which PHP projects have been running longest and are still running now? | [`github-php-repositories`](../raw-data/extractions/github-php-repositories.md), [`github-php-longevity`](../raw-data/extractions/github-php-longevity.md) |
| [The licences of the PHP ecosystem](php-licences.md) | Under what terms is PHP published, and can a company pick a PHP library off the shelf? | [`github-repos-licences`](../raw-data/extractions/github-repos-licences.md) |
| [LICENSE against composer.json](licence-file-vs-composer.md) | How often do the two places a PHP project states its licence disagree, and how much of PHP does a count built on one of them miss? | [`licence-file-vs-composer`](../raw-data/extractions/licence-file-vs-composer.md) |

Each report lists the extractions it was built from, and each extraction links back to the reports that use it.

## Related

* [Raw data sources](../raw-data/sources.md): the tools, the datasets and the queries
* [PHP usage resource compilation](../php-usage-resource-compilation.md): published research and surveys by others
* [Open questions](../questions.md)
