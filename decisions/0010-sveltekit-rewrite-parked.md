# 0010 — SvelteKit rewrite of the newsroom evaluated and parked

**Date:** 2026-10-23
**Decided by:** Krischan
**Status:** parked (revisit when choosing the newsroom's stack going forward)

## Context

The newsroom (Next.js, on Netlify and Vercel) felt slow. A rewrite in SvelteKit looked like a way to get there faster, with better security and an easier codebase. In one day we built a partial SvelteKit version:
- the site shell, the home page and the press release page, including infinite scroll
- deployed to Cloudflare Workers
- benchmarked against both live Next sites

This is a different setup from the one [decision 0009](0009-stay-on-netlify.md) parked. That was the Next app on Workers, which needed OpenNext plus R2 storage and a queue. SvelteKit runs on Workers as one Worker with no storage.

## Decision

Park the SvelteKit rewrite. Keep the newsroom on Next.js for now, and decide the stack going forward separately. The code stays in `krischan-acn/newsroom-svelte` (private), with its test Worker still deployed.

## Why

- **SvelteKit was better on scripts, but not faster for the reader yet.** On a simulated phone (Lighthouse, after fresh deploys of everything):
  - It shipped about a third of the JavaScript and blocked the page 2–3× less.
  - But it painted later: 2.6–3.7 s against 1.9–2.3 s for Next.
  - And it answered slower on fresh connections: 350–920 ms against about 40 ms for Vercel's cache.
  - The causes are known and fixable, but not fixed. They're in the write-up.
- **Redeploying the Next site fixed most of what felt slow.** For example, the Netlify home page's layout shift went from severe (0.886) to 0.001 once it was rebuilt from current `main`.
- **Both versions share the biggest remaining cost:** about 3.4 s of style and layout work on phones, which comes from the page design itself. Changing framework does not remove it.

What we gave up for now: SvelteKit's smaller pages and its security improvements. Those security fixes can be applied to the Next site instead (see the write-up).

## Alternatives considered

- **Finish the three SvelteKit fixes first, then compare again.** Not now. The Next site is acceptable after the redeploy, and the stack decision should come first.
- **Port the rest of the site straight away.** Too early while the numbers did not show a clear win.

Full results, causes, open fixes and how to resume: `docs/evaluation-2026-10-23.md` in `krischan-acn/newsroom-svelte`.
