# The PHP applications behind the web

**crawl: 2025-07-01, mobile, root pages only. data freshness: 2026-08-17 23:14:05.000 UTC. query run: 2026-09-28**

**Question:** the crawl says PHP is on half the web. Half the web running *what*? Which named applications account for it, and how much of the CMS and ecommerce layer do they hold?

**Extractions:** [`httparchive.crawl`, PHP applications](../raw-data/extractions/httparchive-php-applications.md)

## Totals

| Measure | Sites | Share | What it means |
|---|---:|---:|---|
| Sites crawled | 15,313,974 | 100% | Origins tested in the mobile configuration, root page only |
| `PHP` detected | 8,097,840 | 52.88% of all | Wappalyzer saw PHP itself, from a header, a cookie or a URL pattern |
| Runs a named PHP application | 6,290,131 | 41.07% of all | WordPress, WooCommerce, Drupal, Magento and the other platforms on the query's list |
| PHP with no named application | 1,807,713 | 22.32% of PHP sites | PHP is there, but no platform the list knows about. Custom applications, Laravel and Symfony builds, and anything the list still misses |
| Named application, PHP not flagged | 4 | 0.00% | The diagnostic. See below |
| Uses a CMS | 8,342,340 | 54.48% of all | Any technology Wappalyzer files under `CMS` |
| CMS written in PHP | 6,247,271 | 74.89% of CMS sites | Just under three quarters of the CMS layer |
| Is a shop | 2,917,790 | 19.05% of all | Any `Ecommerce` technology, minus the two generic cart detections |
| Shop written in PHP | 1,425,748 | 48.86% of shops | Just under half of online retail, on the Web Almanac's definition of a shop |
| Is a shop, strict | 2,409,038 | 15.73% of all | The same, with `Squarespace Commerce` and `Wix eCommerce` also taken out. See [Families](#families) |
| Shop written in PHP, strict | 1,425,642 | 59.18% of strict shops | Nearly three fifths of online retail once the site-builder detections are gone |

The columns:

* **Measure**: the group of sites being counted. The groups overlap on purpose, a WooCommerce shop is counted in every row that applies to it.
* **Sites**: how many origins fall into that group.
* **Share**: the count as a percentage. Watch the denominator, it changes from row to row and each cell names its own.
* **What it means**: what the group covers, and what it does not.

## Platforms

The 93 largest standalone technologies in the `CMS` and `Ecommerce` categories, same crawl. Seven more were returned by the query and are held back for [Families](#families): they are add-ons that only ever run on top of another row here, so listing them alongside their parent would present one site as two.

| Platform | Sites | Share of all sites | PHP |
|---|---:|---:|---|
| WordPress | 5,386,496 | 35.17% | yes |
| Shopify | 612,738 | 4.00% | no |
| Wix | 435,207 | 2.84% | no |
| Squarespace | 241,092 | 1.57% | no |
| Joomla | 199,187 | 1.30% | yes |
| Drupal | 163,595 | 1.07% | yes |
| Webflow | 109,624 | 0.72% | no |
| Duda | 93,160 | 0.61% | no |
| PrestaShop | 89,869 | 0.59% | yes |
| Tilda | 86,508 | 0.56% | no |
| 1C-Bitrix | 80,766 | 0.53% | yes |
| TYPO3 CMS | 65,806 | 0.43% | yes |
| GoDaddy Website Builder | 62,230 | 0.41% | no |
| Weebly | 60,182 | 0.39% | yes |
| Magento | 51,936 | 0.34% | yes |
| Tistory | 48,525 | 0.32% | no |
| Jimdo | 46,041 | 0.30% | no |
| OpenCart | 34,705 | 0.23% | yes |
| Hatena Blog | 24,666 | 0.16% | no |
| Tiendanube | 23,683 | 0.15% | no |
| HubSpot CMS Hub | 22,471 | 0.15% | no |
| Adobe Experience Manager | 22,402 | 0.15% | no |
| Google Sites | 20,690 | 0.14% | no |
| Nuvemshop | 19,892 | 0.13% | no |
| BigCommerce | 18,495 | 0.12% | no |
| Craft CMS | 16,857 | 0.11% | yes |
| Contao | 16,310 | 0.11% | yes |
| Cafe24 | 15,378 | 0.10% | no |
| Odoo | 15,335 | 0.10% | no |
| Amazon Webstore | 15,020 | 0.10% | no |
| Framer Sites | 14,698 | 0.10% | no |
| Shoptet | 14,352 | 0.09% | yes |
| Skolengo | 13,954 | 0.09% | no |
| Tray | 13,768 | 0.09% | no |
| Contentful | 13,045 | 0.09% | no |
| DNN | 12,949 | 0.08% | no |
| WebNode | 12,832 | 0.08% | no |
| Gnuboard | 12,709 | 0.08% | yes |
| October CMS | 12,383 | 0.08% | yes |
| Loja Integrada | 11,351 | 0.07% | no |
| DataLife Engine | 11,150 | 0.07% | yes |
| ColorMeShop | 10,952 | 0.07% | no |
| Shopline | 10,641 | 0.07% | no |
| MODX | 10,411 | 0.07% | yes |
| Salla | 10,145 | 0.07% | no |
| Shopware | 9,931 | 0.06% | yes |
| Liferay | 9,137 | 0.06% | no |
| Pixnet | 9,005 | 0.06% | no |
| Kajabi | 8,760 | 0.06% | no |
| Kentico CMS | 8,723 | 0.06% | no |
| Ecwid | 8,570 | 0.06% | no |
| Sitecore | 8,555 | 0.06% | no |
| Sanity | 8,404 | 0.05% | no |
| Concrete CMS | 8,368 | 0.05% | yes |
| JouwWeb | 7,776 | 0.05% | no |
| Wagtail | 7,319 | 0.05% | no |
| Strato Website | 7,218 | 0.05% | no |
| Shoper | 7,208 | 0.05% | no |
| Imweb | 6,966 | 0.05% | no |
| Lightspeed eCom | 6,738 | 0.04% | no |
| Megagroup CMS.S3 | 6,682 | 0.04% | no |
| Bentobox | 6,375 | 0.04% | no |
| nopCommerce | 6,251 | 0.04% | no |
| JTL Shop | 6,215 | 0.04% | no |
| Backdrop | 6,002 | 0.04% | yes |
| MakeShop | 5,984 | 0.04% | no |
| Umbraco | 5,945 | 0.04% | no |
| Popmenu | 5,904 | 0.04% | no |
| stores.jp | 5,696 | 0.04% | no |
| ePages | 5,608 | 0.04% | no |
| Mono.net | 5,579 | 0.04% | no |
| Ghost | 5,360 | 0.04% | no |
| ProcessWire | 5,230 | 0.03% | yes |
| Unas | 5,198 | 0.03% | no |
| EC-CUBE | 5,166 | 0.03% | yes |
| ExpressionEngine | 5,060 | 0.03% | yes |
| CS Cart | 5,041 | 0.03% | yes |
| SPIP | 4,994 | 0.03% | yes |
| Haravan | 4,807 | 0.03% | no |
| Silverstripe | 4,792 | 0.03% | yes |
| Bizweb | 4,720 | 0.03% | no |
| Gumroad | 4,684 | 0.03% | no |
| Salesforce Commerce Cloud | 4,622 | 0.03% | no |
| Prismic | 4,612 | 0.03% | no |
| Microsoft SharePoint | 4,389 | 0.03% | no |
| Storyblok | 4,381 | 0.03% | no |
| Plone | 4,115 | 0.03% | no |
| VTEX | 4,055 | 0.03% | no |
| Base | 4,043 | 0.03% | no |
| Gambio | 3,772 | 0.02% | yes |
| inSales | 3,610 | 0.02% | no |
| Pimcore | 3,590 | 0.02% | yes |
| Jumpseller | 3,576 | 0.02% | no |

The columns:

* **Platform**: the technology name as Wappalyzer reports it.
* **Sites**: distinct origins where that technology was detected.
* **Share of all sites**: against the 15,313,974 origins crawled, not against the CMS or shop subtotal.
* **PHP**: whether the platform is written in PHP.

A site can still appear twice here for a different reason: WordPress and Drupal on the same origin, or a platform Wappalyzer files under both `CMS` and `Ecommerce`. Those cases are rare, and the next section measures how rare.

## What PHP is running

The same rows, reduced to the 27 that are PHP and measured against PHP rather than against the whole web. This answers what a PHP site is most likely to be, which the table above cannot show because it is dominated by platforms that are not PHP at all.

| Platform | Sites | Share of PHP sites | Share of all sites |
|---|---:|---:|---|
| WordPress | 5,386,496 | 66.52% | 35.17% |
| Joomla | 199,187 | 2.46% | 1.30% |
| Drupal | 163,595 | 2.02% | 1.07% |
| PrestaShop | 89,869 | 1.11% | 0.59% |
| 1C-Bitrix | 80,766 | 1.00% | 0.53% |
| TYPO3 CMS | 65,806 | 0.81% | 0.43% |
| Weebly | 60,182 | 0.74% | 0.39% |
| Magento | 51,936 | 0.64% | 0.34% |
| OpenCart | 34,705 | 0.43% | 0.23% |
| Craft CMS | 16,857 | 0.21% | 0.11% |
| Contao | 16,310 | 0.20% | 0.11% |
| Shoptet | 14,352 | 0.18% | 0.09% |
| Gnuboard | 12,709 | 0.16% | 0.08% |
| October CMS | 12,383 | 0.15% | 0.08% |
| DataLife Engine | 11,150 | 0.14% | 0.07% |
| MODX | 10,411 | 0.13% | 0.07% |
| Shopware | 9,931 | 0.12% | 0.06% |
| Concrete CMS | 8,368 | 0.10% | 0.05% |
| Backdrop | 6,002 | 0.07% | 0.04% |
| ProcessWire | 5,230 | 0.06% | 0.03% |
| EC-CUBE | 5,166 | 0.06% | 0.03% |
| ExpressionEngine | 5,060 | 0.06% | 0.03% |
| CS Cart | 5,041 | 0.06% | 0.03% |
| SPIP | 4,994 | 0.06% | 0.03% |
| Silverstripe | 4,792 | 0.06% | 0.03% |
| Gambio | 3,772 | 0.05% | 0.02% |
| Pimcore | 3,590 | 0.04% | 0.02% |
| *PHP with no named application* | 1,807,713 | 22.32% | 11.80% |

The columns:

* **Platform**: the 27 standalone PHP technologies, largest first. Derivatives are in [Families](#families) instead.
* **Sites**: distinct origins where the technology was detected.
* **Share of PHP sites**: against the 8,097,840 origins where PHP was detected at all. This is the column that says how much of PHP each platform accounts for.
* **Share of all sites**: against the 15,313,974 origins crawled, repeated from the previous table so the two scales can be compared.

The last row is not a platform. It is the PHP that Wappalyzer could not attribute to anything on the list, and it is here because leaving it out would make the rest look more complete than it is.

**With the derivatives taken out the column almost adds up.** The 27 platforms come to 6,288,660 sites against the 6,290,131 measured as running a PHP application, which is 99.98%. The small shortfall is the platforms below rank 100 that the breakdown never reached, set against the handful of origins carrying two of these at once.

That column is heavily concentrated. **WordPress is 66.52% of every site where PHP was detected**, and that figure already includes its WooCommerce shops. The next three are Joomla at 2.46%, Drupal at 2.02% and PrestaShop at 1.11%. All 26 platforms after WordPress come to 902,164 sites, which is 11.14%.

## Families

Seven of the rows the query returned are not platforms. They are add-ons that only ever run on top of another row, which Wappalyzer records with a `requires` link: `WooCommerce` carries `requires: WordPress`. They are kept out of the tables above and set out here instead, one sub-table per parent.

The query also counted sites running a derivative without its parent. Across all six families it found **4**, all of them WooCommerce with WordPress undetected. So a derivative is carved out of its parent, never added to it, and the parent row in the tables above is the whole family.

#### WordPress
| Component | Sites | Share of the family |
|---|---:|---:|
| WordPress, no derivative detected | 4,306,307 | 79.95% |
| WooCommerce | 1,075,770 | 19.97% |
| EasyDigitalDownloads | 5,687 | 0.11% |
| both derivatives on the same site | 1,268 | 0.02% |
| **WordPress family** | **5,386,496** | **100%** |

#### Wix (not PHP)
| Component | Sites | Share of the family |
|---|---:|---:|
| Wix, no derivative detected | 166,085 | 38.16% |
| Wix eCommerce | 269,122 | 61.84% |
| **Wix family** | **435,207** | **100%** |

#### Squarespace (not PHP)
| Component | Sites | Share of the family |
|---|---:|---:|
| Squarespace, no derivative detected | 39 | 0.02% |
| Squarespace Commerce | 241,053 | 99.98% |
| **Squarespace family** | **241,092** | **100%** |

#### Webflow (not PHP)
| Component | Sites | Share of the family |
|---|---:|---:|
| Webflow, no derivative detected | 93,329 | 85.14% |
| Webflow Ecommerce | 16,295 | 14.86% |
| **Webflow family** | **109,624** | **100%** |

#### Weebly
| Component | Sites | Share of the family |
|---|---:|---:|
| Weebly, no derivative detected | 38,493 | 63.96% |
| Square Online | 21,689 | 36.04% |
| **Weebly family** | **60,182** | **100%** |

#### Magento
| Component | Sites | Share of the family |
|---|---:|---:|
| Magento, no derivative detected | 47,917 | 92.26% |
| Hyva Themes | 4,019 | 7.74% |
| **Magento family** | **51,936** | **100%** |

Two things fall out of these tables.

**WordPress is four fifths a publishing tool and one fifth a shop.** The 4,306,307 sites with no derivative detected are 28.12% of the whole crawl and **53.18% of every site running PHP**. That single figure, WordPress with nothing attached, is larger than every other PHP platform put together.

**Two of the derivatives are detection artefacts rather than shops.** `Squarespace Commerce` sits on 99.98% of the Squarespace family and `Wix eCommerce` on 61.84% of the Wix family. Shares that high are not telling us those sites sell anything, they are telling us the builder supports selling, which is the same failure the Web Almanac already guards against by excluding `Cart Functionality` and `Google Analytics Enhanced eCommerce`.

Excluding them as well removes 508,752 sites from the shop denominator and takes PHP's share of online shops from 48.86% to **59.18%**. Of the sites removed, 106 were running a PHP application, so the numerator barely moves, from 1,425,748 to 1,425,642, and almost all of the change is the denominator shedding non-PHP entries.

The case is stronger for one than the other. A detection present on 99.98% of Squarespace sites is not distinguishing shops from anything. `Wix eCommerce` at 61.84% is less clear cut, since a site builder aimed at small business could plausibly have that many sellers. Excluding Squarespace alone would land the PHP share around 53%, so the figure sits somewhere in 48.86% to 59.18% depending on where the line is drawn, with the published 48.86% at the conservative end.

## How to read this

Wappalyzer applies its own dependency graph at crawl time. Out of 15.3 million sites, only **4** carry a named PHP application without also carrying the `PHP` flag. So the 52.88% figure is not a separate measurement that could be added to the platform counts, it already contains every WordPress, WooCommerce and Drupal site. WooCommerce declares no dependencies of its own, but it never appears without WordPress, which declares PHP, so it inherits the flag anyway.

So the two numbers describe the same sites at different resolutions. Of the 8.1 million sites running PHP, just over three quarters run something with a name, and the remaining **22.32%** run PHP that Wappalyzer cannot attribute to a platform. At 1.81 million sites that group is too large to set aside, and it is where custom applications sit along with anything built on Laravel or Symfony, which Wappalyzer files under `Web frameworks` and not under `CMS`.

The two figures worth quoting are the ratios, not the raw counts. **74.89% of the CMS layer is PHP, and between 48.86% and 59.18% of online shops.** Those hold up without any argument about what counts as "a PHP site", because the denominator is sites that have already been classified as a CMS or a shop by somebody else's rule. The range on the shop figure is not uncertainty in the measurement, it is two defensible definitions of a shop, set out under [Families](#families).

Under those ratios sits the concentration set out in [What PHP is running](#what-php-is-running). WordPress is 64.57% of all CMS sites and 66.52% of PHP, and its WooCommerce shops are 36.87% of all online retail. PHP holds the web's publishing and retail layer, and one product carries most of both.

The figures line up with the published Web Almanac, which draws on the same crawl:

| Measure | This run, mobile | Web Almanac 2025 |
|---|---:|---:|
| Sites using a CMS | 54.48% | over 54% |
| WordPress, share of CMS | 64.57% | 64.3% |
| WordPress, share of all sites | 35.17% | ~35.6% |
| Sites that are shops | 19.05% | 19.2% mobile |
| WooCommerce, share of shops | 36.87% | 35.4% desktop |
| Shopify, share of shops | 21.00% | 21.5% desktop |

The remaining gaps are desktop against mobile, and they run under a point and a half.

Back to the [reports index](README.md) or the [source list](../raw-data/sources.md).
