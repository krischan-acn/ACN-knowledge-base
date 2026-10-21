# Newsroom frontend

Last updated: 2026-10-22. **Current focus** (Kris, 2026-10-22).

## What it is

- The new public-facing newsroom, a Next.js project (Kris, Aug 2026).
- Code mirrored in GitHub `krischan-acn/newsroom` and `s-ging/newsroom`. Deployed to Netlify/Vercel; Azure is the planned home (Kris, Aug 2026).
- Reads from the new API at `development.acnnewswire.com`, migrated from the old production API, with one shared `mapArticle()` helper that shapes article data (Kris, Aug 2026).

## Pages and features built

- Press release page: share buttons, "More from Company", infinite scroll (`IntersectionObserver`), a reading-mode stub, and a fix for two-segment URL parsing.
- Search with nested checkbox filters in the sidebar (`NestedCheckbox`).
- Newsroom, Company profile, Events pages.

(Kris, Aug 2026.)

## Known open items

- Pre-ship checklist exists, split into blockers, important and deferred (Kris, Aug 2026). The list itself is not in this repo yet.
- Deferred: URL slugs for Chinese/Japanese/Korean titles. The converter (kuroshiro/kuromoji) is slow to start cold, so this waits for the Azure deployment (Kris, Aug 2026).
- Which repo is the main one, and what is live where today: **unclear, to confirm**.

## Design direction

See [client/brand.md](../client/brand.md#design-direction-for-new-work). The live site's problems ([research](../research/2026-10-22-homepage-accessibility-structure.md)) are a checklist of what the newsroom must not repeat.
