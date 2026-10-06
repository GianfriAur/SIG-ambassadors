# LICENSE against composer.json

**population measured: 2026-10-05, live from the GitHub API**

**Question:** a PHP project can state its licence in `LICENSE` or in `composer.json`. How often do the two disagree, and how much of PHP does a count built on one of them miss?

**Extractions:** [Licence declared in `LICENSE` against licence declared in `composer.json`](../raw-data/extractions/licence-file-vs-composer.md)

## The answer

| Outcome | Share of PHP repositories with a detected licence |
|---|---:|
| the two sources agree | 38.0% |
| they disagree | 2.2% |
| no `composer.json` at all | 48.2% |
| `composer.json` with no `license` key | 10.7% |
| `composer.json` declaring free text, not SPDX | 0.8% |

Weighted over 918,553 repositories.

**Outright contradiction is rare: 2.2%.** Two sources that both declare usually declare the same thing.

**Invisibility is not rare: 59.7%.** Add the three absences together and almost three PHP repositories in five carry a licence GitHub can read and tell Packagist nothing at all. They are not disagreeing; they are not present.

That is the finding. A licence count built on `composer.json` and one built on `LICENSE` do not disagree about PHP so much as describe different populations, one roughly two fifths the size of the other.

## By licence

| Licence | Population | Sampled | Agree | Disagree | No `composer.json` | No `license` key | Not SPDX |
|---|---:|---:|---:|---:|---:|---:|---:|
| `mit` | 552,930 | 400 | 50.0% | 0.2% | 38.2% | 11.4% | 0.2% |
| `gpl-3.0` | 121,420 | 400 | 13.6% | 8.9% | 70.5% | 4.3% | 2.6% |
| `gpl-2.0` | 104,641 | 400 | 12.4% | 0.7% | 81.9% | 4.2% | 0.8% |
| `apache-2.0` | 63,963 | 400 | 11.5% | 4.0% | 48.6% | 34.1% | 1.8% |
| `bsd-3-clause` | 34,543 | 400 | 75.0% | 3.3% | 16.4% | 2.7% | 1.9% |
| `agpl-3.0` | 14,058 | 400 | 20.0% | 13.2% | 60.3% | 4.6% | 1.9% |
| `lgpl-3.0` | 7,158 | 400 | 42.2% | 6.7% | 47.3% | 3.1% | 0.7% |
| `unlicense` | 6,708 | 333 | 12.2% | 11.4% | 61.1% | 13.2% | 2.0% |
| `lgpl-2.1` | 3,394 | 337 | 35.9% | 6.0% | 52.2% | 4.7% | 1.0% |
| `bsd-2-clause` | 3,322 | 360 | 38.8% | 4.8% | 46.0% | 7.5% | 2.3% |
| `cc0-1.0` | 2,915 | 274 | 5.6% | 9.2% | 78.0% | 6.8% | 0.4% |
| `mpl-2.0` | 2,474 | 321 | 25.1% | 12.3% | 55.2% | 6.5% | 0.9% |
| `isc` | 771 | 283 | 41.2% | 6.5% | 46.8% | 5.5% | 0.0% |
| `artistic-2.0` | 141 | 141 | 1.4% | 7.1% | 85.8% | 4.3% | 0.7% |
| `epl-1.0` | 115 | 115 | 17.4% | 10.4% | 69.6% | 1.7% | 0.9% |
| **weighted total** | **918,553** | **4964** | **38.0%** | **2.2%** | **48.2%** | **10.7%** | **0.8%** |

The columns:

* **Licence**: the licence GitHub detected from the repository's licence file.
* **Population**: repositories GitHub classifies as PHP carrying that licence, live at the time of measurement.
* **Sampled**: how many were actually fetched and compared, across the four star bands.
* The five outcome columns are weighted estimates for the whole population, not raw sample rates. They sum to 100% less a fraction of a percent of unparseable files.

`gpl-2.0` is the extreme: **81.9% of GPL-2.0 PHP repositories have no `composer.json` at all.** `artistic-2.0` reaches 85.8% and `cc0-1.0` 78.0%. At the other end `bsd-3-clause` agrees 75.0% of the time, which is what a population of ordinary Composer libraries looks like.

`apache-2.0` is odd in a different way: 34.1% are Composer packages that declare no licence at all, three times the rate of any other licence here. Nothing in this data explains it.

Disagreement is highest for `agpl-3.0` at 13.2%, `mpl-2.0` at 12.3% and `unlicense` at 11.4%. For AGPL that is the most consequential disagreement available, since the other side is usually MIT.

## By popularity

The same measurement split by stars instead of by licence, aggregated across all fifteen.

| Stars | Population | Sampled | Agree | Disagree | No `composer.json` | No `license` key |
|---|---:|---:|---:|---:|---:|---:|
| 100 or more | 8,280 | 876 | 83.5% | 2.1% | 11.9% | 2.0% |
| 10 to 99 | 36,781 | 1239 | 65.0% | 1.9% | 25.5% | 7.4% |
| 1 to 9 | 210,878 | 1373 | 55.9% | 3.5% | 34.0% | 5.4% |
| none | 662,614 | 1476 | 30.3% | 1.9% | 54.4% | 12.6% |

**Agreement collapses as popularity falls**, from 83.5% to 30.3%, and the reason is almost entirely that the project stops being a Composer package: 11.9% have no `composer.json` at the top, 54.4% at the bottom.

**Disagreement does not move.** It sits between 1.9% and 3.5% in every band. Contradicting yourself is not a habit of unpopular projects; it happens at the same low rate throughout.

The last column moves the other way and is worth noting on its own: among projects that are Composer packages, the share declaring no licence at all rises from 2.0% to 12.6% as popularity falls. So the two failures compound. A project at the bottom of the population is both far less likely to be a Composer package and, if it is one, far more likely to leave the `license` key empty.

## The disagreements worth looking at

The largest projects where both sources declare and the two declarations differ.

| Project | Stars | `LICENSE` says | `composer.json` says |
|---|---:|---|---|
| [crater-invoice-inc/crater](https://github.com/crater-invoice-inc/crater) | 8,350 | `AGPL-3.0` | `MIT` |
| [SuiteCRM/SuiteCRM](https://github.com/SuiteCRM/SuiteCRM) | 5,779 | `AGPL-3.0` | `GPL-3.0` |
| [kuaifan/dootask](https://github.com/kuaifan/dootask) | 5,586 | `AGPL-3.0` | `MIT` |
| [Bubka/2FAuth](https://github.com/Bubka/2FAuth) | 4,170 | `AGPL-3.0` | `MIT` |
| [LinkStackOrg/LinkStack](https://github.com/LinkStackOrg/LinkStack) | 3,889 | `AGPL-3.0` | `GPL-3.0-or-later` |
| [guanguans/favorite-link](https://github.com/guanguans/favorite-link) | 3,345 | `GPL-3.0` | `MIT` |
| [changeweb/Unifiedtransform](https://github.com/changeweb/Unifiedtransform) | 3,002 | `GPL-3.0` | `MIT` |
| [HDInnovations/UNIT3D](https://github.com/HDInnovations/UNIT3D) | 2,432 | `AGPL-3.0` | `MIT` |
| [chillerlan/php-qrcode](https://github.com/chillerlan/php-qrcode) | 2,391 | `Apache-2.0` | `MIT+Apache-2.0` |
| [mylxsw/wizard](https://github.com/mylxsw/wizard) | 2,267 | `Apache-2.0` | `MIT` |
| [dreeveapp/dreeve](https://github.com/dreeveapp/dreeve) | 2,131 | `AGPL-3.0` | `GPL-3.0-or-later` |
| [nilsteampassnet/TeamPass](https://github.com/nilsteampassnet/TeamPass) | 1,831 | `GPL-3.0` | `GPL-3.0-only+GPL-2.0-only+AGPL-3.0-only+LGPL-2.1-only+LGPL-3.0-or-later+LGPL-3.0-only+GPL-3.0-or-later+GPL-3.0-only+LGPL-2.1-or-later` |

`AGPL-3.0` against `MIT` is the widest gap two files can state about the same code: the strongest copyleft against the most permissive licence in common use.

`leenooks/phpLDAPadmin`, found earlier in the same way, shows the usual mechanism. Its `composer.json` still reads `"name": "laravel/laravel"` with the Laravel skeleton description. Somebody ran `composer create-project`, built an application on top, and never touched the metadata. The MIT in that file is Laravel's.

## Free text instead of an identifier

A smaller group declares a licence in a form no parser resolves: `GNU General Public License v3.0` on `elementor/elementor`, `GPL v2` on `dokuwiki/dokuwiki`, `GNU Public License` on `joomla/joomla-platform`, `(Apache-2.0 or GPL-2.0)` on `tchwork/utf8`. At 0.8% these barely move the totals, but each one is a project that believes it has declared a licence and has not, as far as any tool is concerned.

## What is wrong with these numbers

**This is a sample.** 4,964 repositories out of 918,553, around 0.5%. Each licence and band cell holds at most 100 observations, so a cell rate carries roughly five points of sampling error and the small licences carry more. `epl-1.0` rests on 115 repositories in total and `artistic-2.0` on 141.

**The population is repositories GitHub detected a licence for.** Projects with no licence file at all are outside it entirely, so this report says nothing about how much PHP is unlicensed.

**Dual licences are counted as disagreements.** `composer.json` allows an array, and the comparison reduces it to the first recognised family, so a project offered under both MIT and Apache-2.0 that GitHub reports as Apache-2.0 is scored as disagreeing. Measured against the collected examples this is 0.9% of disagreements, three cases in 324.

**Live figures move.** Star counts and licence detections change daily, and the population was measured on 2026-10-05.

Back to the [reports index](README.md) or the [source list](../raw-data/sources.md).
