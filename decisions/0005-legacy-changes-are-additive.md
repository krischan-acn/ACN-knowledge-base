# 0005 — Changes to the legacy platform are additive only

**Date:** agreed Sep 2026, written down 2026-10-22
**Decided by:** Kris
**Status:** accepted

## Context

The live site and its database came with no documentation, no version control and no enforced foreign keys. Anything we remove may be used by something we cannot see.

## Decision

- Put the live server's code under git before changing it.
- Extend the legacy API by adding to it, not by rewriting it.
- Add database columns as nullable, with a backup first. Do not drop or rename.
- Use Mariel's new API as a reference, not as a live replacement, until it is ready.

## Why

Chesterton's fence ([PRINCIPLES.md](../PRINCIPLES.md#chestertons-fence)): on an undocumented system, the safe move is to add and not to clean up. It costs some clutter now, in exchange for no surprise outages.

## Alternatives considered

- Rewrite and switch over in one go: too risky with no documentation and live traffic.
