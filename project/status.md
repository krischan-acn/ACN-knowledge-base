# Status

Last updated: 2026-10-22.

Newest items at the top of each list. Tick items off (`[x]`) and add the date; never delete them.

Honest note: where things stand on several workstreams is **hard to tell** (Kris, 2026-10-22). Items marked "to confirm" need checking, not guessing.

## Timeline

Rough estimate; no dates agreed yet.

```mermaid
gantt
    title ACN frontend phase (estimate)
    dateFormat YYYY-MM-DD
    axisFormat %b %d
    section Baseline
    Knowledge base set up                   :done,    d1, 2026-10-22, 1d
    Homepage audit (PageSpeed, WAVE)        :active,  d2, 2026-10-22, 3d
    section Frontend
    Confirm newsroom state and blockers     :         f1, after d2, 3d
    Clear pre-ship blockers                 :         f2, after f1, 14d
    Launch newsroom                         :         f3, after f2, 1d
```

## Checklist

### To do

- [ ] Fix newsroom infinite scroll: move the next-article lookup server-side, stop the early URL switch, update the tab title ([research](../research/2026-10-22-next-articles-endpoint.md))
- [ ] Send the "next articles" endpoint spec to the API team ([spec](../research/files/2026-10-22-next-articles-endpoint-spec.md))
- [ ] Stop an API outage from caching a blank newsroom homepage ([systems/newsroom.md](../systems/newsroom.md#hosting-and-caching-2026-10-22))
- [ ] Decide the newsroom's canonical host (`www` or `portal`) and set `NEXT_PUBLIC_SITE_URL` ([systems/newsroom.md](../systems/newsroom.md#known-open-items))
- [ ] Confirm which branch Netlify deploys the newsroom from, and whether `staging` is live
- [ ] Change the passwords pasted into chat on 2026-10-22, one per account, 2FA on ([map/accounts.md](../map/accounts.md))
- [ ] Make this repo private again
- [ ] Pick a password manager ([decision 0002](../decisions/0002-credentials.md))
- [ ] Run PageSpeed Insights and WAVE on www.acnnewswire.com, add screenshots to [research](../research/2026-10-22-homepage-accessibility-structure.md)
- [ ] Confirm newsroom state: which repo is main, what is deployed where, the pre-ship blocker list ([systems/newsroom.md](../systems/newsroom.md))
- [ ] Decide on cheap accessibility fixes to the live site ([research](../research/2026-10-22-homepage-accessibility-structure.md#what-this-means-for-the-project))
- [ ] To confirm: portal status after the Sep 14 deadline ([systems/portal.md](../systems/portal.md))
- [ ] Benchmark Business Wire and the other competitors the same way ([client/competitors.md](../client/competitors.md))

### Done

- [x] 2026-10-22 Newsroom: hosting reviewed; stays on Netlify, Cloudflare Workers setup parked ([decision 0009](../decisions/0009-stay-on-netlify.md))
- [x] 2026-10-22 Newsroom slugs cleaned up: whole-word cuts, Hepburn kana, transliteration server-only ([decision 0007](../decisions/0007-transliterated-slugs.md))
- [x] 2026-10-22 Newsroom article pages cached: repeat views 15–30ms, down from 2.5–8s ([decision 0008](../decisions/0008-no-canonical-redirect.md))
- [x] 2026-10-22 Newsroom homepage made lighter: 45KB → 16KB over the network, 15 → 2 image preloads ([log](../logs/2026-10-22-newsroom-speed-and-slugs.md))
- [x] 2026-10-22 Homepage accessibility and structure scan ([research](../research/2026-10-22-homepage-accessibility-structure.md))
- [x] 2026-10-22 Knowledge base set up from the client template ([log](../logs/2026-10-22-knowledge-base-set-up.md))
- [x] Sep 2026 Homepage slowness and app pool outages fixed ([systems/legacy-site.md](../systems/legacy-site.md#history))
- [x] Aug 2026 Newsroom moved to the new API (`development.acnnewswire.com`) ([systems/newsroom.md](../systems/newsroom.md))
- [x] Aug 2026 Legacy database exported, cleaned and analysed ([systems/database.md](../systems/database.md))
