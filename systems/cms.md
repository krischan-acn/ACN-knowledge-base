# Editorial CMS

Last updated: 2026-10-22.

## What it is

- A rebuild of ACN's internal editorial system, from database schema through microservices to the UI, built from the inside out (Kris, Aug 2026).
- Core job: write a press release once, export to NewsML-G2, NITF, email (MJML) and web HTML (Kris, Aug 2026).
- Stack: React with custom CSS (no component libraries), Zustand for state, Tiptap (built on ProseMirror) for the editor (Kris, Aug 2026).

## Early schema choices

- Release body stored as ProseMirror JSON; `body_html` is a derived copy ([decision 0006](../decisions/0006-body-as-prosemirror-json.md)).
- Boilerplate is inherited from the company record.
- `translation_group_id` (a UUID) links language versions of the same release.

(Kris, Aug 2026.)

## Design

"Dated but confident": see [client/brand.md](../client/brand.md#design-direction-for-new-work).

## Open

- Bruce's involvement in the CMS: not yet written down.
- Build status: unclear, to confirm.
