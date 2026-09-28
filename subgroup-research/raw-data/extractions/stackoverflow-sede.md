# `stack-exchange-data-explorer.stackoverflow`

**Source:** [Stack Exchange Data Explorer](../sources.md#12-stack-exchange-data-explorer). [Open a new query](https://data.stackexchange.com/stackoverflow/query/new).

**data freshness: 2026-08-29 23:59:12.000 UTC, last check 2026-09-04 07:00:00.000 UTC**

The public Stack Overflow database, queryable through Stack Exchange Data Explorer: questions, answers, comments, users, votes, tags and edit history, with original timestamps. It covers the site from 2008 to now and is refreshed every week, so unlike the BigQuery copy it has no gap in the recent years.

It suits measuring developer interest in a technology over time, identifying recurring problems during adoption, and gauging the health of an ecosystem through the volume and quality of discussion.

The engine is SQL Server, so the dialect is T-SQL and the schema follows the public data dump: table names in PascalCase, and tags reached through `PostTags` instead of being read off the post itself.

### Table structure

| Table | Granularity | Main fields |
|---|---|---|
| `Posts` | one row per post, questions and answers alike | `Id`, `PostTypeId`, `ParentId`, `AcceptedAnswerId`, `Title`, `Body`, `Tags`, `Score`, `ViewCount`, `AnswerCount`, `CreationDate`, `OwnerUserId` |
| `PostTags` | one row per tag applied to a post | `PostId`, `TagId` |
| `Tags` | one row per tag | `Id`, `TagName`, `Count`, `ExcerptPostId`, `WikiPostId` |
| `Comments` | one row per comment | `Id`, `PostId`, `Text`, `Score`, `CreationDate`, `UserId` |
| `Users` | one row per user | `Id`, `DisplayName`, `Reputation`, `Location`, `AboutMe`, `CreationDate`, `LastAccessDate`, `UpVotes`, `DownVotes` |
| `Votes` | one row per vote | `Id`, `PostId`, `VoteTypeId`, `CreationDate` |
| `Badges` | one row per badge awarded | `Id`, `Name`, `UserId`, `Date`, `Class`, `TagBased` |
| `PostHistory` | one row per revision | `Id`, `PostId`, `PostHistoryTypeId`, `CreationDate`, `Text` |
| `PostLinks` | one row per link between posts | `Id`, `PostId`, `RelatedPostId`, `LinkTypeId` |

> `PostTypeId = 1` is a question and `PostTypeId = 2` an answer. `Posts.Tags` does carry the tags as a single string, but joining through `PostTags` and `Tags` is the reliable way to match a tag exactly.
> [!important]
> the query reported here needs to be reasoned and refined

```sql
WITH Totals AS (
  SELECT
    DATEADD(QUARTER, DATEDIFF(QUARTER, 0, CreationDate), 0) AS Quarter,
    COUNT(*) AS Total
  FROM Posts
  WHERE PostTypeId = 1
  GROUP BY DATEADD(QUARTER, DATEDIFF(QUARTER, 0, CreationDate), 0)
),

Php AS (
  SELECT
    DATEADD(QUARTER, DATEDIFF(QUARTER, 0, p.CreationDate), 0) AS Quarter,
    COUNT(*) AS PhpQuestions
  FROM Posts p
  INNER JOIN PostTags pt ON pt.PostId = p.Id
  INNER JOIN Tags t      ON t.Id = pt.TagId
  WHERE p.PostTypeId = 1
    AND t.TagName = ##TagName:string?php##
  GROUP BY DATEADD(QUARTER, DATEDIFF(QUARTER, 0, p.CreationDate), 0)
)

SELECT
  CONVERT(date, t.Quarter) AS [Quarter],
  p.PhpQuestions,
  t.Total,
  ROUND(100.0 * p.PhpQuestions / t.Total, 2) AS SharePct
FROM Totals t
INNER JOIN Php p ON p.Quarter = t.Quarter
WHERE t.Quarter >= '2000-01-01'
ORDER BY t.Quarter;

```

## Results

The numbers this query returned are in [PHP's share of Stack Overflow questions](../../reports/php-share-of-stackoverflow.md), next to the same figures taken from [`bigquery-public-data.stackoverflow`](stackoverflow-bigquery.md), which measures the same thing but stops in 2022.

Back to the [source list](../sources.md), the [list of extractions](../sources.md#2-extractions) or the [reports index](../../reports/README.md).
