# 0008 — Article pages are cached and not redirected to the canonical URL

**Date:** 2026-10-22
**Decided by:** Krischan, on Claude Code's recommendation
**Status:** accepted

## Context

Homepage and list links point to the short form `/article/{id}`. The article route used to look up the release and then redirect (307) to `/article/{language}/{id}/{slug}`. The route was also fully dynamic, so nothing was cached. A click from the homepage cost two server renders and several API calls: 2.5–8s, measured on production on 2026-10-21.

Making the route cacheable (ISR) broke the redirect. On a cached route, Next.js cannot send a real 307. It stores a 200 page with a 1-second `<meta refresh>` instead.

## Decision

- The article route is ISR: each article is rendered on its first visit, served from cache after that, and refreshed hourly in the background.
- No redirect. The article renders at whatever URL it was requested under. The `<link rel="canonical">` tag tells search engines the real URL, and the page's infinite-scroll code rewrites the address bar to it as the reader scrolls.

## Why

Speed for an impatient audience. Repeat article views dropped to 15–30ms (local production build), and a click no longer pays for a redirect round trip. Pointing search engines at the real URL with a canonical tag is a standard, accepted way to handle duplicate URLs.

What we gave up: the address bar shows the short URL until the reader scrolls. A nonexistent article now returns status 200 rather than 404 on Netlify, although the page still shows not-found and carries `noindex`.

## Alternatives considered

- Keep the route dynamic with the redirect: correct status codes, but every view stays slow.
- Link to the full canonical URL everywhere: the by-industry feed, which supplies most homepage links, carries no language, so the URL cannot be built reliably.
- Redirect in middleware: it would need an uncached API call on every short-URL request.
