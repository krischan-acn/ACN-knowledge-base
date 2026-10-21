# Newsroom frontend

Last updated: 2026-10-22 (speed, caching and slugs: [log](../logs/2026-10-22-newsroom-speed-and-slugs.md)). **Current focus** (Kris, 2026-10-22).

## What it is

- The new public-facing newsroom, a Next.js project (Kris, Aug 2026).
- Code mirrored in GitHub `krischan-acn/newsroom` and `s-ging/newsroom`. Deployed to Netlify/Vercel; Azure is the planned home (Kris, Aug 2026).
- Reads from the new API at `development.acnnewswire.com`, migrated from the old production API, with one shared `mapArticle()` helper that shapes article data (Kris, Aug 2026).

## Pages and features built

- Press release page: share buttons, "More from Company", infinite scroll (`IntersectionObserver`), a reading-mode stub, and a fix for two-segment URL parsing. Infinite scroll has known problems; see open items.
- Search with nested checkbox filters in the sidebar (`NestedCheckbox`).
- Newsroom, Company profile, Events pages.

(Kris, Aug 2026.)

## Hosting and caching (2026-10-22)

- **Where it runs.** `portal.acnnewswire.com` is served by Netlify: a DNS-only CNAME to `acnnewswire.netlify.app` in Cloudflare (checked 2026-10-21). It stays on Netlify for now ([decision 0009](../decisions/0009-stay-on-netlify.md)).
- **Repos and branches.** The local repo has an `all` git remote that pushes to both `s-ging/newsroom` and `krischan-acn/newsroom`. `staging` is at `02d770f` on both (2026-10-22). Which branch Netlify builds from: **to confirm**.
- **Homepage caching.** The homepage is cached (ISR, refreshed hourly).
  - Every section falls back to empty when the API fails, so an API outage renders a blank homepage, and the blank version can stay cached for up to an hour.
  - Netlify edge-cache hits answer in about 190–400ms; misses in about 800–1000ms (curl from the Philippines, 2026-10-21).
- **Article caching.** Article pages are cached (ISR) and not redirected to the canonical URL ([decision 0008](../decisions/0008-no-canonical-redirect.md)). Repeat views take 15–30ms; a first view waits on the API (0.5–4s).
- **Slugs.** Transliterated Latin slugs, cleaned up 2026-10-22 ([decision 0007](../decisions/0007-transliterated-slugs.md)):
  - Kana use Hepburn romanization (`src/lib/romanize-kana.ts`).
  - Kanji still get Mandarin readings; the kuroshiro fix below stays deferred.
  - The slug code is `server-only`. The browser receives finished URLs from `/api/press-release/{id}` (`urlPath`).
- **Cloudflare Workers.** A working setup is parked on a local, unpushed `cloudflare` branch (OpenNext, Next 16.4.2, R2 cache). Details in [decision 0009](../decisions/0009-stay-on-netlify.md).

## Known open items

- **Infinite scroll probably never loads a second article in production** (found 2026-10-22). The API sends no CORS headers, and the next-article lookup runs in the browser. It also only works within the latest 20 articles, switches the URL too early, and never updates the tab title. Fix and API request: [research](../research/2026-10-22-next-articles-endpoint.md).
- **The canonical URL defaults to `https://www.acnnewswire.com`, where article paths return 404** (checked 2026-10-22). Set `NEXT_PUBLIC_SITE_URL` if `portal` should be the canonical host before launch.
- **Missing articles return status 200 with `noindex`** (a soft 404) since the ISR change.
- **`npm run lint` is broken** (an ajv override vs eslint 10). `tsc --noEmit` and `npm run build` are the real checks (repo CLAUDE.md).
- Pre-ship checklist exists, split into blockers, important and deferred (Kris, Aug 2026). The list itself is not in this repo yet.
- Deferred: proper kanji readings in Japanese slugs. The converter (kuroshiro/kuromoji) is slow to start cold, so this waits for the Azure deployment (Kris, Aug 2026). Kana and general slug cleanup were done 2026-10-22.
- Which repo is the main one, and what is live where today: **unclear, to confirm**.

## Design direction

See [client/brand.md](../client/brand.md#design-direction-for-new-work). The live site's problems ([research](../research/2026-10-22-homepage-accessibility-structure.md)) are a checklist of what the newsroom must not repeat.
