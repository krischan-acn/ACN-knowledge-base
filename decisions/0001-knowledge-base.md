# 0001 — A Git knowledge base as the single source of truth

**Date:** 2026-10-22
**Decided by:** Kris (standing default from the client template, kept for ACN)
**Status:** accepted

## Context

Information about ACN is spread across chats, calls and screenshots. AI chat history does not carry over between sessions, tools or models.

## Decision

Keep everything we know and decide about ACN in this Git repository as Markdown. It should be private; it was public on 2026-10-22 so Claude could read it, and must be made private again. `AGENTS.md` holds the rules and map; `CLAUDE.md` loads it for Claude Code. Every session that changes something leaves a log.

## Why

A new chat or agent reads the repo and is up to speed. Git history shows what changed, when and why. Markdown can later be built into a browsable site.

## Alternatives considered

- Chat history or project memory in one AI tool: does not carry over between tools, models or teammates.
- A shared drive of documents: no history of why things changed, and agents read it poorly.
