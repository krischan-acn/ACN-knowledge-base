# ACN Newswire (ACN)

Knowledge base for ACN Newswire (Asia Corporate News Network), a press release distribution service covering Asia, publishing in English, Simplified and Traditional Chinese, Japanese and Korean.

**The short version:** ACN runs on a legacy ASP/IIS platform with no documentation or version history from the people who built it. We are replacing it from the inside out (database, then API, then CMS and public site), and right now the focus is the public-facing frontend. This repo exists so that knowledge stops living only in chats.

**Start with [AGENTS.md](AGENTS.md)**, then [project/status.md](project/status.md).

| Folder | What it is |
| --- | --- |
| [client/](client/) | Who ACN is: company, people, brand, competitors |
| [project/](project/) | Our work: status, scope, roadmap, open questions |
| [research/](research/) | Audits and research, dated |
| [deliverables/](deliverables/) | Reports and other files we hand to the client |
| [map/](map/) | Accounts, domains, tools, where logins are kept |
| [systems/](systems/) | How each system works, known problems, history |
| [decisions/](decisions/) | Why things are the way they are |
| [logs/](logs/) | What already happened |
| [templates/](templates/) | Log, decision and research templates |

## Starting a new session

- **Claude (claude.ai or the Claude app):** start the chat with: "We're working on the ACN knowledge base in `krischan-acn/ACN-knowledge-base`. Pull it and read AGENTS.md before anything else."
- **Claude Code on your computer:** clone the repo, open a terminal in it and run `claude`. `CLAUDE.md` loads automatically.
- **Other AI tools (Cursor, Codex and similar):** open the repo; they read `AGENTS.md`.
- **Chatbots without repo access:** paste `AGENTS.md` and `project/status.md` at the start.

Every session ends by committing and pushing its changes.

Private repository. No plain-text passwords or keys; see [AGENTS.md](AGENTS.md#credentials).
