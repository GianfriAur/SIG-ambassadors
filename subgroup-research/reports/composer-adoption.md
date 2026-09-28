# Composer adoption across public PHP repositories

**data freshness: 2025-10-14 05:11:35.999 UTC. query run: 2026-09-28**

> These are assumed metrics and should not be treated as final. The query behind them still needs to be reasoned and refined.

**Question:** of the public repositories that contain PHP, how many take part in the Composer ecosystem, and how do they lay their projects out?

**Extractions:** [`bigquery-public-data.github_repos`](../raw-data/extractions/github-repos.md)

## Results

| Metric | Count | Share | What it means |
|---|---:|---:|---|
| PHP repositories (total) | 339,426 | 100% | Every repo Linguist tags as containing PHP, including those where PHP is a minor part of the codebase |
| Ships a `composer.json` | 159,237 | 46.91% of total | Declares its own manifest somewhere outside `vendor/`, the project participates in the Composer ecosystem |
| No `composer.json` | 180,189 | 53.09% of total | No manifest of its own anywhere in the tree |
| Manifest at repository root | 142,299 | 89.36% of Composer users | The conventional layout: `composer.json` sits next to the README |
| Manifest outside the root | 16,938 | 10.64% of Composer users | Manifest lives in a subdirectory. Deliberate in monorepos, accidental in projects whose PHP code is buried under `src/`, `api/` or `web/` |
| `vendor/` committed to Git | 17,443 | 10.95% of Composer users | Dependencies checked into version control instead of being installed from the lock file |
| `vendor/` committed, no manifest | 2,067 | 1.15% of repos without a manifest | Dependencies committed without the file that describes them, these repos use the ecosystem but are invisible to any manifest-based search |

The columns:

* **Metric**: the group of repositories being counted. The groups are not parallel, each one names its own denominator in the **Share** column.
* **Count**: how many repositories fall into that group.
* **Share**: the count as a percentage. Watch the denominator, it changes from row to row: the first three rows are shares of all PHP repositories, the next three of the repositories that ship a manifest, and the last of those that do not.
* **What it means**: what the group describes, and what it does or does not prove.

## How to read this

The denominator is every repository GitHub Linguist tags as containing PHP, and that includes projects where PHP is only a minor part of the codebase. A JavaScript project with one stray `.php` file is counted here, which pushes up the "no `composer.json`" figure.

The snapshot only covers repositories GitHub classifies as open source through its License API, and for file contents only text files under 10 MB. It is not a mirror of GitHub.

The 10.64% with a manifest outside the root mixes two different situations: monorepos, where the layout is deliberate, and projects whose PHP code simply ended up under `src/`, `api/` or `web/`. Paths alone are not enough to tell them apart.

Back to the [reports index](README.md) or the [source list](../raw-data/sources.md).
