# The first two years of a PHP package

**population collected: 2026-10-20. t=0 is the first release on Packagist**

**Question:** when does a package stop being a repository and start being used? Is there a moment when adoption takes off, and does it come at a different time for packages that succeed?

**Extractions:** [The first two years of a Packagist package](../raw-data/extractions/package-first-two-years.md)

## The three grades

7,259 packages, each with 104 complete weeks of download history from its first release.

| Grade | Packages | Downloads, weeks 1 to 4 | Downloads, weeks 101 to 104 | Week of peak | Weeks at zero |
|---|---:|---:|---:|---:|---:|
| A, adopted | 2,475 | 135 | 5,070 | 96 | 0 |
| B, partly adopted | 2,441 | 55 | 587 | 79 | 4 |
| C, not adopted | 2,343 | 2 | 0 | 11 | 90 |

All figures are medians.

The columns:

* **Grade**: A has 25 or more dependents or 100,000 downloads a month; B is 3 to 24 dependents or 1,000 to 99,999 a month; C has no dependents, under 100 downloads a month, and at least 10 releases. The full rules are in the extraction.
* **Downloads, weeks 1 to 4**: the first month after the package appeared on Packagist.
* **Week of peak**: which of the 104 weeks held the highest download count.
* **Weeks at zero**: how many of the 104 weeks recorded no downloads at all.

**Grade C is not a group of failures.** Its median package spends most of the two years at zero downloads. These packages were never installed by anyone, so showing that they differ from adopted ones proves nothing. The rest of this report compares A against B, where both groups have real downloads.

## A tendency in week one, not a separation

| Group | Packages | p25 | Median | p75 |
|---|---:|---:|---:|---:|
| A on dependents only | 1,857 | 8 | 102 | 545 |
| B, partly adopted | 2,441 | 7 | 55 | 318 |
| C, not adopted | 2,343 | 0 | 2 | 16 |

Downloads in weeks 1 to 4.

The first row is the test that makes this worth reading. Those 1,857 packages are graded A **on dependents alone**, with a download rate below the threshold, so their label never looks at the download series. If the pattern survives there, it is not an artefact of defining success by downloads and then discovering downloads.

It survives, and it is weak. Adopted packages open about six times higher than unadopted ones, but **42.2% of the partly adopted group reaches the adopted median**, against 3.3% of the never-installed. The distributions overlap heavily.

So the first month leans the odds. It does not sort packages into winners and losers.

## The trajectories are the same shape

Normalising every curve by its own level in weeks 101 to 104 removes the difference in scale and leaves only the shape.

| Week | A, adopted | B, partly adopted |
|---|---:|---:|
| 1 | 3.9% | 8.0% |
| 4 | 3.9% | 6.9% |
| 13 | 9.9% | 12.9% |
| 26 | 20.2% | 25.7% |
| 52 | 42.8% | 50.9% |
| 78 | 73.0% | 72.8% |

Median share of the package's own final level, already reached by that week.

The two columns track each other throughout. Both grow along the same path and approach their own ceiling at the same rate.

What differs is only how high that ceiling is:

| Week | A, adopted | B, partly adopted | Ratio |
|---|---:|---:|---:|
| 1 | 22 | 14 | 1.6 |
| 4 | 21 | 9 | 2.3 |
| 13 | 48 | 19 | 2.5 |
| 26 | 95 | 31 | 3.1 |
| 52 | 227 | 64 | 3.5 |
| 78 | 403 | 103 | 3.9 |
| 104 | 659 | 143 | 4.6 |

Median weekly downloads. Across all 104 weeks the ratio has a median of 3.5, a minimum of 1.6 and a maximum of 4.7: **flat**.

## What that means

There is no leap. The gap between a package that gets adopted and one that almost does is present in week one and stays roughly the same for two years.

Whatever decides the outcome happened **before the first release**. Publication is not the beginning of the story; it is the point at which a story already written becomes visible.

One qualification stops this being overstated.

Week-one downloads and the final level move together, but loosely. Their correlation coefficient is **0.34** on 3,083 packages. A correlation coefficient runs from 0, where knowing one number tells you nothing about the other, to 1, where it tells you everything; squaring it gives the share of the variation accounted for, so 0.34 accounts for about 12% and leaves 88% to other causes.

The relationship also flattens. Fitting the two against each other on a logarithmic scale gives a slope of **0.47**, where 1 would mean the opening gap is preserved exactly. At 0.47 it compresses: a package starting ten times ahead ends roughly three times ahead.

The start tilts the outcome. It does not fix it.

## The exceptions, and why they are not encouraging

A median hides the cases that broke the pattern. Two groups were isolated by thresholds taken from the groups themselves: packages that **opened below the partly-adopted median and closed above the adopted median**, and the mirror image, packages that opened above the adopted median and closed below the partly-adopted one.

| | Packages | Share of the cohort |
|---|---:|---:|
| rose from B levels to A levels | 426 | 5.9% |
| fell from A levels to B levels | 443 | 6.1% |

**They are symmetric.** For every package that leapt, another collapsed, in almost exactly equal numbers. That is the signature of noise and regression to the mean, not of a mechanism that lifts good projects. A report that showed only the 426 risers would be telling half a story.

Among the risers the jump can be dated, by looking for the week where the four-week average most exceeds the preceding four weeks.

| Weeks | Risers | Share |
|---|---:|---:|
| 1 to 13 | 107 | 25.2% |
| 14 to 26 | 167 | 39.4% |
| 27 to 52 | 96 | 22.6% |
| 53 to 78 | 29 | 6.8% |
| 79 to 104 | 25 | 5.9% |

Two thirds of the jumps fall in the first six months, with a median at week 19. So when a leap does happen, it happens early, and a package still flat after a year rarely takes off later.

The largest risers:

| Package | Downloads, weeks 1 to 4 | Downloads, weeks 101 to 104 | Jump week |
|---|---:|---:|---:|
| [doctrine/deprecations](https://packagist.org/packages/doctrine/deprecations) | 0 | 5,815,450 | 10 |
| [laminas/laminas-zendframework-bridge](https://packagist.org/packages/laminas/laminas-zendframework-bridge) | 9 | 2,582,227 | 44 |
| [spatie/ignition](https://packagist.org/packages/spatie/ignition) | 0 | 2,363,346 | 36 |
| [psr/simple-cache](https://packagist.org/packages/psr/simple-cache) | 0 | 2,144,474 | 15 |
| [markbaker/matrix](https://packagist.org/packages/markbaker/matrix) | 6 | 1,129,422 | 19 |
| [markbaker/complex](https://packagist.org/packages/markbaker/complex) | 5 | 1,101,632 | 22 |
| [psr/http-client](https://packagist.org/packages/psr/http-client) | 0 | 705,671 | 11 |
| [symfony/polyfill-php82](https://packagist.org/packages/symfony/polyfill-php82) | 45 | 591,490 | 8 |
| [fig/link-util](https://packagist.org/packages/fig/link-util) | 0 | 550,655 | 29 |
| [league/flysystem](https://packagist.org/packages/league/flysystem) | 0 | 412,485 | 16 |
| [google/common-protos](https://packagist.org/packages/google/common-protos) | 9 | 379,752 | 9 |
| [codeception/module-webdriver](https://packagist.org/packages/codeception/module-webdriver) | 9 | 311,216 | 10 |
| [spatie/laravel-data](https://packagist.org/packages/spatie/laravel-data) | 0 | 247,778 | 21 |
| [livewire/livewire](https://packagist.org/packages/livewire/livewire) | 0 | 243,561 | 27 |
| [gecko-packages/gecko-php-unit](https://packagist.org/packages/gecko-packages/gecko-php-unit) | 1 | 217,633 | 59 |

Read the names and the mechanism is visible. `doctrine/deprecations`, `psr/simple-cache`, `psr/http-client`, `symfony/polyfill-php82`, `laminas/laminas-zendframework-bridge`, `fig/link-util`: these are small components split out of large projects, or interface packages. Their jump is the week a widely used parent added them to its own `require`.

The vendor counts say the same. The 426 risers come from 250 distinct vendors, but the ten most frequent account for 31% of them, led by `spryker` with 36 and `typo3` with 28, both of which publish split monorepos.

**So the leap is real but it mostly is not adoption.** It is a package being pulled in by its own ecosystem through somebody else's dependency update. Very little of it looks like a community discovering a project on its merits.

## Does the author explain the head start?

If the outcome is already set at publication, an obvious candidate is the audience the author brought with them.

| Group | Packages | p25 | Median | p75 | Vendors with one package |
|---|---:|---:|---:|---:|---:|
| A on dependents only | 757 | 41 | 115 | 213 | 0.3% |
| B, partly adopted | 736 | 41 | 115 | 259 | 0.5% |
| C, not adopted | 163 | 19 | 62 | 296 | 2.5% |

Packages published by the same vendor, counted today.

| Vendor's packages | Packages | Week-one downloads | Share graded A |
|---|---:|---:|---:|
| 1 | 6 | 8 | 33.3% |
| 2 to 5 | 23 | 5 | 34.8% |
| 6 to 20 | 153 | 21 | 54.2% |
| 21 to 100 | 517 | 33 | 48.5% |
| over 100 | 794 | 40 | 52.0% |

Across the whole range the correlation coefficient is **0.22**, which accounts for under 5% of the variation: weaker still than the week-one relationship above.

The honest reading is narrow. A handful of industrial vendors start somewhere else entirely. For everyone else, being on your first package or your twentieth makes little measurable difference, and the head start remains unexplained by this data.

## What is wrong with these numbers

**The cohort is not a random sample.** Grades A and B are drawn from the registry sorted by dependents and by downloads, because a random sample never reaches them: in 3,900 random packages the highest dependent count observed was 62 against `monolog/monolog`'s 6,809. Grade C is drawn from random pages. The three groups are built by different methods and their relative sizes mean nothing.

**How deep those sorted lists are read is a choice that moves the result, not just its precision.** Reading only the first ranks of each axis fills grade A with the extreme top of the registry and widens every gap in this report; reading deeper fills it with ordinary adopted packages and narrows them. This cohort reads 10,000 ranks per axis, which is roughly where a direct dependent count falls to 25. A report built on a shallower read would describe the top of Packagist while claiming to describe Packagist.

**The vendor counts are today's.** A vendor that grew *because* of the package being studied is credited retroactively. The effect is softened by the signal sitting almost entirely among vendors with over 100 packages, who were in all likelihood already large, but it is not removed.

**Downloads are not people.** A Packagist install is dominated by continuous integration. A package inside a popular CI matrix collects thousands of downloads from a handful of actual users, so the absolute levels measure automation more than adoption. The shapes and ratios are what this report rests on.

**The exceptions are dated with a crude rule.** The jump week is where a four-week average most exceeds the preceding four, requiring at least five downloads a week beforehand so that a rise from one to forty does not register as a fortyfold leap. A package with two separate rises is credited only with the larger.

**Two of the three planned measurements are missing.** Commit history and star history were to be collected alongside downloads. Both need an authenticated GitHub token: `stats/contributors` is capped at 60 requests an hour without one, and starring timestamps are refused outright. They are the measurements that would test what this report can only point at, which is that the outcome is decided before the package is published.

Back to the [reports index](README.md) or the [source list](../raw-data/sources.md).
