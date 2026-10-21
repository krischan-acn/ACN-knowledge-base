# 0009 — Newsroom stays on Netlify; Cloudflare Workers parked

**Date:** 2026-10-22
**Decided by:** Krischan
**Status:** accepted (revisit if a hosting budget appears)

## Context

We looked at moving the newsroom's CI/CD and hosting to Cloudflare, where the DNS already lives.

- **Cloudflare Pages** can only host a static export. The newsroom needs a server for its API routes, OG images, ISR caching and the A/B overlay, so Pages cannot run it.
- **Cloudflare Workers** (via OpenNext or vinext) can run it. In practice that means:
  - the $5/month Workers Paid plan, because rendering a page uses more than the free plan's 10ms of CPU per request
  - somewhere to store cached pages (R2, which asks for a card on file, or KV)

## Decision

Stay on Netlify's free plan, which allows commercial use and handles ISR caching with no setup. The Cloudflare work is kept on a local `cloudflare` branch (commit `7155a44`, not pushed). That branch has:

- the OpenNext adapter
- Next 16.4.2
- an R2 page cache and a Durable Object queue

It was verified locally in Cloudflare's runtime: every route worked, and the bundle is 2.17MB gzipped.

## Why

No budget for hosting right now, and the site needs to just work. Netlify already serves it, and today's speed fixes need nothing extra there.

What we gave up: Cloudflare's network, and being on one platform with the DNS. Netlify edge-cache misses still cost about 800ms before the first byte.

## Alternatives considered

- Cloudflare Workers free plan with a KV cache: no card needed, but first renders risk the 10ms CPU limit (error 1102).
- Vercel Hobby: free, but its terms do not allow commercial sites.
