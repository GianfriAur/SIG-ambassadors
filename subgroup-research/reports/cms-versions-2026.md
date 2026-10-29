# Which version of WordPress, Drupal and Magento is actually running, 2026

**crawl used: 2026-07-01, mobile, root pages only. latest crawl available: 2026-08-17. query run: 2026-10-29**

**Question:** PHP applications carry a reputation for being left unpatched. The crawl can check it, because the version is often sitting in the page.

**Extractions:** [`httparchive.crawl`, CMS versions](../raw-data/extractions/httparchive-cms-versions.md)

The same measurement as [the 2025 crawl](cms-versions.md), one year later, on the same day of the year so the two are comparable.

## How many sites say which version they run

| Platform | Detected | Declares a version | Share |
|---|---:|---:|---:|
| Drupal | 145,838 | 119,840 | 82.17% |
| Magento | 44,342 | 28,895 | 65.16% |
| WordPress | 5,090,760 | 3,231,804 | 63.48% |
| Joomla | 160,238 | 5,085 | 3.17% |

Everything below is a share of the sites that declare, because the rest are unknown rather than old. Each table closes with the sites that could not be identified, counted against everything detected for that platform.

The four figures are within a point of the year before, and so is the order. Whatever makes a site hide its version is stable: it did not move while the version numbers underneath it did.

These four percentages do not all measure the same thing, and the difference is in how each platform is recognised. WordPress is detected five ways, from the generator tag, the `X-Pingback` header, the `api.w.org` link and two patterns in `/wp-content/` paths, but only the first carries a version. Its 63.48% therefore means what it looks like it means: **1.86 million WordPress sites are recognisable while having removed the version**, which is standard hardening advice.

Joomla's 3.17% means something else. Joomla is mostly detected through a low-confidence HTML pattern and a JavaScript variable, neither of which carries a version, so the figure describes the detection rules more than it describes Joomla administrators.

## WordPress moved a major version in six weeks

| Version | Sites | Share of versioned |
|---|---:|---:|
| 7.0 | 1,933,861 | **59.84%** |
| 6.9 | 499,119 | 15.44% |
| 6.8 | 242,382 | 7.50% |
| 6.7 | 101,529 | 3.14% |
| 6.6 | 58,443 | 1.81% |
| rest of 6.x | 202,858 | 6.28% |
| 5.x | 137,122 | 4.24% |
| 4.x | 52,355 | 1.62% |
| 3.x and older | 3,907 | 0.12% |
| rest of 7.x | 180 | 0.01% |
| *version not identified* | 1,858,956 | |

The sites in the last row are 36.52% of every WordPress installation the crawl found. They are counted here and excluded from every percentage above it.

**WordPress 7.0 was released on 20 May 2026, six weeks before this crawl, and 59.84% of versioned sites were already on it.** Three quarters were on 7.0 or 6.9.

That is faster than the year before, and the comparison is close to fair. In the 2025 crawl, 6.8 held 65.07% eleven weeks after its release. 7.0 reached 59.84% in six, and it is a major version bump with a raised minimum PHP requirement, which is the kind of release that normally slows adoption down.

The old tail barely moved. 5.98% were on 5.x or older, against 9.17% a year earlier, and WordPress 4.9 has gone from 39,872 sites to 26,544. The sites that are going to update have updated; what is left is shrinking slowly on its own.

Versions that cannot exist, such as `99.99` and `13.37`, account for 48 sites. Editing the generator tag to mislead a scanner is a thing people do, and at 0.001% it is a rounding error.

## Drupal is still the opposite, with less distance

| Version | Sites | Share | Status at the crawl |
|---|---:|---:|---|
| 10 | 47,725 | 39.82% | supported until 9 December 2026 |
| 7 | 26,092 | **21.77%** | end of life since January 2025 |
| 11 | 24,709 | 20.62% | supported |
| 9 | 11,585 | 9.67% | end of life since November 2023 |
| 8 | 9,711 | 8.10% | end of life since November 2021 |
| other readings | 18 | 0.02% | nine read as Drupal 1, two genuine antiques on 5 and 6, the rest impossible |
| *version not identified* | 25,998 | | 17.83% of all Drupal detected |

**39.54% of versioned Drupal sites were running a version that no longer receives security fixes**, down from 51.2% a year earlier. The movement is real and it is concentrated in one place: Drupal 11 went from 4.65% to 20.62%, roughly a fifth of versioned Drupal in under two years from its release.

Drupal 7 is still 21.77%, eighteen months past an end of life that had already been extended repeatedly. Drupal 8 is still 8.10% after five unsupported years.

Drupal 8 reaching end of life before Drupal 7 is not a misprint. Drupal 8 was the first release built on Symfony and was tied to Symfony 3's support window, which closed in November 2021; moving it to Symfony 4 would have broken compatibility inside the Drupal 8 series. Drupal 7 predates Symfony entirely and had its own window extended into 2025. The version that had upgraded lost support first.

The contrast with WordPress holds, and it is not about the languages or the communities being different in diligence. WordPress updates itself by default and has since 3.7 in 2013. Drupal does not: a major upgrade from 7 to 10 is a migration, not an update, and the numbers show what that costs at scale.

What this crawl adds is that the gap closes when a migration path is short. Drupal 10 to 11 is an ordinary upgrade, and the sites took it.

## Magento

Magento publishes only a major in its version string, so there is nothing finer to group by.

| Version | Sites | Share of versioned | Status at the crawl |
|---|---:|---:|---|
| 2 | 27,573 | 95.42% | supported |
| 1 | 1,322 | **4.58%** | end of life since June 2020 |
| *version not identified* | 15,447 | | 34.84% of all Magento detected |

1,322 shops were taking payments on a platform that had stopped receiving security patches six years earlier, down from 1,496. The share went the other way, from 4.24% to 4.58%, because the versioned Magento population shrank faster than the Magento 1 tail did. Sites on 1 are leaving, and they are leaving more slowly than everyone else.

## Joomla

| Version | Sites | Share of versioned | Status at the crawl |
|---|---:|---:|---|
| 1.5 | 3,328 | 65.45% | end of life since 2012 |
| 2.5 | 1,309 | 25.74% | end of life since 2014 |
| 1.7 | 235 | 4.62% | end of life since 2012 |
| 1.6 | 121 | 2.38% | end of life since 2011 |
| 3.x | 91 | 1.79% | end of life since 2023 |
| other readings | 1 | 0.02% | an impossible 8.99 |
| *version not identified* | 155,153 | | 96.83% of all Joomla detected |

**This table should not be quoted.** It says 98.19% of versioned Joomla sites are on branches that went end of life more than a decade ago, which is true of the sample and misleading about Joomla. Modern Joomla stopped putting its version in the generator tag, so the only Joomla sites that announce one are the ones old enough to still do it. Of 160,238 Joomla installations found, 96.83% said nothing, and the 3.17% that spoke are almost by definition the antique ones. Joomla 6.0 shipped in October 2025 and 6.1 in April 2026, and neither appears here at all.

It is reported because leaving it out would hide that the question cannot be answered for Joomla with this data.

## What is wrong with these numbers

**The sites that hide their version are not a random third.** Removing the generator tag is deliberate, and the people who do it are the ones administering their sites attentively, who are also the ones more likely to have patched. Every figure here is therefore biased toward the neglected end, and the true distributions are more current than what is shown. This matters most for WordPress, where 36.5% is hidden.

**A version number is not a patch level.** The grouping is by major and minor, so 7.0.1 and 7.0.2 are counted together. A site on the latest minor but missing a security patch is indistinguishable from one fully up to date.

**The detected counts fell on every platform, and that is not a measurement of decline.** WordPress is down 5.5% on the year, Drupal 10.9%, Magento 14.6%, Joomla 19.6%. The crawl's own site list changes between runs, so part of this is which sites were visited rather than which software they run. The share tables are unaffected, since each is computed inside its own platform, but the absolute counts should not be read as a trend. [The PHP applications behind the web](php-applications-on-the-web.md) is the report that measures share of the web, and it measures it properly.

**Joomla is reported but should not be quoted.** At 3.17% coverage and with the modern branches absent by construction, its distribution answers a different question from the one it appears to answer.

**WooCommerce was measured and then left out.** Its coverage is the best of the five at 84.86%, but 0.61% of its readings are impossible values such as `6435` and `1420.3832`. WooCommerce is detected partly through a script URL, and that rule picks up the `?ver=` query string of whatever asset matched, which is often not WooCommerce. The `info` field does not record which rule produced a value, so the contamination cannot be separated in SQL, only flagged by implausibility, which catches the obvious cases and misses any that happen to look like a version.

**The crawl sees public home pages.** Software that does not sit on one is invisible: phpMyAdmin appears on four sites in the entire crawl, which says nothing about phpMyAdmin and everything about what HTTP Archive looks at.

**One crawl, one configuration.** 2026-07-01, mobile, root pages only, matching [the 2025 report](cms-versions.md) and [The PHP applications behind the web](php-applications-on-the-web.md) so the site counts are comparable. Two points are not a time series, and the year-over-year statements here are differences between two July days, not trends.

Back to the [reports index](README.md) or the [source list](../raw-data/sources.md).
