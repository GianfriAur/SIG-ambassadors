# Top 10 long running PHP

**population collected: 2026-10-02. first commits: `bigquery-public-data.github_repos`, snapshot 2025-10-14**

> Real query results rather than assumed metrics. Start dates come from two sources of unequal quality and every row says which it used. Read [What is wrong with these numbers](#what-is-wrong-with-these-numbers) before quoting a position.

**Question:** which PHP projects have been running longest and are still running now?

**Extractions:** [GitHub PHP repositories](../raw-data/extractions/github-php-repositories.md), ranked by [GitHub PHP longevity](../raw-data/extractions/github-php-longevity.md)

## The ten

| # | Project | Stars | Started | Months | What it is |
|---|---|---:|---|---:|---|
| 1 | [phpmyadmin/phpmyadmin](https://github.com/phpmyadmin/phpmyadmin) | 7,946 | 2001-05 | 305 | The MySQL web interface. Shipped by nearly every shared host for a quarter of a century |
| 2 | [moodle/moodle](https://github.com/moodle/moodle) | 7,454 | 2001-11 | 298 | The learning platform universities and schools run on |
| 3 | [Dolibarr/dolibarr](https://github.com/Dolibarr/dolibarr) | 7,680 | 2002-04 | 293 | ERP and CRM for small businesses |
| 4 | [dompdf/dompdf](https://github.com/dompdf/dompdf) | 11,194 | 2005-01 | 260 | HTML to PDF. The invoice at the end of a checkout is often this |
| 5 | [sebastianbergmann/phpunit](https://github.com/sebastianbergmann/phpunit) | 20,061 | 2006-06 | 243 | The testing framework, maintained by the same person throughout |
| 6 | [glpi-project/glpi](https://github.com/glpi-project/glpi) | 6,420 | 2004-01 | 272 | IT asset management and service desk |
| 7 | [cakephp/cakephp](https://github.com/cakephp/cakephp) | 8,789 | 2005-05 | 257 | The framework that brought Rails conventions to PHP, still shipping |
| 8 | [Piwigo/Piwigo](https://github.com/Piwigo/Piwigo) | 3,866 | 2003-05 | 281 | Self-hosted photo galleries |
| 9 | [pfsense/pfsense](https://github.com/pfsense/pfsense) | 5,747 | 2004-11 | 263 | The firewall distribution. Its entire web interface is PHP |
| 10 | [revive-adserver/revive-adserver](https://github.com/revive-adserver/revive-adserver) | 1,505 | 2000-12 | 310 | Ad serving, descended from phpAdsNew. The oldest start date in the table |

All ten have been running for more than twenty years and all ten were pushed to within the last twelve months. Seven are applications that someone installs and uses rather than libraries a developer pulls in, which is the opposite of the picture a star ranking gives.

The list reaches back further than GitHub does. Revive Adserver starts in 2000, phpMyAdmin and Moodle in 2001, and none of those dates could have come from the GitHub API.

## The first hundred

The extraction ranks 1,000 projects. These are the first 100.

| # | Project | Stars | Started | Source | Months | Last push | Score |
|---|---|---:|---|---|---:|---|---:|
| 1 | [phpmyadmin/phpmyadmin](https://github.com/phpmyadmin/phpmyadmin) | 7,946 | 2001-05 | commit | 305 | 2026-10 | 1189.3 |
| 2 | [moodle/moodle](https://github.com/moodle/moodle) | 7,454 | 2001-11 | commit | 298 | 2026-10 | 1155.1 |
| 3 | [Dolibarr/dolibarr](https://github.com/Dolibarr/dolibarr) | 7,680 | 2002-04 | commit | 293 | 2026-10 | 1138.7 |
| 4 | [dompdf/dompdf](https://github.com/dompdf/dompdf) | 11,194 | 2005-01 | commit | 260 | 2026-09 | 1053.3 |
| 5 | [sebastianbergmann/phpunit](https://github.com/sebastianbergmann/phpunit) | 20,061 | 2006-06 | commit | 243 | 2026-10 | 1045.8 |
| 6 | [glpi-project/glpi](https://github.com/glpi-project/glpi) | 6,420 | 2004-01 | commit | 272 | 2026-10 | 1036.1 |
| 7 | [cakephp/cakephp](https://github.com/cakephp/cakephp) | 8,789 | 2005-05 | commit | 257 | 2026-10 | 1011.8 |
| 8 | [Piwigo/Piwigo](https://github.com/Piwigo/Piwigo) | 3,866 | 2003-05 | commit | 281 | 2026-09 | 1007.1 |
| 9 | [pfsense/pfsense](https://github.com/pfsense/pfsense) | 5,747 | 2004-11 | commit | 263 | 2026-03 | 987.9 |
| 10 | [revive-adserver/revive-adserver](https://github.com/revive-adserver/revive-adserver) | 1,505 | 2000-12 | commit | 310 | 2026-10 | 984.5 |
| 11 | [doctrine/dbal](https://github.com/doctrine/dbal) | 9,704 | 2006-04 | commit | 246 | 2026-09 | 979.2 |
| 12 | [roundcube/roundcubemail](https://github.com/roundcube/roundcubemail) | 7,202 | 2005-09 | commit | 252 | 2026-09 | 972.7 |
| 13 | [openemr/openemr](https://github.com/openemr/openemr) | 5,489 | 2005-03 | commit | 259 | 2026-10 | 967.6 |
| 14 | [Cacti/cacti](https://github.com/Cacti/cacti) | 1,872 | 2002-05 | commit | 292 | 2026-10 | 957 |
| 15 | [PHPMailer/PHPMailer](https://github.com/PHPMailer/PHPMailer) | 22,302 | 2008-08 | commit | 218 | 2026-09 | 947 |
| 16 | [doctrine/annotations](https://github.com/doctrine/annotations) | 6,728 | 2006-04 | commit | 246 | 2025-12 | 940.1 |
| 17 | [joomla/joomla-cms](https://github.com/joomla/joomla-cms) | 5,143 | 2005-09 | commit | 253 | 2026-10 | 937.2 |
| 18 | [doctrine/common](https://github.com/doctrine/common) | 5,799 | 2006-04 | commit | 246 | 2026-06 | 924.3 |
| 19 | [nextcloud/server](https://github.com/nextcloud/server) | 36,974 | 2010-03 | commit | 199 | 2026-10 | 907.7 |
| 20 | [symfony/symfony](https://github.com/symfony/symfony) | 31,164 | 2010-01 | GitHub | 201 | 2026-10 | 902.6 |
| 21 | [TestLinkOpenSourceTRMS/testlink-code](https://github.com/TestLinkOpenSourceTRMS/testlink-code) | 1,618 | 2003-10 | commit | 276 | 2025-12 | 884.2 |
| 22 | [phpseclib/phpseclib](https://github.com/phpseclib/phpseclib) | 5,597 | 2007-06 | commit | 232 | 2026-09 | 868.3 |
| 23 | [ezyang/htmlpurifier](https://github.com/ezyang/htmlpurifier) | 3,350 | 2006-04 | commit | 246 | 2026-09 | 865.6 |
| 24 | [danielmiessler/SecLists](https://github.com/danielmiessler/SecLists) | 73,874 | 2012-02 | GitHub | 175 | 2026-10 | 853.9 |
| 25 | [mockery/mockery](https://github.com/mockery/mockery) | 10,724 | 2009-02 | GitHub | 211 | 2026-09 | 851 |
| 26 | [doctrine/inflector](https://github.com/doctrine/inflector) | 11,337 | 2009-07 | commit | 206 | 2026-06 | 835.7 |
| 27 | [composer/composer](https://github.com/composer/composer) | 29,537 | 2011-04 | commit | 186 | 2026-10 | 830.9 |
| 28 | [sebastianbergmann/php-code-coverage](https://github.com/sebastianbergmann/php-code-coverage) | 8,928 | 2009-05 | commit | 208 | 2026-10 | 822.2 |
| 29 | [guzzle/guzzle](https://github.com/guzzle/guzzle) | 23,452 | 2011-02 | GitHub | 187 | 2026-09 | 817.6 |
| 30 | [Seldaek/monolog](https://github.com/Seldaek/monolog) | 21,402 | 2011-02 | commit | 187 | 2026-10 | 811.7 |
| 31 | [mrclay/minify](https://github.com/mrclay/minify) | 2,994 | 2007-05 | commit | 233 | 2026-02 | 810 |
| 32 | [matomo-org/matomo](https://github.com/matomo-org/matomo) | 21,921 | 2011-03 | GitHub | 186 | 2026-10 | 807.7 |
| 33 | [doctrine/collections](https://github.com/doctrine/collections) | 5,977 | 2008-12 | commit | 213 | 2026-09 | 805.9 |
| 34 | [doctrine/cache](https://github.com/doctrine/cache) | 7,854 | 2009-07 | commit | 207 | 2025-10 | 805.6 |
| 35 | [yiisoft/yii](https://github.com/yiisoft/yii) | 4,824 | 2008-07 | commit | 218 | 2026-09 | 804.3 |
| 36 | [propelorm/Propel2](https://github.com/propelorm/Propel2) | 1,268 | 2005-04 | commit | 258 | 2026-08 | 799.9 |
| 37 | [twigphp/Twig](https://github.com/twigphp/Twig) | 8,371 | 2009-10 | commit | 204 | 2026-09 | 799.4 |
| 38 | [doctrine/orm](https://github.com/doctrine/orm) | 10,166 | 2010-04 | GitHub | 198 | 2026-10 | 792.7 |
| 39 | [simplepie/simplepie](https://github.com/simplepie/simplepie) | 1,576 | 2006-03 | commit | 247 | 2026-07 | 789.3 |
| 40 | [predis/predis](https://github.com/predis/predis) | 7,781 | 2009-11 | GitHub | 203 | 2026-10 | 788.9 |
| 41 | [nikic/PHP-Parser](https://github.com/nikic/PHP-Parser) | 17,471 | 2011-04 | commit | 185 | 2026-09 | 786.7 |
| 42 | [slimphp/Slim](https://github.com/slimphp/Slim) | 12,277 | 2010-09 | GitHub | 192 | 2026-10 | 786.5 |
| 43 | [sebastianbergmann/php-file-iterator](https://github.com/sebastianbergmann/php-file-iterator) | 7,478 | 2009-11 | GitHub | 203 | 2026-10 | 785.8 |
| 44 | [owncloud/core](https://github.com/owncloud/core) | 8,834 | 2010-03 | commit | 199 | 2026-09 | 784.2 |
| 45 | [sebastianbergmann/php-text-template](https://github.com/sebastianbergmann/php-text-template) | 7,427 | 2009-11 | commit | 202 | 2026-10 | 783.3 |
| 46 | [phoronix-test-suite/phoronix-test-suite](https://github.com/phoronix-test-suite/phoronix-test-suite) | 3,148 | 2008-04 | commit | 222 | 2026-09 | 776.4 |
| 47 | [roots/sage](https://github.com/roots/sage) | 13,281 | 2011-01 | GitHub | 188 | 2026-06 | 775.5 |
| 48 | [drupal/drupal](https://github.com/drupal/drupal) | 4,290 | 2009-01 | GitHub | 213 | 2026-10 | 773.2 |
| 49 | [WordPress/WordPress](https://github.com/WordPress/WordPress) | 21,449 | 2011-12 | GitHub | 178 | 2026-10 | 771.1 |
| 50 | [yiisoft/yii2](https://github.com/yiisoft/yii2) | 14,286 | 2011-04 | commit | 186 | 2026-09 | 771.1 |
| 51 | [vrana/adminer](https://github.com/vrana/adminer) | 7,915 | 2010-04 | GitHub | 197 | 2026-09 | 768.9 |
| 52 | [abraham/twitteroauth](https://github.com/abraham/twitteroauth) | 4,298 | 2009-02 | GitHub | 211 | 2026-09 | 768.4 |
| 53 | [sebastianbergmann/php-timer](https://github.com/sebastianbergmann/php-timer) | 7,738 | 2010-05 | commit | 197 | 2026-10 | 765.3 |
| 54 | [chriskacerguis/codeigniter-restserver](https://github.com/chriskacerguis/codeigniter-restserver) | 4,865 | 2009-06 | GitHub | 207 | 2026-05 | 764.8 |
| 55 | [kimai/kimai](https://github.com/kimai/kimai) | 5,054 | 2009-08 | commit | 206 | 2026-10 | 762.4 |
| 56 | [symfony/http-foundation](https://github.com/symfony/http-foundation) | 8,623 | 2010-08 | commit | 193 | 2026-10 | 761 |
| 57 | [symfony/event-dispatcher](https://github.com/symfony/event-dispatcher) | 8,533 | 2010-08 | commit | 193 | 2026-09 | 760.1 |
| 58 | [phingofficial/phing](https://github.com/phingofficial/phing) | 1,166 | 2006-02 | commit | 248 | 2026-10 | 760 |
| 59 | [symfony/http-kernel](https://github.com/symfony/http-kernel) | 8,104 | 2010-08 | commit | 193 | 2026-09 | 755.8 |
| 60 | [symfony/css-selector](https://github.com/symfony/css-selector) | 7,419 | 2010-08 | commit | 193 | 2026-08 | 748.4 |
| 61 | [laravel/framework](https://github.com/laravel/framework) | 34,944 | 2013-01 | GitHub | 165 | 2026-10 | 748.1 |
| 62 | [symfony/console](https://github.com/symfony/console) | 9,813 | 2011-02 | GitHub | 187 | 2026-09 | 747.5 |
| 63 | [dokuwiki/dokuwiki](https://github.com/dokuwiki/dokuwiki) | 4,726 | 2010-01 | GitHub | 201 | 2026-09 | 738.2 |
| 64 | [symfony/finder](https://github.com/symfony/finder) | 8,432 | 2011-02 | GitHub | 187 | 2026-09 | 735.1 |
| 65 | [woocommerce/woocommerce](https://github.com/woocommerce/woocommerce) | 10,535 | 2011-08 | GitHub | 182 | 2026-10 | 731.1 |
| 66 | [doctrine/migrations](https://github.com/doctrine/migrations) | 4,765 | 2010-03 | GitHub | 198 | 2026-06 | 729.3 |
| 67 | [magento/magento2](https://github.com/magento/magento2) | 12,196 | 2011-11 | GitHub | 178 | 2026-10 | 727.4 |
| 68 | [Respect/Validation](https://github.com/Respect/Validation) | 6,031 | 2010-09 | GitHub | 192 | 2026-10 | 726.9 |
| 69 | [symfony/routing](https://github.com/symfony/routing) | 7,612 | 2011-02 | GitHub | 187 | 2026-09 | 726.8 |
| 70 | [symfony/process](https://github.com/symfony/process) | 7,455 | 2011-02 | GitHub | 187 | 2026-09 | 725.1 |
| 71 | [FOGProject/fogproject](https://github.com/FOGProject/fogproject) | 1,679 | 2008-02 | commit | 224 | 2026-10 | 721.1 |
| 72 | [phpmd/phpmd](https://github.com/phpmd/phpmd) | 2,457 | 2009-01 | commit | 212 | 2026-10 | 720.3 |
| 73 | [tijsverkoyen/CssToInlineStyles](https://github.com/tijsverkoyen/CssToInlineStyles) | 5,823 | 2010-10 | GitHub | 191 | 2026-01 | 719.9 |
| 74 | [phalcon/cphalcon](https://github.com/phalcon/cphalcon) | 10,820 | 2011-11 | GitHub | 178 | 2026-10 | 718.8 |
| 75 | [doctrine/DoctrineBundle](https://github.com/doctrine/DoctrineBundle) | 4,831 | 2010-07 | commit | 195 | 2026-09 | 717.9 |
| 76 | [php-imagine/Imagine](https://github.com/php-imagine/Imagine) | 4,471 | 2010-05 | GitHub | 196 | 2026-06 | 716.9 |
| 77 | [symfony/translation](https://github.com/symfony/translation) | 6,599 | 2011-02 | GitHub | 187 | 2026-09 | 715.2 |
| 78 | [KnpLabs/snappy](https://github.com/KnpLabs/snappy) | 4,475 | 2010-06 | GitHub | 195 | 2026-07 | 713.7 |
| 79 | [PHP-CS-Fixer/PHP-CS-Fixer](https://github.com/PHP-CS-Fixer/PHP-CS-Fixer) | 13,557 | 2012-05 | GitHub | 173 | 2026-09 | 712.9 |
| 80 | [opencart/opencart](https://github.com/opencart/opencart) | 8,207 | 2011-08 | GitHub | 182 | 2026-10 | 712.4 |
| 81 | [briannesbitt/Carbon](https://github.com/briannesbitt/Carbon) | 16,596 | 2012-09 | commit | 169 | 2026-10 | 712.3 |
| 82 | [silexphp/Pimple](https://github.com/silexphp/Pimple) | 2,662 | 2009-06 | GitHub | 208 | 2026-03 | 710.8 |
| 83 | [Sylius/Sylius](https://github.com/Sylius/Sylius) | 8,546 | 2011-09 | commit | 181 | 2026-09 | 710.5 |
| 84 | [phpDocumentor/phpDocumentor](https://github.com/phpDocumentor/phpDocumentor) | 4,348 | 2010-07 | GitHub | 195 | 2026-09 | 708.8 |
| 85 | [pheanstalk/pheanstalk](https://github.com/pheanstalk/pheanstalk) | 1,920 | 2008-10 | GitHub | 216 | 2026-09 | 707.8 |
| 86 | [doctrine/DoctrineMigrationsBundle](https://github.com/doctrine/DoctrineMigrationsBundle) | 4,301 | 2010-07 | commit | 195 | 2026-08 | 707.7 |
| 87 | [php-amqplib/php-amqplib](https://github.com/php-amqplib/php-amqplib) | 4,602 | 2010-09 | commit | 193 | 2026-09 | 705.3 |
| 88 | [FreshRSS/FreshRSS](https://github.com/FreshRSS/FreshRSS) | 16,201 | 2012-10 | commit | 167 | 2026-10 | 704.3 |
| 89 | [Behat/Behat](https://github.com/Behat/Behat) | 3,973 | 2010-06 | commit | 195 | 2026-09 | 702 |
| 90 | [ramsey/uuid](https://github.com/ramsey/uuid) | 12,630 | 2012-07 | GitHub | 171 | 2026-09 | 700.8 |
| 91 | [gabordemooij/redbean](https://github.com/gabordemooij/redbean) | 2,313 | 2009-05 | GitHub | 208 | 2026-07 | 700.1 |
| 92 | [symfony/dependency-injection](https://github.com/symfony/dependency-injection) | 4,165 | 2010-08 | commit | 193 | 2026-09 | 699.9 |
| 93 | [simplesamlphp/simplesamlphp](https://github.com/simplesamlphp/simplesamlphp) | 1,143 | 2007-09 | commit | 229 | 2026-10 | 698.9 |
| 94 | [bobthecow/mustache.php](https://github.com/bobthecow/mustache.php) | 3,285 | 2010-03 | GitHub | 198 | 2026-07 | 697.8 |
| 95 | [doctrine-extensions/DoctrineExtensions](https://github.com/doctrine-extensions/DoctrineExtensions) | 4,137 | 2010-09 | GitHub | 193 | 2026-09 | 697.7 |
| 96 | [symfony/dom-crawler](https://github.com/symfony/dom-crawler) | 4,027 | 2010-08 | commit | 193 | 2026-08 | 697.1 |
| 97 | [Kunena/Kunena-Forum](https://github.com/Kunena/Kunena-Forum) | 1,729 | 2008-11 | commit | 214 | 2026-10 | 694.5 |
| 98 | [bobthecow/psysh](https://github.com/bobthecow/psysh) | 9,833 | 2012-04 | commit | 174 | 2026-09 | 693.1 |
| 99 | [symfony/yaml](https://github.com/symfony/yaml) | 3,838 | 2010-08 | commit | 193 | 2026-10 | 693 |
| 100 | [YOURLS/YOURLS](https://github.com/YOURLS/YOURLS) | 12,251 | 2012-08 | GitHub | 169 | 2026-09 | 692.1 |

The columns:

* **#**: position by score.
* **Project**: `owner/name` on GitHub.
* **Stars**: `stargazers_count` when the population was collected.
* **Started**: the earliest date available for the project.
* **Source**: `commit` where the first commit in the BigQuery export was earlier than the GitHub date, `GitHub` where the export had nothing and the repository creation date had to do.
* **Months**: from **Started** to the collection date.
* **Last push**: the most recent push to any branch, which is what "still running" is checked against.
* **Score**: `months × log10(stars)`.

Sixty of these hundred rows carry a first commit date and forty do not, so the **Source** column is not decoration. A `GitHub` row older than 2008 is understated, by an unknown amount.

## How the ranking works

`score = months × log10(stars)`.

Age alone would return a graveyard and stars alone would return whatever is fashionable, so the score holds both. The logarithm is not decoration either. Stars in this population span three orders of magnitude while months span less than one, so a plain `months × stars` is decided by the star count and stops measuring longevity: on that formula `coollabsio/coolify`, started in 2021, outranks `phpmyadmin/phpmyadmin`. Compressing the star scale puts age back in charge and leaves stars as a weight on relevance.

Three filters run before scoring. Archived repositories are out, because finished is not the same as long running. Forks are out, because they inherit a parent's history. Anything without a push in the last 12 months is out. Of the 1,553 repositories collected, 135 were archived, 348 had gone quiet, and 1,070 remained.

## What is wrong with these numbers

**Two thirds of the ranking still rests on the GitHub creation date.** The BigQuery export covers 640 of the 1,553 repositories collected, and an earlier first commit was found for 202 of the 1,070 eligible, which is 19%. The rest keep the date their repository was created on GitHub, a site that opened in 2008. Corrected rows rise and uncorrected rows fall, so the gap between them is an artefact of coverage rather than of age.

**The projects that gap hurts most are the famous ones.** `drupal/drupal`, `WordPress/WordPress`, `phpbb/phpbb`, `PrestaShop/PrestaShop` and `smarty-php/smarty` are absent from the export entirely, so they sit at #48, #49, #133, #135 and #491 on GitHub dates of 2009 to 2014 against real histories running back to 2000 and earlier. Every one of them belongs higher. `bigquery-public-data.github_repos` only includes repositories GitHub classifies as open source through its License API, and its snapshot is from 2025-10-14.

**Even a first commit is a floor.** phpMyAdmin starts here in 2001-05 against a project that began in 1998: the CVS history reaches back only as far as the import carried it. No date in this table is earlier than the truth.

**Linguist decides what counts as PHP.** It picks the language with the most bytes in a repository, which would leave out a PHP back end sitting behind a larger JavaScript front end.

**Stars measure attention, not use.** They accumulate and are rarely withdrawn, so an old project carries stars collected over years while a new one does not, which the score already rewards through months. Nothing here measures installs, downloads or production use.

**The star floor is a choice.** At 1,000 stars the population is 1,553 projects. A lower floor would admit long-lived and genuinely used projects that never became popular; a higher one would drop `revive-adserver` from the table above. The floor is defensible, not neutral.

**The table is a tenth of what was ranked.** 1,553 repositories were collected, 1,070 passed the filters and 1,000 were ranked and kept. Reading only the first 100 is a choice about length, not a property of the data.

**One crawl of the API, one day.** Stars and last-push dates move. The population was collected on 2026-10-02 and nothing here is a time series.

Back to the [reports index](README.md) or the [source list](../raw-data/sources.md).
