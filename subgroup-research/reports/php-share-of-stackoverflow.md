# PHP's share of Stack Overflow questions

> These are assumed metrics and should not be treated as final. The queries behind them still need to be reasoned and refined.

**Question:** how has the volume of PHP questions on Stack Overflow, and PHP's share of all questions, moved over time?

**Extractions:**

* [`stack-exchange-data-explorer.stackoverflow`](../raw-data/extractions/stackoverflow-sede.md), covering 2008 to today and refreshed weekly
* [`bigquery-public-data.stackoverflow`](../raw-data/extractions/stackoverflow-bigquery.md), covering 2008 to Q3 2022 and frozen there

Both extractions measure the same thing. The Data Explorer series is the one to quote. The BigQuery series is kept because a lot of published analysis is built on that copy, and because the difference between the two is worth knowing about.

## Columns

The four tables below use the same columns.

* **Quarter** and **Year**: the period a question was created in, taken from its creation timestamp. Quarters are calendar quarters, so Q1 is January to March.
* **PHP questions**: questions created in that period and tagged exactly `php`. Answers and comments are not counted, and neither are the other PHP-adjacent tags.
* **Total** and **Totals**: every question created in the same period, whatever its tags. This is the denominator.
* **Share %**: PHP questions divided by the total, as a percentage.

The yearly Data Explorer table carries three more.

* **Δ PHP**: the change in PHP questions against the year before, as a percentage.
* **Δ Totals**: the same, for the total.
* **Δ pp**: the change in the share against the year before, in percentage points. Points and not percent: 8.24% down to 7.73% is -0.51 pp, not -6.2%.

## Stack Exchange Data Explorer

### By quarter

| Quarter | PHP Questions | Totals | Share % |
|---|---:|---:|---:|
| 2008 Q3 | 628 | 17,771 | 3.53 |
| 2008 Q4 | 1,571 | 39,359 | 3.99 |
| 2009 Q1 | 2,257 | 53,874 | 4.19 |
| 2009 Q2 | 3,665 | 75,456 | 4.86 |
| 2009 Q3 | 6,395 | 98,271 | 6.51 |
| 2009 Q4 | 7,780 | 112,597 | 6.91 |
| 2010 Q1 | 10,238 | 141,947 | 7.21 |
| 2010 Q2 | 11,629 | 157,794 | 7.37 |
| 2010 Q3 | 14,300 | 185,671 | 7.70 |
| 2010 Q4 | 14,861 | 202,975 | 7.32 |
| 2011 Q1 | 21,032 | 263,740 | 7.97 |
| 2011 Q2 | 23,545 | 294,743 | 7.99 |
| 2011 Q3 | 25,350 | 309,008 | 8.20 |
| 2011 Q4 | 24,767 | 312,579 | 7.92 |
| 2012 Q1 | 29,619 | 371,602 | 7.97 |
| 2012 Q2 | 31,364 | 391,724 | 8.01 |
| 2012 Q3 | 34,154 | 415,285 | 8.22 |
| 2012 Q4 | 34,251 | 433,301 | 7.90 |
| 2013 Q1 | 39,173 | 484,548 | 8.08 |
| 2013 Q2 | 38,784 | 495,722 | 7.82 |
| 2013 Q3 | 42,354 | 511,523 | 8.28 |
| 2013 Q4 | 43,959 | 524,524 | 8.38 |
| **2014 Q1** | **50,896** | **583,142** | **8.73** |
| 2014 Q2 | 44,652 | 531,152 | 8.41 |
| 2014 Q3 | 40,225 | 505,171 | 7.96 |
| 2014 Q4 | 38,411 | 494,160 | 7.77 |
| 2015 Q1 | 41,588 | 523,490 | 7.94 |
| 2015 Q2 | 43,396 | 562,491 | 7.71 |
| 2015 Q3 | 42,689 | 553,847 | 7.71 |
| 2015 Q4 | 40,504 | 536,076 | 7.56 |
| 2016 Q1 | 44,086 | 570,607 | 7.73 |
| 2016 Q2 | 42,128 | 570,235 | 7.39 |
| 2016 Q3 | 37,635 | 530,067 | 7.10 |
| 2016 Q4 | 35,373 | 512,637 | 6.90 |
| 2017 Q1 | 38,761 | 553,469 | 7.00 |
| 2017 Q2 | 36,806 | 542,909 | 6.78 |
| 2017 Q3 | 33,773 | 519,696 | 6.50 |
| 2017 Q4 | 30,177 | 483,632 | 6.24 |
| 2018 Q1 | 28,814 | 486,414 | 5.92 |
| 2018 Q2 | 26,590 | 484,374 | 5.49 |
| 2018 Q3 | 24,591 | 462,889 | 5.31 |
| 2018 Q4 | 21,321 | 442,271 | 4.82 |
| 2019 Q1 | 22,093 | 456,918 | 4.84 |
| 2019 Q2 | 20,043 | 440,624 | 4.55 |
| 2019 Q3 | 17,960 | 424,916 | 4.23 |
| 2019 Q4 | 17,305 | 433,337 | 3.99 |
| 2020 Q1 | 17,119 | 447,530 | 3.83 |
| 2020 Q2 | 18,960 | 541,085 | 3.50 |
| 2020 Q3 | 15,612 | 456,001 | 3.42 |
| 2020 Q4 | 13,911 | 410,532 | 3.39 |
| 2021 Q1 | 13,823 | 420,181 | 3.29 |
| 2021 Q2 | 12,576 | 398,521 | 3.16 |
| 2021 Q3 | 11,394 | 365,725 | 3.12 |
| 2021 Q4 | 10,237 | 349,957 | 2.93 |
| 2022 Q1 | 9,527 | 356,351 | 2.67 |
| 2022 Q2 | 9,615 | 341,286 | 2.82 |
| 2022 Q3 | 8,927 | 326,929 | 2.73 |
| 2022 Q4 | 7,786 | 311,400 | 2.50 |
| 2023 Q1 | 6,348 | 269,451 | 2.36 |
| 2023 Q2 | 4,655 | 198,332 | 2.35 |
| 2023 Q3 | 4,250 | 175,261 | 2.42 |
| 2023 Q4 | 3,315 | 144,938 | 2.29 |
| 2024 Q1 | 3,257 | 138,251 | 2.36 |
| 2024 Q2 | 2,621 | 114,361 | 2.29 |
| 2024 Q3 | 1,848 | 83,986 | 2.20 |
| 2024 Q4 | 1,375 | 61,921 | 2.22 |
| 2025 Q1 | 1,034 | 48,791 | 2.12 |
| 2025 Q2 | 572 | 28,807 | 1.99 |
| 2025 Q3 | 288 | 17,943 | 1.61 |
| 2025 Q4 | 251 | 14,934 | 1.68 |
| 2026 Q1 | 160 | 10,098 | 1.58 |
| 2026 Q2 | 127 | 6,859 | 1.85 |
| 2026 Q3 * | 47 | 2,660 | 1.77 |

\* Partial quarter: data collected up to 2026-09-02.

### By year

| Year | PHP Questions | Δ PHP | Totals | Δ Totals | Share % | Δ pp |
|---|---:|---:|---:|---:|---:|---:|
| 2008 * | 2,199 | — | 57,130 | — | 3.85 | — |
| 2009 | 20,097 | +813.9% | 340,198 | +495.5% | 5.91 | +2.06 |
| 2010 | 51,028 | +153.9% | 688,387 | +102.3% | 7.41 | +1.51 |
| 2011 | 94,694 | +85.6% | 1,180,070 | +71.4% | 8.02 | +0.61 |
| 2012 | 129,388 | +36.6% | 1,611,912 | +36.6% | 8.03 | +0.00 |
| 2013 | 164,270 | +27.0% | 2,016,317 | +25.1% | 8.15 | +0.12 |
| **2014** | **174,184** | +6.0% | **2,113,625** | +4.8% | **8.24** | +0.09 |
| 2015 | 168,177 | -3.4% | 2,175,904 | +2.9% | 7.73 | -0.51 |
| 2016 | 159,222 | -5.3% | 2,183,546 | +0.4% | 7.29 | -0.44 |
| 2017 | 139,517 | -12.4% | 2,099,706 | -3.8% | 6.64 | -0.65 |
| 2018 | 101,316 | -27.4% | 1,875,948 | -10.7% | 5.40 | -1.24 |
| 2019 | 77,401 | -23.6% | 1,755,795 | -6.4% | 4.41 | -0.99 |
| 2020 | 65,602 | -15.2% | 1,855,148 | +5.7% | 3.54 | -0.87 |
| 2021 | 48,030 | -26.8% | 1,534,384 | -17.3% | 3.13 | -0.41 |
| 2022 | 35,855 | -25.3% | 1,335,966 | -12.9% | 2.68 | -0.45 |
| 2023 | 18,568 | -48.2% | 787,982 | -41.0% | 2.36 | -0.33 |
| 2024 | 9,101 | -51.0% | 398,519 | -49.4% | 2.28 | -0.07 |
| 2025 | 2,145 | -76.4% | 110,475 | -72.3% | 1.94 | -0.34 |
| 2026 * | 334 | -84.4% | 19,617 | -82.2% | 1.70 | -0.24 |

## BigQuery snapshot, for comparison

Covers 2012 onward; the query filters out the earlier years as noisy. Ends at Q3 2022, where the dump stops.

### By quarter

| Quarter | PHP questions | Total | Share % |
|-------------|--------------:|-------------:|---:|
| 2012 Q1     |        30,153 |      376,853 | 8.00 |
| 2012 Q2     |        31,925 |      397,370 | 8.03 |
| 2012 Q3     |        34,599 |      418,704 | 8.26 |
| 2012 Q4     |        34,579 |      436,459 | 7.92 |
| 2013 Q1     |        39,453 |      487,682 | 8.09 |
| 2013 Q2     |        39,119 |      499,706 | 7.83 |
| 2013 Q3     |        42,801 |      516,364 | 8.29 |
| 2013 Q4     |        44,575 |      529,938 | 8.41 |
| **2014 Q1** |    **51,559** |  **588,822** | **8.76** |
| 2014 Q2     |        45,358 |      537,546 | 8.44 |
| 2014 Q3     |        40,840 |      511,556 | 7.98 |
| 2014 Q4     |        38,960 |      499,511 | 7.80 |
| 2015 Q1     |        42,045 |      528,559 | 7.95 |
| 2015 Q2     |        43,935 |      568,055 | 7.73 |
| 2015 Q3     |        43,152 |      558,809 | 7.72 |
| 2015 Q4     |        41,014 |      541,253 | 7.58 |
| 2016 Q1     |        44,647 |      575,649 | 7.76 |
| 2016 Q2     |        42,547 |      574,969 | 7.40 |
| 2016 Q3     |        37,927 |      533,886 | 7.10 |
| 2016 Q4     |        35,675 |      516,298 | 6.91 |
| 2017 Q1     |        39,153 |      557,366 | 7.02 |
| 2017 Q2     |        37,172 |      547,215 | 6.79 |
| 2017 Q3     |        34,081 |      523,400 | 6.51 |
| 2017 Q4     |        30,524 |      488,231 | 6.25 |
| 2018 Q1     |        29,043 |      490,342 | 5.92 |
| 2018 Q2     |        26,806 |      488,334 | 5.49 |
| 2018 Q3     |        24,653 |      465,469 | 5.30 |
| 2018 Q4     |        21,415 |      444,844 | 4.81 |
| 2019 Q1     |        22,210 |      459,576 | 4.83 |
| 2019 Q2     |        20,086 |      443,263 | 4.53 |
| 2019 Q3     |        18,013 |      427,610 | 4.21 |
| 2019 Q4     |        17,344 |      436,484 | 3.97 |
| 2020 Q1     |        17,224 |      451,332 | 3.82 |
| 2020 Q2     |        19,091 |      545,781 | 3.50 |
| 2020 Q3     |        15,714 |      459,909 | 3.42 |
| 2020 Q4     |        14,020 |      414,673 | 3.38 |
| 2021 Q1     |        13,902 |      424,970 | 3.27 |
| 2021 Q2     |        12,678 |      403,943 | 3.14 |
| 2021 Q3     |        11,620 |      376,487 | 3.09 |
| 2021 Q4     |        11,943 |      424,180 | 2.82 |
| 2022 Q1     |        11,412 |      436,766 | 2.61 |
| 2022 Q2     |        11,673 |      427,400 | 2.73 |
| 2022 Q3     |        11,261 |      404,622 | 2.78 |

### By year

| Year      | PHP questions |         Total | Share % |
|-----------|---------------:|--------------:|---:|
| 2012      |        131,256 |     1,629,386 | 8.06 |
| 2013      |        165,948 |     2,033,690 | 8.16 |
| **2014**  |    **176,717** | **2,137,435** | **8.27** |
| 2015      |        170,146 |     2,196,676 | 7.75 |
| 2016      |        160,796 |     2,200,802 | 7.31 |
| 2017      |        140,930 |     2,116,212 | 6.66 |
| 2018      |        101,917 |     1,888,989 | 5.39 |
| 2019      |         77,653 |     1,766,933 | 4.39 |
| 2020      |         66,049 |     1,871,695 | 3.53 |
| 2021      |         50,143 |     1,629,580 | 3.08 |
| 2022*     |         34,346 |     1,268,788 | 2.71 |

\* Partial year: first three quarters only.

## How to read this

The two series do not quite agree, and the Data Explorer one is always the lower of the two. 2014 Q1 is 51,559 questions in the BigQuery dump against 50,896 in Data Explorer, and a gap of about 1% runs through every quarter they share. The BigQuery copy was frozen in late 2022, while Data Explorer is refreshed every week and so no longer counts the questions deleted since. The live figure is the lower one, and it will keep dropping for past quarters as moderation goes on.

The collapse in the totals after 2022 has nothing to do with PHP. Total questions fall 41% in 2023, 49% in 2024 and 72% in 2025, because Stack Overflow's whole question volume dropped once assistants became a common substitute for asking. The absolute PHP counts for those years have to be read against that. PHP falls because the site does, and the share column is what separates the two.

The share is the number to quote. It peaks around 8.2% in 2014, declines to roughly 2.3% by 2023, then flattens out: 2.36%, 2.28%, 1.94%.

The last row of each table is partial. 2026 Q3 only covers data up to 2026-09-02, and the 2026 row of the yearly table is a partial year, so its Δ columns compare a few months against twelve and mean nothing. The same goes for 2008, which starts in August, and for the BigQuery 2022 row, which holds three quarters.

Only questions tagged exactly `php` are counted. `phpunit`, `php-7`, `laravel` and the rest are not in the numerator, even though they are PHP work.

Back to the [reports index](README.md) or the [source list](../raw-data/sources.md).
