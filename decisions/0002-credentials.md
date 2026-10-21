# 0002 — Where logins and secrets are kept

**Date:** 2026-10-22
**Decided by:** pending (Kris)
**Status:** proposed (standing default from the client template)

## Context

We will hold client logins (hosting, domain, Google, site builders and similar). The team wants everything in one place. Plain-text passwords in Git are a risk: they stay in history forever, sit in every clone, and are read by every AI session that opens the repo.

## Options

1. **Shared password manager vault** (for example Bitwarden or 1Password, one "ACN" vault). The repo records which logins exist and "kept in: ACN vault". Recommended: simplest, revocable per person, nothing secret in Git.
2. **Encrypted file in the repo** (SOPS with age, or git-crypt). Secrets live next to the docs but are unreadable without a key held by the team.
3. **Plain text in the private repo.** Not recommended.

## Decision

Pending: Kris to pick a password manager. Until decided, no plain-text secrets in the repo (see [AGENTS.md](../AGENTS.md#credentials)). Several logins were pasted into a chat on 2026-10-22; they must be changed when they move into the vault ([map/accounts.md](../map/accounts.md)).
