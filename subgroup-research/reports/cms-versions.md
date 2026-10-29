# Which version of WordPress, Drupal and Magento is actually running, 2025

**crawl used: 2025-07-01, mobile, root pages only. latest crawl available: 2026-08-17. query run: 2026-10-27**

**Question:** PHP applications carry a reputation for being left unpatched. The crawl can check it, because the version is often sitting in the page.

**Extractions:** [`httparchive.crawl`, CMS versions](../raw-data/extractions/httparchive-cms-versions.md)

The same measurement one year later is in [the 2026 crawl](cms-versions-2026.md).

## How many sites say which version they run

| Platform | Detected | Declares a version | Share |
|---|---:|---:|---:|
| Drupal | 163,595 | 132,886 | 81.23% |
| Magento | 51,936 | 35,294 | 67.96% |
| WordPress | 5,386,519 | 3,456,550 | 64.17% |
| Joomla | 199,188 | 7,480 | 3.76% |

Everything below is a share of the sites that declare, because the rest are unknown rather than old. Each table closes with the sites that could not be identified, counted against everything detected for that platform.

These four percentages do not all measure the same thing, and the difference is in how each platform is recognised. WordPress is detected five ways, from the generator tag, the `X-Pingback` header, the `api.w.org` link and two patterns in `/wp-content/` paths, but only the first carries a version. Its 64.17% therefore means what it looks like it means: **1.93 million WordPress sites are recognisable while having removed the version**, which is standard hardening advice.

Joomla's 3.76% means something else. Joomla is mostly detected through a low-confidence HTML pattern and a JavaScript variable, neither of which carries a version, so the figure describes the detection rules more than it describes Joomla administrators.

## WordPress is current, and not slightly

| Version | Sites | Share of versioned |
|---|---:|---:|
| 6.8 | 2,249,343 | **65.07%** |
| 6.7 | 337,957 | 9.78% |
| 6.6 | 145,590 | 4.21% |
| 6.5 | 99,619 | 2.88% |
| 6.4 | 99,084 | 2.87% |
| rest of 6.x | 207,907 | 6.02% |
| 5.x | 233,664 | 6.76% |
| 4.x | 77,626 | 2.25% |
| 3.x and older | 5,657 | 0.16% |
| *version not identified* | 1,929,969 | |

The sites in the last row are 35.83% of every WordPress installation the crawl found. They are counted here and excluded from every percentage above it.

**6.8 was the current release when the crawl ran**, and two thirds of sites were on it. Three quarters were on one of the last two, and 90.83% were on the 6 branch.

Under a tenth, 9.17%, were on 5.x or older. WordPress 4.9 still holds 39,872 sites, which is a real tail, but it is a tail: 1.15% of the versioned population.

Versions that cannot exist, such as `99.99`, `999.9` and `13.37`, account for roughly 76 sites. Editing the generator tag to mislead a scanner is a thing people do, and at 0.002% it is a rounding error.

## Drupal is the opposite

| Version | Sites | Share | Status at the crawl |
|---|---:|---:|---|
| 10 | 58,721 | 44.20% | supported |
| 7 | 37,044 | **27.88%** | end of life since January 2025 |
| 9 | 17,402 | 13.10% | end of life since November 2023 |
| 8 | 13,496 | 10.16% | end of life since November 2021 |
| 11 | 6,177 | 4.65% | supported |
| 1 | 15 | 0.01% | a misreading, not a real Drupal 1 |
| *version not identified* | 30,709 | | 18.77% of all Drupal detected |

**51.2% of versioned Drupal sites were running a version that no longer receives security fixes.** Drupal 7 alone is 27.88%, six months past its end of life after a support window that had already been extended repeatedly, and Drupal 8 has been unsupported for four years.

The contrast with WordPress is the finding of this report, and it is not about the languages or the communities being different in diligence. WordPress updates itself by default and has since 3.7 in 2013. Drupal does not: a major upgrade from 7 to 10 is a migration, not an update, and the numbers show what that costs at scale.

## Magento

Magento publishes only a major in its version string, so there is nothing finer to group by.

| Version | Sites | Share of versioned | Status at the crawl |
|---|---:|---:|---|
| 2 | 33,798 | 95.76% | supported |
| 1 | 1,496 | **4.24%** | end of life since June 2020 |
| *version not identified* | 16,642 | | 32.04% of all Magento detected |

1,496 shops were taking payments on a platform that had stopped receiving security patches five years earlier. The share is small; the absolute number is not nothing, and unlike a blog a shop handles card details.

## Joomla

| Version | Sites | Share of versioned | Status at the crawl |
|---|---:|---:|---|
| 1.5 | 5,006 | 66.93% | end of life since 2012 |
| 2.5 | 1,899 | 25.39% | end of life since 2014 |
| 1.7 | 309 | 4.13% | end of life since 2012 |
| 1.6 | 180 | 2.41% | end of life since 2011 |
| 3.x | 84 | 1.12% | end of life since 2023 |
| 4.5 | 1 | 0.01% | supported |
| *version not identified* | 191,708 | | 96.24% of all Joomla detected |

**This table should not be quoted.** It says 98.9% of versioned Joomla sites are on branches that went end of life more than a decade ago, which is true of the sample and misleading about Joomla. Modern Joomla stopped putting its version in the generator tag, so the only Joomla sites that announce one are the ones old enough to still do it. Of 199,188 Joomla installations found, 96.24% said nothing, and the 3.76% that spoke are almost by definition the antique ones.

It is reported because leaving it out would hide that the question cannot be answered for Joomla with this data.

## What is wrong with these numbers

**The sites that hide their version are not a random third.** Removing the generator tag is deliberate, and the people who do it are the ones administering their sites attentively, who are also the ones more likely to have patched. Every figure here is therefore biased toward the neglected end, and the true distributions are more current than what is shown. This matters most for WordPress, where 35.8% is hidden.

**A version number is not a patch level.** The grouping is by major and minor, so 6.4.2 and 6.4.3 are counted together. A site on the latest minor but missing a security patch is indistinguishable from one fully up to date.

**Joomla is reported but should not be quoted.** At 3.76% coverage and with the modern branches absent by construction, its distribution answers a different question from the one it appears to answer.

**WooCommerce was measured and then left out.** Its coverage is the best of the five at 84.69%, but about 1.2% of its readings are impossible values such as `438.3301` and `9999.8`. WooCommerce is detected partly through a script URL, and that rule picks up the `?ver=` query string of whatever asset matched, which is often not WooCommerce. The `info` field does not record which rule produced a value, so the contamination cannot be separated in SQL, only flagged by implausibility, which catches the obvious cases and misses any that happen to look like a version.

**The crawl sees public home pages.** Software that does not sit on one is invisible: phpMyAdmin appears on five sites in the entire crawl, which says nothing about phpMyAdmin and everything about what HTTP Archive looks at.

**One crawl, one configuration.** 2025-07-01, mobile, root pages only, matching [The PHP applications behind the web](php-applications-on-the-web.md) so the site counts are comparable. Nothing here is a time series. [The 2026 crawl](cms-versions-2026.md) repeats the measurement on the same day of the next year.

Back to the [reports index](README.md) or the [source list](../raw-data/sources.md).
