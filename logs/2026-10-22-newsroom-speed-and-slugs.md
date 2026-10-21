# 2026-10-22 — Newsroom speed, article caching and slugs

**Date:** October 21–22, 2026 (Asia/Manila)
**Agent or person:** Claude Code, working with Krischan
**Approved by:** Krischan (code pushed to `staging` on their go-ahead)

## Asked

The newsroom homepage looked dead and loaded slowly, and clicking from the homepage into a press release was very slow. The goal is sub-300ms first paint for an audience that skews older and impatient. Also: review the CJK slug transliteration, check how infinite scroll behaves, and look at hosting on Cloudflare.

## Done

- **Diagnosed the "dead" homepage.** Every homepage section quietly falls back to empty when the API fails, so an API outage renders a blank page with a clean console, and that blank page then gets cached. The API (`development.acnnewswire.com`) was seen returning HTTP 522 (Cloudflare could not reach the origin) for about 20s per request on 2026-10-22. Not changed in code; see open items.
- **Homepage made lighter** (commit `71a1f51`):
  - The Events row was receiving all 816 events since 2007 and filtering in the browser. It now gets at most 20 upcoming events, filtered on the server.
  - Category rows no longer carry article summaries they never show.
  - Only the first hero image is preloaded; it was all 6.
  - The hero "Read more" opens in the same tab; it opened a new tab before.
  - Footer social icons lazy-load, removing 9 preloads from every page.
  - Homepage HTML went from 45KB to 16KB as sent over the network (brotli); image preloads from 15 to 2.
- **Article pages cached** (commit `9cb35af`):
  - `/article/...` was fully dynamic (`Cache-Control: private, no-store`), so every view re-rendered and re-called the API: 2.5–8s per click from the homepage, measured on production.
  - It is now ISR: rendered on first visit, served from cache after that, refreshed hourly in the background. Repeat views take 15–30ms (local production build).
  - The redirect from `/article/{id}` to the canonical URL is gone ([decision 0008](../decisions/0008-no-canonical-redirect.md)).
  - Only a real API 404 becomes "not found". Timeouts and 5xx errors no longer get cached as a missing article.
- **Slugs cleaned up** (commit `02d770f`, [decision 0007](../decisions/0007-transliterated-slugs.md)):
  - Slugs are cut at a whole word, contain only a–z and 0–9, and get a break between Latin and CJK text (`espn-lian-he`, `2026-nian`).
  - Japanese kana use standard Hepburn romanization: ジャパン → `japan`, ネットワーク → `nettowaku`, ちょっと → `chotto`. Before it was `ziyapan`, `netutowaku`, `tiyotuto`.
  - The transliteration library runs on the server only. Article pages ship about 70KB (gzipped) less JavaScript, and the server now gives infinite scroll its next URL.
  - Old links keep working, because the article route resolves by ID.
- **Wrote an API request** for a "next articles" endpoint: [research/2026-10-22-next-articles-endpoint.md](../research/2026-10-22-next-articles-endpoint.md).
- **Prepared a Cloudflare Workers setup** (OpenNext adapter, Next 16.2.6 → 16.4.2, R2 page cache). It was verified locally in Cloudflare's runtime, then **parked** because it needs a paid plan ([decision 0009](../decisions/0009-stay-on-netlify.md)).

## Changed

- GitHub `s-ging/newsroom` and `krischan-acn/newsroom`, branch `staging`: now at `02d770f`. Pushed via the `all` remote. The push also carried the earlier unpushed commit `b2ab478 new events page`, plus 11 older commits that `krischan-acn` had not received.
- Branch `cloudflare` (commit `7155a44`) exists **only on the local machine**. It is not pushed.
- Netlify, DNS, the API and the database: not touched.
- This repo: [systems/newsroom.md](../systems/newsroom.md), [project/status.md](../project/status.md), decisions 0007–0009, and the research note above.

## Open items

- **Infinite scroll probably never loads a second article in production.** The browser calls the API directly, and the API sends no CORS headers. It also only works within the latest 20 articles, switches the URL to the next article too early, and never updates the tab title. Fix: a server-side "next article" route, ideally backed by the endpoint in the [API request](../research/2026-10-22-next-articles-endpoint.md). Owner: the API team for the endpoint, the newsroom side for the route.
- **API outages blank the homepage, and the blank version gets cached for up to an hour.** Make the homepage keep its last good copy when every call fails. Not started.
- **Kanji in Japanese slugs still get Mandarin readings** (三菱重工 → `san-ling-chong-gong`). A proper fix needs a Japanese reading dictionary (kuroshiro/kuromoji).
- **The canonical URL defaults to `www.acnnewswire.com`, where article paths 404.** Set `NEXT_PUBLIC_SITE_URL` on the host, or accept it until the newsroom moves to `www`.
- **Missing articles return status 200 (soft 404) on Netlify.** The page itself shows not-found with `noindex`.
- **Confirm which branch Netlify deploys from,** and whether `staging` is live.
- **The first visit to each article still waits on the API** (0.5–4s). Options: pre-build the latest articles at deploy time, or refresh them when someone publishes.

---

Authored by Krischan.
