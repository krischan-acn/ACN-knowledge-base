# Accounts, domains and tools

What exists and where each login is kept. **Never the password itself.** See [AGENTS.md](../AGENTS.md#credentials).

Last updated: 2026-10-22.

> **Action needed (2026-10-22):** logins for Cloudflare, GoDaddy, GitHub and db.acnnewswire.com were pasted into a Claude chat on this date. Treat them as exposed: change each password, use a different password per account, and turn on 2FA. Then fill in "Login kept in" below once [decision 0002](../decisions/0002-credentials.md) picks a password manager.

## ACN properties

| Property | Detail | Owner | Login kept in |
| --- | --- | --- | --- |
| www.acnnewswire.com | Live legacy site: classic ASP on IIS, behind Cloudflare. See [systems/legacy-site.md](../systems/legacy-site.md) | ACN | n/a (server access below) |
| development.acnnewswire.com | New API, used by the newsroom frontend | ACN (Mariel) | not yet recorded |
| portal.acnnewswire.com | Customer portal | ACN | not yet recorded |
| db.acnnewswire.com | Database web login; two accounts exist (a shared one and Kris's own) | ACN | **not in a vault yet: rotate** |
| photos.acnnewswire.com | Image host used on the homepage | ACN | not yet recorded |
| Domain acnnewswire.com | Registrar: GoDaddy | ACN, under Kris's login | **not in a vault yet: rotate** |
| DNS and proxy | Cloudflare (the live site is served through it) | ACN, under Kris's account | **not in a vault yet: rotate** |
| Production web server | IIS, app pool for www.acnnewswire.com | ACN | not yet recorded |
| SQL Server | Instance `SqlServerStd17`, legacy ACN database | ACN | not yet recorded |
| Azure | Hosting and infrastructure | ACN | not yet recorded |
| Newsroom deploys | Netlify / Vercel | ACN | not yet recorded |
| Social accounts | X/Twitter `@acnnewswire` (linked from homepage); others not listed yet | ACN | not yet recorded |

## Our tools for this project

| Tool | Used for | Owner | Login kept in |
| --- | --- | --- | --- |
| GitHub `krischan-acn/ACN-knowledge-base` | This knowledge base | Kris | **not in a vault yet: rotate** |
| GitHub `krischan-acn/newsroom`, `s-ging/newsroom` | Newsroom frontend (mirrored) | Kris | GitHub accounts |
| Claude (claude.ai) and Claude Code | Research, maintaining this repo | Kris | |
