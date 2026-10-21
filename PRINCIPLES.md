# How we work

Our working principles, written down so every session and agent works the same way. [AGENTS.md](AGENTS.md) holds the short, binding version; this file explains them.

## Two modes

Not every conversation needs the full rulebook. Pick the mode from what the session is about to touch.

### Brainstorm mode (the default for conversation)

For talking ideas through, research, analysis, drafting, and writing inside this repository.

- **Start immediately.** No restating, no three questions, no waiting for a yes. Ask questions alongside the first results, not before them.
- Pareto still applies: lead with what matters most.
- Nothing outside this repository changes in this mode.

### Work mode (trial by fire)

For anything that touches **code**, a **client property** (live site, staging, DNS, email, accounts, listings), a **deploy**, or anything **hard to undo**.

- Run the [confirmation gate](AGENTS.md#confirmation-gate) and the standing workflow: restate the ask, say what you understood and what is out of scope, ask **at least three** clarifying questions (which repo, which environment, which page, what "done" means, what must stay as it is), and wait for an explicit yes before writing code.
- Chesterton's fence is front and center.
- Pareto decides the order of the work.

### Switching

- Moving from brainstorm to work mode happens the moment a session is about to change code or anything outside this repository. Say so in one line ("This touches the live site, so switching to work mode") and run the gate.
- The person you are working with can also switch explicitly: "work mode" or "brainstorm mode".
- When unsure which mode applies, it is work mode.

## Chesterton's fence

Do not remove, rename, or clean something up until you know why it is there.

That includes code, config, copy, a redirect, a branch, and a step in a process. If you cannot say what it is for, leave it. Find out first. Then ask before you change it.

Where to find out: [decisions/](decisions/) for why, [logs/](logs/) for what happened, then ask a teammate. On an old website this matters a lot: an odd URL like `/contact-2-2/` may still carry links or rankings, so it gets a redirect, not a deletion.

## Pareto principle

Start with the small part of the work that carries most of the result. About 20% of the effort usually drives about 80% of the outcome.

When that high-leverage path is clear, do it first. Do not spend the session on low-impact busywork while the useful path is still open.

## Jakob's law

Users spend most of their time on other sites, so they expect ours to work the same way. Design for familiarity and for the client's actual buyers, not for design awards: visible, labeled navigation, the main action where people expect it, no hidden menus. Utilitarian by choice. See [decision 0003](decisions/0003-design-for-familiarity.md).

---

Authored by Krischan.
