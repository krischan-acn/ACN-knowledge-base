# 2026-10-23 — Newsroom: codebase review and SvelteKit evaluation

**Date:** October 23, 2026 (UTC+8)
**Agent or person:** Claude Code, working with Krischan
**Approved by:** Krischan (new repo, its pushes, and pushing newsroom `main`)

## Asked

Review the newsroom codebase's health. Try a SvelteKit rewrite of the components, home page and a press release page, with the A/B tool dropped and speed fixes applied. Then compare it online against the live Next sites.

## Done

- **Reviewed the newsroom codebase.**
  - Security findings: plaintext credentials still in `log.md` in git history, unescaped JSON-LD, no security headers, and a regex-only first-paint sanitiser.
  - Speed findings: Inter loaded twice (one copy render-blocking), the A/B overlay calling the server on every page view, and list rows fetching whole press releases just to find a logo.
- **Built a partial SvelteKit 3 version** in a new private repo, `krischan-acn/newsroom-svelte`, deployed to Cloudflare Workers. It adds edge caching, a CSP and security headers, server-side sanitising, and fixed infinite scroll.
- **Pushed the newsroom's two docs commits** (AGENTS.md, README, BIBLE) from `staging` to `main` on both GitHub repos. This redeployed both Next hosts.
- **Benchmarked and ran Lighthouse on all three deployments,** then parked the rewrite ([decision 0010](../decisions/0010-sveltekit-rewrite-parked.md)).

## Changed

- New repo `krischan-acn/newsroom-svelte` (commits `a933af4`, `a026537`) and its Cloudflare Worker.
- Newsroom `main` and `staging` at `6ccfbab` on `s-ging/newsroom` and `krischan-acn/newsroom` (docs only). Both hosts redeployed.
- This repo: decision 0010, this log, `project/status.md`.

## Open items

- Decide the newsroom stack going forward (Krischan).
- Apply the review's fixes to the live Next site: the font loaded twice, security headers, JSON-LD escaping, and the per-row logo fetches.
- Rotate the credentials in the newsroom's `log.md` and purge it from git history. It is still tracked on `main`.
- Both newsroom repos are on GitHub; whether either is public is unconfirmed.
- Leftover `svelte` branch in the local newsroom checkout. It is empty (same as `main`) and can be deleted.

---

Authored by Krischan, with Claude Code.
