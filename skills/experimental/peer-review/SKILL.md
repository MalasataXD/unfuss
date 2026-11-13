---
name: peer-review
description: Use when the user asks for a peer review by other models.
disable-model-invocation: true
---

# Peer review

A second opinion from outside this conversation. **Peers** each run the `review` skill on the same target, cold, with none of this conversation in their context. This session is the **host**: it checks what comes back, merges it and presents it. The value is in what the peers see that this conversation's assumptions hide, and models from other providers add blind spots of their own that don't overlap.

> Present only. The host changes no code, and the peers write nothing but their review reports.

## 1. Pin the target and the panel

1. **Target**: A ref, a range, a pull request, a path or the working tree, as the user names it. Pin it to commits, so every peer reads the same code. For uncommitted changes, also save `git diff HEAD` to a file in the work folder, with each untracked file from `git ls-files --others --exclude-standard` added to it as a new file, and give each peer its path. The project's `AGENTS.md` or `CLAUDE.md` names the work folder, and when it names none, use `.ai/` at the repository root.
2. **Panel**: The models the user names. Otherwise, the newest Opus, Sol and Grok models at `high` effort. A peer from this session's own provider is still a fresh run, never this session.
3. **Jester**: When the user names a model for it, that model joins as the jester. Instead of a review, it makes the strongest case that the change is wrong, from the angle the others are least likely to take.

This step is done when the target is pinned and each peer is named.

## 2. Dispatch the peers

Run every peer in parallel, as a fresh run through whatever the harness offers for its model, such as `delegate_task` in T3 Code, or a skill installed for it. A peer the harness can't reach is noted for the report. When none can be reached, say so and stop.

Each brief carries only these, so each peer starts unanchored by this conversation:

1. **The target**: Its commits and any diff file, and the user's question when they gave one.
2. **The task**: Run the `review` skill on the target, save the report as `<work folder>/reviews/YYYY-MM-DD-<slug>-<model>.md`, and change nothing else. When the skill isn't installed, return each finding with a title, the file and lines, the problem, a verdict of Act on, Consider or Noted, and how sure it is. The jester's task is its case instead, returned in the conversation.

Re-dispatch a peer that fails once. When it fails again, note it for the report and carry on with the rest.

This step is done when each peer has returned its report or failed twice.

## 3. Weigh the findings

Read every report. Merge the findings that name the same problem, and note which peers raised each one. Then check each finding against the code yourself, and sort it:

1. **Confirmed**: The code shows the problem.
2. **Unsure**: Plausible, but the code doesn't settle it. Say what would.
3. **Dismissed**: Wrong or out of scope, backed by evidence at **Cited** or better on the `prove-it` skill's ladder.

Keep each finding in its peer's wording, with its peer's verdict. Weigh the jester's case the same way: does it hold against the code?

This step is done when every finding is merged and sorted.

## 4. Present

Rank the confirmed findings by verdict, then by how many peers raised them. Leave out a section with nothing in it.

```markdown
## Peer review: <target> (<short base>..<short head>)

**Panel:** Each peer, its model and effort, and its report path.

### Confirmed

#### <n>. <title>

**Raised by:** <peers> · **Verdict:** <the strongest verdict a peer gave>
**Where:** The file and lines.
**Problem:** In the peer's words.
**Your take:** What you recommend, and why.

### Unsure

- <title> (<peers>): What would settle it.

### Jester's case

The case in one paragraph, and whether it holds.

### Dismissed

- <title> (<peers>): The evidence against it.

**Failed:** The peers that returned nothing.
```
