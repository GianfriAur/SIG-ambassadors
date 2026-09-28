# `bigquery-public-data.stackoverflow`

**Source:** [Google BigQuery](../sources.md#11-google-bigquery). [Open the dataset in the console](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=stackoverflow&t=posts_questions&page=table).

**data freshness: 2022-11-25 01:03:23.684 UTC, last check 2026-09-01 16:00:00.000 UTC**

A full copy of the public Stack Overflow data dump loaded into BigQuery: questions, answers, comments, users, votes, tags and edit history, with original timestamps. It covers the site from 2008 to Q3 2022.

It suits measuring developer interest in a technology over time, identifying recurring problems during adoption, and gauging the health of an ecosystem through the volume and quality of discussion.

Compared with `github_repos` this is a **small** dataset, tens of gigabytes rather than terabytes, and therefore far cheaper to query.

### Table structure

| Table | Granularity | Main fields |
|---|---|---|
| `posts_questions` | one row per question | `id`, `title`, `body`, `tags`, `score`, `view_count`, `answer_count`, `accepted_answer_id`, `creation_date`, `owner_user_id` |
| `posts_answers` | one row per answer | `id`, `parent_id`, `body`, `score`, `creation_date`, `owner_user_id` |
| `stackoverflow_posts` | all post types in one table | as above, plus `post_type_id` |
| `comments` | one row per comment | `id`, `post_id`, `text`, `score`, `creation_date`, `user_id` |
| `users` | one row per user | `id`, `display_name`, `reputation`, `location`, `about_me`, `creation_date`, `last_access_date`, `up_votes`, `down_votes` |
| `votes` | one row per vote | `id`, `post_id`, `vote_type_id`, `creation_date` |
| `badges` | one row per badge awarded | `id`, `name`, `user_id`, `date`, `class`, `tag_based` |
| `tags` | one row per tag | `id`, `tag_name`, `count`, `excerpt_post_id`, `wiki_post_id` |
| `post_history` | one row per revision | `id`, `post_id`, `post_history_type_id`, `creation_date`, `text` |
| `post_links` | one row per link between posts | `id`, `post_id`, `related_post_id`, `link_type_id` |

> [!important]
> the query reported here needs to be reasoned and refined

```sql
WITH totals AS (
  -- Denominator: all questions. Normalising matters, site-wide volume
  -- shifted a lot over the years, so absolute counts mislead.
  SELECT DATE_TRUNC(DATE(creation_date), QUARTER) AS quarter,
         COUNT(*) AS total_questions
  FROM `bigquery-public-data.stackoverflow.posts_questions`
  GROUP BY quarter  -- DATE() needed: creation_date is a TIMESTAMP
),

php AS (
  -- Numerator: questions tagged 'php'.
  SELECT DATE_TRUNC(DATE(q.creation_date), QUARTER) AS quarter,
         COUNT(*) AS php_questions
  FROM `bigquery-public-data.stackoverflow.posts_questions` AS q,
       -- tags is a '|'-joined string, not an ARRAY
       UNNEST(SPLIT(q.tags, '|')) AS tag
  WHERE tag = 'php'  -- exact match: LIKE '%php%' would catch 'phpunit'
  GROUP BY quarter
)

SELECT
  t.quarter,
  p.php_questions,
  t.total_questions,
  ROUND(100 * p.php_questions / t.total_questions, 2) AS share_pct
FROM totals AS t
JOIN php AS p USING (quarter)  -- INNER: fine for a common tag, not a rare one
WHERE t.quarter >= DATE '2012-01-01'  -- early years are noisy
ORDER BY t.quarter;
```

## Results

The numbers this query returned are in [PHP's share of Stack Overflow questions](../../reports/php-share-of-stackoverflow.md), next to the same figures taken from [`stack-exchange-data-explorer.stackoverflow`](stackoverflow-sede.md), which covers the years this snapshot is missing.

Back to the [source list](../sources.md), the [list of extractions](../sources.md#2-extractions) or the [reports index](../../reports/README.md).
