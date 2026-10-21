# Request: "next articles" endpoint

**For:** API team · **From:** newsroom (portal.acnnewswire.com) · **Date:** 2026-10-22

## Why we need it

When a reader finishes an article, the newsroom loads the next one below it (infinite scroll). Today the
site works out "next" by fetching the latest 20 articles and finding the current one in that list. That
breaks in three ways:

1. It only works for the latest 20 articles. Open anything older and the feed ends immediately.
2. The browser calls `development.acnnewswire.com` directly, and the API sends no CORS headers, so
   browsers block the call. In production the feed most likely never loads a second article.
3. It cannot follow a company or a sector, which is the reading experience we want.

The newsroom will call the new endpoint from its own server (so CORS stops mattering) and cache the answers.

## The endpoint

```
GET /api/Articles/{artId}/next?mode=latest|company|sector&take=5
```

| Param | Required | Default | Meaning |
|---|---|---|---|
| `artId` | yes | | The article the reader just finished (the "anchor") |
| `mode` | no | `latest` | `latest`: next article overall. `company`: next from the anchor's primary issuer. `sector`: next in the anchor's sector type |
| `take` | no | `5` | How many to return, 1 to 20. We ask for several so the site can skip ones the reader has already seen |
| `companyId` | no | anchor's primary issuer | Override for `mode=company` |
| `sectorType` | no | anchor's primary sector type | Override for `mode=sector`. One of the 9 sector types (`Business`, `Financial`, `Technology`, …), the same values `/api/Articles/by-industry?industry=` accepts |

### Response, 200

```json
{
  "anchorId": 106708,
  "mode": "latest",
  "language": "ZH-TW",
  "companyId": null,
  "sectorType": null,
  "items": [
    { "articleId": 106697, "headline": "康希諾生物一季度業績穩健增長…", "publishDate": "2026-04-28T19:04:00+08:00", "language": "ZH-TW" },
    { "articleId": 106671, "headline": "由美國馬球協會…", "publishDate": "2026-04-28T19:00:00+08:00", "language": "ZH-TW" },
    { "articleId": 106687, "headline": "…", "publishDate": "2026-04-28T15:11:00+08:00", "language": "ZH-TW" }
  ],
  "exhausted": false
}
```

- `items` is in reading order: the first item is the next article.
- `exhausted: true` means there is nothing older in this mode. The site then falls back to `mode=latest`.
- Only `articleId` is strictly needed. `headline`, `publishDate` and `language` let us build the URL without a
  second call.
- **404** if `artId` does not exist or is unpublished. **400** for a bad `mode` or `take`.
- **Please send** `Cache-Control: public, max-age=300`. The answer is the same for every reader, so it caches well.

## The rules, and why

1. **"Next" means the next older article.** Readers scroll down a news feed, which goes back in time. That
   matches the current behaviour.
2. **Order by `publishDate DESC, articleId DESC`.** Many articles share a timestamp: 14 of the latest 100
   rows tie with another. Without the `articleId` tie-break, readers get skipped or repeated articles.
3. **Same language as the anchor.** Translations of one release go out together. After 106708 (ZH-TW) the
   next three rows are the ZH-CN, EN and JA versions of the same story, so without this filter a reader sees
   the same story three times.
4. **Published and live only.** `isPublished = 1` and `publishDate <= now`, so embargoed or scheduled
   releases never leak through "next".
5. **Company mode uses the primary issuer, not the first company listed.** On 106649 `companies[0]` is
   *JCN Newswire* (the distributor) and TANAKA is second. Following `companies[0]` would chain every JCN
   release ever. This is the same `primaryIssuerId` we asked for in `press-release.after.json`.
6. **Sector mode uses the sector type** (the 9 coarse groups), not the 76 industry names. Industry is too
   narrow and runs dry after a few articles.

## SQL (T-SQL)

> **Table and column names below are guesses.** The API does not expose its schema. Swap in the real
> names. The logic is what matters.

Assumed shape:

- `Articles (ArticleId, Headline, PublishDate, Language, IsPublished)`
- `ArticleCompanies (ArticleId, CompanyId, IsPrimaryIssuer)`: one row per issuer on an article
- `ArticleSectors (ArticleId, SectorId)` and `Sectors (SectorId, SectorName, SectorType)`

One query per mode keeps each execution plan simple. A single query with `OR`s across modes would plan
badly.

```sql
DECLARE @ArtId int = 106708, @Take int = 5;

-- 1. The anchor. No row means 404.
DECLARE @AnchorDate datetime2, @Lang nvarchar(10);
SELECT @AnchorDate = PublishDate, @Lang = Language
FROM   Articles
WHERE  ArticleId = @ArtId AND IsPublished = 1;

-- 2a. mode=latest
SELECT TOP (@Take) a.ArticleId, a.Headline, a.PublishDate, a.Language
FROM   Articles a
WHERE  a.IsPublished = 1
  AND  a.PublishDate <= SYSDATETIME()            -- see the time zone note below
  AND  a.Language = @Lang
  AND (a.PublishDate < @AnchorDate
       OR (a.PublishDate = @AnchorDate AND a.ArticleId < @ArtId))
ORDER BY a.PublishDate DESC, a.ArticleId DESC;

-- 2b. mode=company
DECLARE @CompanyId int = (
  SELECT TOP 1 CompanyId FROM ArticleCompanies
  WHERE  ArticleId = @ArtId
  ORDER BY IsPrimaryIssuer DESC, CompanyId);     -- replace the tie-break with however the primary is really marked

SELECT TOP (@Take) a.ArticleId, a.Headline, a.PublishDate, a.Language
FROM   Articles a
JOIN   ArticleCompanies ac ON ac.ArticleId = a.ArticleId AND ac.CompanyId = @CompanyId
WHERE  a.IsPublished = 1
  AND  a.PublishDate <= SYSDATETIME()
  AND  a.Language = @Lang
  AND (a.PublishDate < @AnchorDate
       OR (a.PublishDate = @AnchorDate AND a.ArticleId < @ArtId))
ORDER BY a.PublishDate DESC, a.ArticleId DESC;

-- 2c. mode=sector
DECLARE @SectorType nvarchar(50) = (
  SELECT TOP 1 s.SectorType FROM ArticleSectors x
  JOIN   Sectors s ON s.SectorId = x.SectorId
  WHERE  x.ArticleId = @ArtId
  ORDER BY x.SectorId);                          -- or however the primary sector is marked

SELECT TOP (@Take) a.ArticleId, a.Headline, a.PublishDate, a.Language
FROM   Articles a
WHERE  a.IsPublished = 1
  AND  a.PublishDate <= SYSDATETIME()
  AND  a.Language = @Lang
  AND (a.PublishDate < @AnchorDate
       OR (a.PublishDate = @AnchorDate AND a.ArticleId < @ArtId))
  AND  EXISTS (SELECT 1 FROM ArticleSectors x
               JOIN Sectors s ON s.SectorId = x.SectorId
               WHERE x.ArticleId = a.ArticleId AND s.SectorType = @SectorType)
ORDER BY a.PublishDate DESC, a.ArticleId DESC;
```

The `(PublishDate < @d OR (PublishDate = @d AND ArticleId < @id))` condition is keyset pagination. It
stays fast however deep the reader goes, unlike `OFFSET`.

### Indexes

```sql
CREATE INDEX IX_Articles_Next
  ON Articles (Language, IsPublished, PublishDate DESC, ArticleId DESC)
  INCLUDE (Headline);

CREATE INDEX IX_ArticleCompanies_Company ON ArticleCompanies (CompanyId, ArticleId);
CREATE INDEX IX_ArticleSectors_Article   ON ArticleSectors   (ArticleId, SectorId);
```

### Time zone

The wire sends `publishDate` with no offset (`2026-04-28T22:00:00`). If the column holds Singapore local
time, compare it against Singapore "now", not server time, or articles appear up to 8 hours early or late.
Please also return `publishDate` with its offset (`+08:00`), as already requested in `press-release.after.json`.

## Acceptance checks (real IDs)

| Call | Expect |
|---|---|
| `/api/Articles/106708/next` | First items `106697`, `106671`, `106687`, all ZH-TW. **Not** 106707, 106706 or 106701 (the translations) |
| `/api/Articles/106649/next?mode=company` | Only TANAKA (companyId 513) releases, older than 2026-04-28 20:00, in English. **No** JCN Newswire-only releases |
| `/api/Articles/106649/next?mode=sector` | English releases from 106649's sector type, older than 106649 |
| `/api/Articles/{oldest article}/next` | `items: []`, `exhausted: true` |
| `/api/Articles/999999999/next` | 404 |
| Any call with ties | Two articles with the same `publishDate` both appear, once each, higher `articleId` first |

## What the newsroom does with it

The site's server calls this endpoint and caches the result for 5 minutes. The browser receives the list
and loads the first article the reader hasn't already seen. Which `mode` applies (latest, company or sector)
depends on where the reader came from; that is decided on the newsroom side.
