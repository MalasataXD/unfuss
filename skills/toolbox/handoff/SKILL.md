---
name: handoff
description: Use when the user asks for a handoff of the current conversation to another agent.
argument-hint: "What will the next session be used for?"
disable-model-invocation: true
---

# Handoff

Write the current conversation down as a handoff, so a fresh agent can pick up the work without it. The handoff carries what only the conversation knows: where the work stands, what was decided and why, and what was tried and failed. Everything already written down elsewhere gets a pointer instead.

> Never include secrets or personal data, such as API keys, passwords and personal details.

When the user passes arguments, they describe what the next session will focus on, so shape the handoff around that.

## 1. Gather the state

Note the date, the branch and the short hash of `HEAD`. Run `git status` for changes that aren't committed, since the commit hash doesn't cover them. Outside a git repository, note the date and the files the session changed instead. The header then names those files, and the handoff is stale when any of them changed after that date. Collect the paths and URLs of the specs, tickets, plans, ADRs, PRs and reports the conversation used or produced.

This step is done when you have the commit and the uncommitted changes, or the changed files outside git, and the paths of the artifacts.

## 2. Write the handoff

Save it to `<work folder>/handoff.md`. The project's `AGENTS.md` or `CLAUDE.md` names the work folder, and when it names none, use `.ai/` at the repository root, or in the current folder outside a repository. There is one handoff file, and each handoff overwrites it: the next agent needs one known path, and a handoff describes a moment, so the old one is worth less than its replacement.

Point to what the artifacts already say, by path or URL, instead of repeating it. Give each decision and dead end a line or two: the handoff is a summary, not a transcript.

```markdown
# Handoff: <topic>

> **Written:** YYYY-MM-DD at commit `<short hash>` on `<branch>`. Git is the source of truth, and this file is a claim about it. If commits landed after that hash, this handoff is stale: rebuild where things stand from `git log` and `git diff`, keep the decisions and notes that still hold, and rewrite this file. Even when it is fresh, check what it claims against the diff before building on it.

## Where things stand

What is done, what is in progress, and the uncommitted changes. What was verified, and how. The next concrete step. Each open question waiting on the user, with the options on the table.

## Decisions

Each decision the conversation made that no artifact records, and why.

## Notes for the next agent

What was tried and failed, the gotchas found, and anything the user asked to leave alone.

## Suggested skills

The skill to reach for next, and the ticket, spec or PR it applies to:
- `next-ticket` when a ticket is chosen but not yet planned.
- `implement` when a plan is approved and the build is underway.
- `complete-ticket` when the work looks finished.
- `diagnosing-bugs` when the session was chasing a symptom.
- `babysit-pr` when a PR is open and waiting on checks.
```

Leave out a section that has nothing to say.

This step is done when the file is saved, and every decision and dead end from the conversation is in it or in an artifact it points to. Then give the user its path.
