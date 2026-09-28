# `httparchive.crawl`, PHP applications

**Source:** [HTTP Archive](../sources.md#14-http-archive) through [Google BigQuery](../sources.md#11-google-bigquery). [Open the dataset in the console](https://console.cloud.google.com/bigquery?p=httparchive&d=crawl&t=pages&page=table).

**data freshness: 2026-08-17 23:14:05.000 UTC, last check 2026-09-01 16:00:00.000 UTC**

The same table as [`httparchive.crawl`](httparchive-crawl.md), asked a different question. That extraction counts the sites where Wappalyzer reports the `PHP` technology. This one counts the sites running a PHP **application** that has a name people recognise: WordPress, WooCommerce, Drupal, Joomla, Magento, PrestaShop and the rest.

The distinction matters for outreach. "PHP is on 52% of sites" invites the reply that it is old servers nobody maintains. "PHP is what WordPress, WooCommerce, Drupal and Magento are written in, and those are three quarters of the CMS layer and half of all online shops" is the same fact with the burden of proof moved.

It also answers a question the first extraction cannot: whether the `PHP` flag and the application flag agree.

### Defining "PHP-based"

Wappalyzer carries a dependency graph with two kinds of edge. `implies` means another technology is also present, and for most PHP applications it contains `PHP` directly. `requires` means the technology is only ever detected on top of another one, which is how plugins are recorded.

| Technology | `cats` | `implies` | `requires` |
|---|---|---|---|
| `WordPress` | CMS, Blogs | `PHP`, `MySQL` | |
| `Drupal` | CMS | `PHP` | |
| `Joomla` | CMS | `PHP` | |
| `Magento` | Ecommerce, CMS | `PHP`, `MySQL` | |
| `PrestaShop` | Ecommerce, CMS | `PHP`, `MySQL` | |
| `Shopware` | Ecommerce | `PHP`, `MySQL`, `jQuery`, `Symfony` | |
| `Craft CMS` | CMS | `Yii` | |
| `WooCommerce` | Ecommerce, Payment processors | *empty* | `WordPress` |
| `EasyDigitalDownloads` | Ecommerce | *empty* | `WordPress` |

Resolved over the [HTTP Archive fork of the Wappalyzer definitions](https://github.com/HTTPArchive/wappalyzer/tree/main/src/technologies), `implies` alone reaches 296 technologies, 123 of them in `CMS` and 69 in `Ecommerce`. Following `implies` and `requires` together reaches 651.

> [!warning]
> The difference between those two numbers is `WooCommerce` and everything like it. `WooCommerce` declares no `implies`, so a rule built on `implies` alone misses the largest PHP commerce platform in the crawl, 35.4% of all ecommerce sites in the 2025 Web Almanac. It reaches PHP only through `requires: WordPress`.

The queries below use a hand-written list rather than either rule, because the figures in the report were produced with that list. Following both fields reproduces it exactly, so it can be swapped for a derived one whenever the numbers are refreshed.

Two platforms cannot be settled either way. `Shoper` (7,208 sites) and `JTL Shop` (6,215) declare neither field, so the graph says nothing about them in either direction and their absence from the list rests on nothing.

### Method

Both almanac chapters classify a site by the Wappalyzer categories attached to its detected technologies, unnesting twice and matching `cats = 'CMS'` or `cats = 'Ecommerce'`. The Ecommerce chapter then drops two detections that mark a shop without being a platform, `Cart Functionality` and `Google Analytics Enhanced eCommerce`. The queries keep both conventions so the totals can be checked against the published figures.

Two further detections are dropped from a second, stricter shop count. `Squarespace Commerce` is present on 99.98% of Squarespace sites and `Wix eCommerce` on 61.84% of Wix sites, which means they record that a site builder supports selling rather than that a site sells. Both counts are reported so the looser one stays comparable with the Web Almanac.

Crawl dates follow the almanac so the numbers line up with the chapters: `2025-07-01`, `2024-06-01`, `2023-06-01`, `2022-06-01`, `2021-07-01`.

> [!important]
> the queries reported here need to be reasoned and refined

## Query 1: totals

Site counts, both shop definitions, and the WordPress family broken out.

```sql
DECLARE php_apps ARRAY<STRING> DEFAULT [
  'WordPress', 'WooCommerce', 'Drupal', 'Joomla', 'Magento', 'PrestaShop',
  'OpenCart', 'TYPO3 CMS', 'Shopware', 'Concrete CMS', 'MODX', 'Contao',
  'SPIP', 'OXID eShop', 'CS Cart', 'osCommerce', 'X-Cart', 'CubeCart',
  'Pimcore', 'Silverstripe', 'ExpressionEngine', 'DataLife Engine',
  '1C-Bitrix', 'Grav', 'Statamic', 'Textpattern CMS', 'e107', 'XOOPS',
  'Weebly', 'Shoptet', 'Craft CMS', 'Square Online',
  'Gnuboard', 'October CMS', 'Backdrop', 'ProcessWire', 'EC-CUBE',
  'Hyva Themes', 'Gambio', 'EasyDigitalDownloads'
];

DECLARE not_a_platform ARRAY<STRING> DEFAULT [
  'Cart Functionality', 'Google Analytics Enhanced eCommerce'
];

DECLARE builder_commerce ARRAY<STRING> DEFAULT [
  'Squarespace Commerce', 'Wix eCommerce'
];

WITH sites AS (
  SELECT
    root_page,
    LOGICAL_OR(t.technology = 'PHP')              AS php_detected,
    LOGICAL_OR(t.technology IN UNNEST(php_apps))  AS php_app,
    LOGICAL_OR('CMS' IN UNNEST(t.categories))     AS is_cms,
    LOGICAL_OR('Ecommerce' IN UNNEST(t.categories)
               AND t.technology NOT IN UNNEST(not_a_platform)) AS is_shop,
    LOGICAL_OR('Ecommerce' IN UNNEST(t.categories)
               AND t.technology NOT IN UNNEST(not_a_platform)
               AND t.technology NOT IN UNNEST(builder_commerce)) AS is_shop_strict,
    LOGICAL_OR(t.technology = 'WordPress')            AS wordpress,
    LOGICAL_OR(t.technology = 'WooCommerce')          AS woocommerce,
    LOGICAL_OR(t.technology = 'EasyDigitalDownloads') AS edd
  FROM `httparchive.crawl.pages`, UNNEST(technologies) AS t
  WHERE date = '2025-07-01'
    AND client = 'mobile'
    AND is_root_page
  GROUP BY root_page
)

SELECT
  -- Totals.
  COUNT(*)                                    AS total_sites,
  COUNTIF(php_detected)                       AS php_detected,
  COUNTIF(php_app)                            AS runs_a_php_application,
  COUNTIF(php_app AND NOT php_detected)       AS php_app_but_php_not_flagged,
  COUNTIF(php_detected AND NOT php_app)       AS php_without_a_named_application,
  COUNTIF(is_cms)                             AS cms_sites,
  COUNTIF(is_cms AND php_app)                 AS cms_sites_on_php,
  ROUND(100 * COUNTIF(is_cms AND php_app) / NULLIF(COUNTIF(is_cms), 0), 2) AS pct_of_cms_on_php,

  -- Ecommerce, both definitions.
  COUNTIF(is_shop)                            AS ecommerce_sites,
  COUNTIF(is_shop AND php_app)                AS ecommerce_sites_on_php,
  ROUND(100 * COUNTIF(is_shop AND php_app) / NULLIF(COUNTIF(is_shop), 0), 2) AS pct_of_shops_on_php,
  COUNTIF(is_shop_strict)                     AS shops_strict,
  COUNTIF(is_shop_strict AND php_app)         AS shops_strict_on_php,
  ROUND(100 * COUNTIF(is_shop_strict AND php_app) / NULLIF(COUNTIF(is_shop_strict), 0), 2) AS pct_php_strict,
  COUNTIF(is_shop AND NOT is_shop_strict)             AS dropped,
  COUNTIF(is_shop AND NOT is_shop_strict AND php_app) AS dropped_but_php,

  -- WordPress family detail.
  COUNTIF(wordpress)                                  AS wordpress_sites,
  COUNTIF(wordpress AND NOT woocommerce AND NOT edd)   AS wp_core_only,
  COUNTIF(woocommerce)                                AS woocommerce,
  COUNTIF(edd)                                        AS easy_digital_downloads,
  COUNTIF(woocommerce AND edd)                        AS both,
  COUNTIF(woocommerce AND NOT wordpress)              AS woocommerce_without_wordpress,
  COUNTIF(edd AND NOT wordpress)                      AS edd_without_wordpress
FROM sites;
```

## Query 2: platform breakdown

The largest technologies in the `CMS` and `Ecommerce` categories, labelled by whether they are PHP. The limit is set to 100 to make the tail visible; the scan is the same either way, so the extra rows cost nothing.

Both `DECLARE` statements from query 1 are in scope only for the script that opens them, so this has to run in the same tab or be preceded by a copy of them.

```sql
SELECT
  t.technology AS platform,
  t.technology IN UNNEST(php_apps) AS is_php,
  COUNT(DISTINCT p.root_page) AS sites
FROM `httparchive.crawl.pages` AS p, UNNEST(p.technologies) AS t, UNNEST(t.categories) AS cat
WHERE p.date = '2025-07-01'
  AND p.client = 'mobile'
  AND p.is_root_page
  AND cat IN ('CMS', 'Ecommerce')
  AND t.technology NOT IN UNNEST(not_a_platform)
GROUP BY platform, is_php
ORDER BY sites DESC
LIMIT 100;
```

## Query 3: families

Some rows in the breakdown are not platforms but add-ons that only ever run on top of another row, which `requires` records. Six families exist across the 100 rows. The metadata records no other parent links, so everything else counts as standalone.

| Parent | Derivatives | Link |
|---|---|---|
| `WordPress` | `WooCommerce`, `EasyDigitalDownloads` | `requires` |
| `Weebly` | `Square Online` | `requires` and `implies` |
| `Webflow` | `Webflow Ecommerce` | `requires` |
| `Magento` | `Hyva Themes` | `implies` |
| `Wix` | `Wix eCommerce` | `implies` |
| `Squarespace` | `Squarespace Commerce` | `implies` |

Site counts alone cannot split a parent into core and derivative, because a derivative site carries both detections and is counted in both rows. Splitting them needs the crawl itself.

```sql
-- Core against derivatives, one row per family.
-- Adding a family is one line; keep `watched` in sync with it.
DECLARE families ARRAY<STRUCT<parent STRING, children ARRAY<STRING>>> DEFAULT [
  STRUCT('WordPress'   AS parent, ['WooCommerce', 'EasyDigitalDownloads'] AS children),
  STRUCT('Weebly',      ['Square Online']),
  STRUCT('Webflow',     ['Webflow Ecommerce']),
  STRUCT('Magento',     ['Hyva Themes']),
  STRUCT('Wix',         ['Wix eCommerce']),
  STRUCT('Squarespace', ['Squarespace Commerce'])
];

-- Every name above, flattened. Filtering on it keeps the arrays below tiny.
DECLARE watched ARRAY<STRING> DEFAULT [
  'WordPress', 'WooCommerce', 'EasyDigitalDownloads',
  'Weebly', 'Square Online',
  'Webflow', 'Webflow Ecommerce',
  'Magento', 'Hyva Themes',
  'Wix', 'Wix eCommerce',
  'Squarespace', 'Squarespace Commerce'
];

WITH sites AS (
  SELECT root_page, ARRAY_AGG(DISTINCT t.technology) AS techs
  FROM `httparchive.crawl.pages`, UNNEST(technologies) AS t
  WHERE date = '2025-07-01'
    AND client = 'mobile'
    AND is_root_page
    AND t.technology IN UNNEST(watched)  -- sites with none of these drop out
  GROUP BY root_page
)

SELECT
  f.parent AS family,
  COUNTIF(f.parent IN UNNEST(s.techs)) AS parent_sites,

  -- The split the family question is about.
  COUNTIF(f.parent IN UNNEST(s.techs)
          AND EXISTS(SELECT 1 FROM UNNEST(f.children) AS c WHERE c IN UNNEST(s.techs)))
    AS with_derivative,
  COUNTIF(f.parent IN UNNEST(s.techs)
          AND NOT EXISTS(SELECT 1 FROM UNNEST(f.children) AS c WHERE c IN UNNEST(s.techs)))
    AS core_only,

  -- Should be near zero. Anything large means the parent link is wrong, or that
  -- the parent can go undetected on sites where the derivative is visible.
  COUNTIF(f.parent NOT IN UNNEST(s.techs)
          AND EXISTS(SELECT 1 FROM UNNEST(f.children) AS c WHERE c IN UNNEST(s.techs)))
    AS derivative_without_parent,

  ROUND(100 * COUNTIF(f.parent IN UNNEST(s.techs)
                      AND EXISTS(SELECT 1 FROM UNNEST(f.children) AS c WHERE c IN UNNEST(s.techs)))
        / NULLIF(COUNTIF(f.parent IN UNNEST(s.techs)), 0), 2) AS pct_with_derivative
FROM sites AS s, UNNEST(families) AS f
GROUP BY family
ORDER BY parent_sites DESC;
```

## Results

**crawl: 2025-07-01, mobile, root pages only. query run: 2026-09-28**

The numbers are in [The PHP applications behind the web](../../reports/php-applications-on-the-web.md), next to [PHP on the public web](../../reports/php-on-the-public-web.md), which counts the same crawl by the plain `PHP` flag.

The totals match the published Web Almanac to within a point and a half, which is the desktop against mobile difference.

Back to the [source list](../sources.md), the [list of extractions](../sources.md#2-extractions) or the [reports index](../../reports/README.md).
