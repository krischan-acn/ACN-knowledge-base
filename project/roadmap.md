# Roadmap and vision

Internal. Last updated: 2026-10-22. Each phase is earned by delivering the one before it.

## The opportunity

Replace an undocumented legacy platform with one ACN owns and understands, without outages along the way, and give it a public face that competes with Business Wire and the Asian wires.

## Phase 1: frontend (now)

See [scope.md](scope.md).

**Proves:** the new API can carry the public site, and the new design works for readers.

## Later phases

- New database schema and data migration (Mariel) ([systems/database.md](../systems/database.md)).
- Editorial CMS: write once, export to NewsML-G2, NITF, email, web ([systems/cms.md](../systems/cms.md)).
- Customer portal ([systems/portal.md](../systems/portal.md)).
- Retire the legacy ASP site once nothing depends on it.

## Watch-outs

- Old URLs on the legacy site carry links and search rankings: redirect, don't delete.
- Five languages, including Chinese/Japanese/Korean URL slugs.
