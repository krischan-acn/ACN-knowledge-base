# 0004 — A systems/ folder for how each system works

**Date:** 2026-10-22
**Decided by:** Kris
**Status:** accepted

## Context

The template is built for an agency working with an outside client: brand, competitors, buyers. Most of what we know about ACN is engineering: how the legacy site, database, CMS, newsroom and portal work, and what is broken in each. None of the template's folders fits that.

## Decision

Add `systems/`, one Markdown file per system, plus a README index with a one-line status each. Dated investigations still go in `research/`; choices still go in `decisions/`.

## Why

A new session can open one file and know how a system works, without reading every log. Keeps `client/` about the business and `project/` about the plan.

## Alternatives considered

- Put system notes in `research/`: those are dated snapshots, not a current picture.
- Put them in `map/`: that is for accounts and where logins live, and would get too long.
