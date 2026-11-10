---
name: retro
description: Use when the user asks for a retrospective on a coding session.
disable-model-invocation: true
---

# Retro

Look back at a coding session for **friction**: the moments the agent was slowed down, went wrong or was corrected. Then suggest changes to the agent's environment that would remove that friction next time. The environment is everything around the agent: its steering files, checks, skills, docs and tools. The code the session wrote is out of scope.

> Suggest only. Change a file once the user picks its candidate.

## 1. Read the session

Use the session the user names, or the current one, which is already in your context. For an earlier session, or the part of the current one that a compaction dropped, read `references/session-logs.md`.

Friction shows up as:

1. **Corrections**: The user corrected, redirected or overruled the agent. These are the strongest signal.
2. **Searches**: Many calls to find a file, a command or a fact.
3. **Retries**: A command, edit or test that failed and was tried again.
4. **Mistakes**: An error that got past the agent and was found later, by a review, a check or the user.
5. **Expensive calls**: A tool call whose output was far bigger than the agent needed.

This step is done when every friction moment is listed with its quote or turn.

## 2. Find the candidates

Load the `writing-for-agents` skill. For each friction moment, ask what change to the environment would have prevented it, using the categories below. Read the files a candidate would change, so it builds on what is there. One candidate can cover several moments. Drop a moment that no change would have prevented, or that cost little and is unlikely to happen again.

1. **Navigation**: The agent searched for something a **navigation pointer** would have led it to, or tripped on a hidden dependency between files.
2. **Automated checks**: A lint, type check, test or filesystem check would have caught the mistake. Read the repo's own check commands first, in its `package.json` or build-tool scripts and its CI workflow, because a check that exists but is unwired or silently broken is the finding. A repo with no **guardrail**, meaning no pre-commit hook and no CI job running its checks, is a finding on its own.
3. **Coding standards**: The review missed a mistake. A **mechanical** violation, such as a fixed syntactic pattern, a banned API, an import shape or a file-location rule, gets a check: a rule in the repo's own linter, a pre-commit hook or a CI job, whichever is cheapest there. A **judgement call**, such as consistency across files or matching the surrounding style, goes in `CODING_STANDARDS.md`. A rule that is wrong or unclear gets removed or clarified.
4. **Skills**: A skill didn't fire when it should have, fired when it shouldn't have, or the agent skipped or misread one of its steps. The fix is usually in the description or the step's wording. A skill installed from another repo is fixed in that repo, not in the installed copy.
5. **Steering files**: An instruction in `AGENTS.md` or `CLAUDE.md`, in the repo or the user's global scope, that belongs lower in step 3's order, or that changes nothing about how the agent behaves.
6. **Tool economy**: A tool, CLI or MCP server whose calls cost far more tokens than the job needed, and a cheaper way to the same answer.
7. **Information access**: Something the agent needed and couldn't reach, such as dev server logs written to a file, or read-only access to a third-party service.

Two findings need no friction moment, so check for them in every retro: a repo with no guardrail, and an `AGENTS.md` or `CLAUDE.md` large enough to hide instructions that change nothing. Report them after the candidates, one line each, and only when found.

This step is done when every friction moment has a candidate or was dropped, and both standing checks are done.

## 3. Place each fix

Put each fix at the first place in this list where it will still work. Each place down the list costs more context, for every agent in every session.

1. **A check**: It catches the mistake with no instruction at all.
2. **`CODING_STANDARDS.md`**: It is read at review, not while building. The builder carries the exploration and debugging, while the reviewer starts from a diff with room to spare, so standards belong to the review. When the file grows past about 1,000 lines, move sections into docs behind navigation pointers.
3. **A skill or a doc**: A skill for steps or a command the user runs, since only its description is always loaded. A doc for reference, reached through a pointer from another file. Look for an existing one before writing a new one.
4. **`AGENTS.md` or `CLAUDE.md`**: It is loaded into every agent's context, so it holds navigation pointers and little else.

This step is done when each candidate names the file it changes.

## 4. Present the candidates

Order them by severity: what the friction cost the session, and how likely it is to happen again.

```markdown
### <n>. <title: the change, not the category>

**Category:** One from step 2.
**Friction:** What happened, with the quote or turn.
**Change:** The file, and the text, rule or check to add, change or remove.
```

When the session had little friction, say so: a short retro is a good result.

This step is done when every candidate is presented. Then ask which to apply, and apply the ones the user picks, in the style of `writing-for-agents`.
