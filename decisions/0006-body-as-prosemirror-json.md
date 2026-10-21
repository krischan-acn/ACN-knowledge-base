# 0006 — Press release body stored as ProseMirror JSON

**Date:** Aug 2026, written down 2026-10-22
**Decided by:** Kris
**Status:** accepted (early schema decision; confirm with Mariel when the schema is final)

## Context

The legacy `Article.Body` is free-form NTEXT HTML. A release has to be exported to NewsML-G2, NITF, email and web, and edited in Tiptap.

## Decision

Store the body as ProseMirror JSON, the format Tiptap edits natively. Keep `body_html` as a derived copy for display and speed, rebuilt from the JSON.

## Why

One clean structured source makes every export a conversion from known structure, not from messy HTML. Gives up: old releases must be converted, and `body_html` must always be regenerated, never edited directly.

## Alternatives considered

- HTML as the source: every export has to parse whatever the HTML happens to be.
