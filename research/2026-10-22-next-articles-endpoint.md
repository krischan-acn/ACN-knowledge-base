# 2026-10-22 — "Next articles" endpoint for infinite scroll

**Method:**
- Read the newsroom's infinite-scroll code (`InfiniteArticleFeed.tsx`, `services/reading-mode.ts`).
- Probed `development.acnnewswire.com`, including its swagger, with curl on 2026-10-22.
- Checked the latest 100 rows from `/api/Articles`.
- Done in a Claude Code session with Krischan.

## Findings

- **No endpoint answers "what comes after article X".** The site fakes it by fetching the latest 20 articles and finding the current one in that list. Open any older article and the feed ends immediately. (Code, 2026-10-22.)
- **The API sends no `Access-Control-Allow-Origin` header.** The browser-side call is likely blocked, so infinite scroll probably never loads a second article in production. (curl with an `Origin` header, 2026-10-22.)
- **Publish times tie often:** 14 of the latest 100 articles share a publish time with another, so ordering needs `articleId` as a tie-breaker. (API data, 2026-10-22.)
- **Translations of one release are published together.** After 106708 (ZH-TW), the next three rows are its ZH-CN, EN and JA versions. Without a same-language filter, readers would see one story three times. (API data, 2026-10-22.)
- **`companies[0]` is not always the issuer.** On 106649 it is JCN Newswire (the distributor), and TANAKA comes second. A "same company" feed needs the primary issuer. (API data, 2026-10-22.)
- **Company and sector reading modes are stubs.** All three fall back to chronological. (Code, 2026-10-22.)
- The database schema is not documented. Table names in the SQL are **guesses**.

## What this means for the project

- We need a request to the API team: [the full spec](files/2026-10-22-next-articles-endpoint-spec.md). It covers:
  - endpoint shape: `GET /api/Articles/{artId}/next?mode=latest|company|sector&take=5`
  - the ordering and filtering rules, each with the reason for it
  - T-SQL (SQL Server) for each mode, plus indexes
  - a time zone note
  - acceptance checks using real article IDs
- Newsroom side, once the endpoint exists: a server route that calls it and caches the answer for 5 minutes. The browser then picks the first article the reader hasn't seen yet.

## Next steps

- Send the spec to the API team (Mariel). Owner: Krischan.
- Decide which reading mode applies where (for example: company mode when arriving from a company page). This is a product decision.
- Newsroom fixes that don't need the endpoint:
  - stop the early URL switch
  - update the tab title as the reader scrolls
  - move the next-article lookup server-side
