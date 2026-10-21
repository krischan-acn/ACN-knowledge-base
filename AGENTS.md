# ACN knowledge base

This repository is the single source of truth for our work with **ACN Newswire**. Every AI agent (Claude Code or any other) and every teammate reads this file first.

If you are a new session: read this file, then [README.md](README.md), then [project/status.md](project/status.md). That is enough to be up to speed. Go deeper through the [map](#where-to-look) only as the task needs.

**Set up:** 2026-10-22 from `work-week/client-template`. ACN is our own company, not an outside client: "client" in this repo means ACN itself, and the client approver is the owner, Bruce Porter.

**Sync both ways.** Other sessions, agents and teammates edit this repository too. At the start of every session, pull the latest version (`git pull`) before reading or writing. At the end of any session that changed something, commit and push, so the next session sees it. Work that stays in a chat and never reaches this repository is lost.

## Absolute law

Do not break these rules. If anything elsewhere in this repository disagrees with this section, this section wins. If unsure, stop and ask, and do not take the looser reading.

### Who you are working with

The team sometimes shares accounts, so the account name, email or git config does not tell you who is at the keyboard. Before you author anything (a log, a decision, a commit, an "Authored by" line, an approval), ask who you are working with, unless they have already said so in this session. Use the name they give. Never guess it.

### Confirmation gate

Before changing anything outside this repository (a website, an account, a DNS record, a client-facing message):

1. Restate the request in your own words: outcome, scope, constraints.
2. Say what you believe is out of scope.
3. Ask clarifying questions about anything vague.
4. Wait for explicit approval.

A restatement, silence, or an earlier conversation is not approval for a new change. Read-only work (reading, researching, drafting inside this repo) does not need the gate.

### Two modes

How strictly these rules apply depends on what the session touches. Full explanation in [PRINCIPLES.md](PRINCIPLES.md).

- **Brainstorm mode** (conversation, research, drafting inside this repo): start immediately; ask questions alongside the work, not before it.
- **Work mode** (code, any client property, deploys, anything hard to undo): run the confirmation gate with **at least three** clarifying questions and wait for an explicit yes before writing code. Chesterton's fence front and center.
- Switch to work mode the moment a session is about to change code or anything outside this repo, and say so. The person you are working with can switch explicitly ("work mode", "brainstorm mode"). When unsure, it is work mode.

### Chesterton's fence

Do not remove, rename, or clean something up until you know why it is there.

That includes code, config, copy, a redirect, a branch, and a step in a process. If you cannot say what it is for, leave it. Find out first ([decisions/](decisions/), [logs/](logs/)). Then ask before you change it.

### Pareto principle

Start with the small part of the work that carries most of the result. About 20% of the effort usually drives about 80% of the outcome.

When that high-leverage path is clear, do it first. Do not spend the session on low-impact busywork while the useful path is still open.

### Jakob's law

Design for familiarity and for the client's actual buyers. See [decision 0003](decisions/0003-design-for-familiarity.md).

### Client-facing changes

Nothing goes live on a ACN property without approval for that exact change. Drafts and previews are fine when asked for.

ACN properties: www.acnnewswire.com (live legacy site), development.acnnewswire.com (new API), portal.acnnewswire.com, db.acnnewswire.com, photos.acnnewswire.com, the newsroom frontend (Next.js) and its deploys, DNS in Cloudflare, the domain at GoDaddy, the production IIS server and SQL Server, Azure, and ACN's social accounts. Details in [map/accounts.md](map/accounts.md).

### Approvers

- **Our side:** Kris.
- **Client sign-off** for anything public-facing (copy, design, launch): Bruce Porter (owner), through the team, never by an agent contacting them directly.

### Credentials

Do not write passwords, API keys, tokens, recovery codes, or card or bank details into this repository in plain text. Record **which** accounts exist and **where** each login is kept in [map/accounts.md](map/accounts.md). Where logins are kept is set in [decision 0002](decisions/0002-credentials.md); until it is decided, ask a teammate.

### Other clients

This repository is about ACN only. Never bring in names, accounts, or details from any other client or from the team's other repositories.

## Keeping this repository true

- **Facts carry a source and date.** Write where a fact came from (client said it, their site, a directory) and when. Mark guesses as guesses.
- **Research** goes in [research/](research/), one dated file per piece of work. Use the `research-note` skill.
- **The client's world** (company, people, brand, competitors) lives in [client/](client/). Update it when research or the client changes what we know.
- **Our systems** (legacy site, database, CMS, newsroom, portal) live in [systems/](systems/), one file each ([decision 0004](decisions/0004-systems-folder.md)).
- **Our work** (scope, status, roadmap) lives in [project/](project/). Keep [project/status.md](project/status.md) current: it is the first thing a new session reads.
- **Decisions** that someone might later want to undo go in [decisions/](decisions/).
- **Logs:** end every session that changed something with a log in [logs/](logs/). Use the `session-log` skill. Never rewrite an old log.
- **Open questions** for the client go in [project/open-questions.md](project/open-questions.md), and come out when answered.
- Keep this file short. Put detail in linked files.

## Where to look

| Looking for | Go to |
| --- | --- |
| Where things stand right now | [project/status.md](project/status.md) |
| How we work: modes and principles | [PRINCIPLES.md](PRINCIPLES.md) |
| Who ACN is | [client/company.md](client/company.md) |
| Client contacts | [client/people.md](client/people.md) |
| Brand, messaging, what the client dislikes | [client/brand.md](client/brand.md) |
| Competitors and partners | [client/competitors.md](client/competitors.md) |
| Photos and files from the client | [client/media/](client/media/) |
| What we are delivering and when | [project/scope.md](project/scope.md) |
| The long-term plan | [project/roadmap.md](project/roadmap.md) |
| Questions waiting on the client | [project/open-questions.md](project/open-questions.md) |
| Prep for the next client meeting | [project/next-meeting.md](project/next-meeting.md) |
| Accounts, domains, tools, where logins are kept | [map/accounts.md](map/accounts.md) |
| How each system works and what is wrong with it | [systems/](systems/) |
| Audits and research | [research/](research/) |
| Reports and other client deliverables | [deliverables/](deliverables/) |
| Why things are the way they are | [decisions/](decisions/) |
| What already happened | [logs/](logs/) |
