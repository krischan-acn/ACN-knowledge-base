# Database

Last updated: 2026-10-22.

## Legacy database

- SQL Server, instance `SqlServerStd17`, 100+ tables (Kris, Aug 2026).
- About 80 tables are dead. Much of the apparent complexity is copies of the same table per locale (for example `Company_CH`, `Company_CT`, `Company_JP`, `Company_KO`) and duplicated vendor lookup tables (Kris, Aug 2026).
- No enforced foreign keys, which is a data-integrity problem (Kris, Aug 2026).
- Press releases: `Article` (about 109,000 rows, body as NTEXT), `Article_Body`, and `Comp_Art` linking releases to companies (Kris, Aug 2026).
- Read it with `READ UNCOMMITTED` for exports so live traffic is not blocked (Kris, Aug 2026).

## Export and cleanup work done

- PowerShell bulk export to 177 pipe-delimited CSVs; a Python clean-and-merge script; "starter pack" scripts that join press releases and enrich companies (sectors, tickers, contacts, activity); an Excel converter for sharing through OneDrive (Kris, Aug 2026). The scripts live on the work VM, not in a repo yet.

## New schema (Mariel designs and builds it)

Proposed by Kris, Aug 2026, not final:

- `companies` and `press_releases` normalized, with `company_translations`, `company_social_links` (junction table), portal credentials split into their own table, and `activity_tier` as a computed view.
- `translation_group_id` to link language versions of a release: flagged as high value.
- Body as ProseMirror JSON with derived `body_html` ([decision 0006](../decisions/0006-body-as-prosemirror-json.md)).

## Ideas explored, not decided

- A parallel dev database (`ACNNewswire_Dev`) restored from backup on the same server.
- A Node.js/Express API over SQL Server (`mssql`, connection pooling, parameterized queries).

## Open

- Where the export scripts should live (a repo?) and whether they are still needed.
