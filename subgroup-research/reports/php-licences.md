# The licences of the PHP ecosystem

**data freshness: 2025-10-14 05:11:35.999 UTC. query run: 2026-10-02**

> Real query results rather than assumed metrics. Every figure here is a share of **licensed** projects: the dataset holds only repositories GitHub already classified as open source, so projects without a licence are absent rather than counted as zero.

**Question:** under what terms is PHP published, and can a company pick a PHP library off the shelf without calling a lawyer?

**Extractions:** [`bigquery-public-data.github_repos`, licences](../raw-data/extractions/github-repos-licences.md)

## Families

| Family | Projects | Share |
|---|---:|---:|
| permissive | 148,628 | 62.6% |
| copyleft | 88,646 | 37.4% |
| **total** | **237,274** | **100%** |

The columns:

* **Family**: how the licence behaves for someone reusing the code. The mapping is in the extraction.
* **Projects**: repositories where PHP holds the most bytes, which is Linguist's rule and the one GitHub reports as a repository's language.
* **Share**: against the 237,274 in that column.

**Just under two thirds of licensed PHP is permissive.** For anyone asking whether PHP libraries can be used inside a commercial product, that is the answer, though the copyleft third is large enough that the question has to be asked per project rather than assumed.

## Every licence

| Licence | PHP is the main language | Share | Contains any PHP |
|---|---:|---:|---:|
| `MIT` | 113,607 | 47.88% | 161,595 |
| `GPL-2.0` | 54,461 | 22.95% | 69,079 |
| `GPL-3.0` | 24,678 | 10.40% | 35,887 |
| `BSD-3-Clause` | 16,875 | 7.11% | 22,778 |
| `Apache-2.0` | 13,485 | 5.68% | 29,306 |
| `AGPL-3.0` | 4,096 | 1.73% | 6,106 |
| `LGPL-3.0` | 3,188 | 1.34% | 4,193 |
| `BSD-2-Clause` | 2,150 | 0.91% | 3,052 |
| `Unlicense` | 1,644 | 0.69% | 2,295 |
| `LGPL-2.1` | 1,420 | 0.60% | 2,132 |
| `MPL-2.0` | 563 | 0.24% | 1,034 |
| `CC0-1.0` | 509 | 0.21% | 918 |
| `ISC` | 358 | 0.15% | 544 |
| `Artistic-2.0` | 158 | 0.07% | 277 |
| `EPL-1.0` | 82 | 0.03% | 230 |
| **total** | **237,274** | **100%** | **339,426** |

The columns:

* **Licence**: the SPDX identifier. BigQuery stores these as lowercase short names; they are written here in the usual form.
* **PHP is the main language**: repositories where PHP holds the most bytes.
* **Share**: against the 237,274 in that column.
* **Contains any PHP**: repositories with any PHP at all, including projects whose licence was chosen for a JavaScript or Python codebase that happens to ship a `.php` file.

The two definitions disagree by a third, 237,274 against 339,426, which is the measure of how much PHP sits inside projects that are not really PHP projects.

The second column totals exactly the PHP repository count reported by [Composer adoption across public PHP repositories](composer-adoption.md), which reached it through a completely different query against the same dataset. Two independent paths to the same number is the closest thing to a correctness check this folder has.

## What the distribution says

**`MIT` is not a majority.** At 47.9% it is the largest single licence by a wide margin, but more PHP is published under something else than under MIT.

**`GPL-2.0` is the second licence of PHP, at 23.0%.** That is 54,461 repositories, more than double `GPL-3.0`, and it is unusual: `GPL-2.0` has been superseded for eighteen years and new projects rarely choose it. The most likely explanation is WordPress. WordPress is `GPL-2.0`, its plugins and themes are derivative works that inherit those terms, and there are a great many of them. The same project accounts for 66.52% of PHP on the public web in [The PHP applications behind the web](php-applications-on-the-web.md), so one codebase shaping a quarter of PHP's licence distribution is consistent with everything else measured here.

That reading is a hypothesis. Nothing in this data marks a repository as a WordPress plugin, and confirming it would mean looking for WordPress-specific files or headers across those 54,461 repositories.

**The tail is thin.** Below `Apache-2.0` at 5.7%, no licence reaches 2%. Thirteen of the fifteen licences in the table together account for less than a fifth of the population.

## What is wrong with these numbers

**The population is 3.7% of PHP on GitHub, and it is not a random 3.7%.** GitHub holds 6,415,897 repositories where PHP is the main language. `bigquery-public-data.github_repos` holds 237,274 of them. The dataset is a periodically loaded snapshot of a selected subset rather than a mirror, and the selection rule is that GitHub classified the repository as open source through its License API.

**Unlicensed projects are therefore invisible**, not counted as zero. A project with no licence file is absent from the dataset rather than present with an empty value, so this report cannot say what share of PHP is unlicensed and nothing in it should be read as suggesting that share is small.

**Detection is GitHub's.** A project stating its terms in a README, a `composer.json` or a header comment rather than in a licence file is not recognised, and is therefore outside the dataset along with the genuinely unlicensed.

**The snapshot is from 2025-10-14** and is loaded periodically rather than continuously. Licences change rarely, so this matters less here than it would for stars or activity, but the population is a year old.

Back to the [reports index](README.md) or the [source list](../raw-data/sources.md).
