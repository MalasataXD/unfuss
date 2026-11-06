---
name: complete-ticket
description: Use when a ticket's implementation is done and needs checking against its acceptance criteria.
---

# Complete ticket

Check whether the work done for a ticket satisfies it: walk each acceptance criterion against the real changes, and report what is met and what isn't. The goal is a trustworthy answer to "is this ticket finished?", not a rubber stamp.

> When you can't see that a criterion is met, it isn't met.

## 1. Find the ticket and the scope

1. **A ticket file**: When the user names one, use it.
2. **Otherwise**: Infer it from the conversation, the current changes and the `In progress` tickets under `<work folder>/tickets/`. The project's `AGENTS.md` or `CLAUDE.md` names the work folder, and when it names none, use `.ai/` at the repository root.
3. **Still unclear**: When several open tickets fit and nothing points to one, ask which to check, rather than guessing.

Read the whole ticket: the `Type` and `Status` lines under the title, `What to build`, `Acceptance criteria`, `Blocked by`, and the `Parent` spec.

Then settle the scope of the changes: the commits the build made since the commit it started from, such as `<start>..HEAD`, plus any uncommitted changes that belong to the ticket.

When the starting commit isn't known, work the scope out from `git log`, `git status` and `git diff`, and ask when unrelated changes are mixed in.

This step is done when you can name the ticket and the scope.

## 2. Hand it to a cold read

The judging goes to a sub-agent. This session often wrote the changes, and an author checking its own work reads the ticket through what it built, not what was asked.

Give the sub-agent two things: the ticket file and the scope. Leave out the conversation, the plan and any explanation of intent. It reads what was actually done, from the commits, the diff and the files they touch. It runs the tests or the app where that is the only way to confirm a behavioural criterion, and fixes or extends nothing.

When you are a sub-agent yourself and can't start another, stop before judging. Hand back to whoever started you, with the ticket and the scope, and ask them to run the cold read.

This step is done when the sub-agent has judged every criterion.

## 3. Judge each criterion

Each criterion gets one of two states, with its evidence graded on the `prove-it` ladder and the rung stated:

1. **Met**: The change demonstrably satisfies it, and the evidence points to the file, function or test that proves it. A behavioural criterion is met only at **Ran it** or higher.
2. **Not met**: Anything short of that. Name what is missing: the work itself, an error path or the proof. A criterion that can't be verified from here, such as a manual step, an environment you lack or a human judgement, is not met, and its evidence names what would verify it.

Then look past the checklist:

1. **Scope**: Does the change match `What to build`, and did it stay inside the ticket?
2. **Loose ends**: Obvious regressions, missing tests, or anything an implementer would be embarrassed to ship.
3. **Safety fact**: For anything risky the criteria don't cover, the `prove-it` safety fact and the rung it reached.

## 4. Report

Present the sub-agent's assessment in the conversation in this shape, without softening its verdicts:

```markdown
## Ticket check: <filename> (<title>)

**Verdict:** Complete / Not done, in one line.
**Type:** RFA / RFH

### Acceptance criteria

| Criterion | State | Evidence or what's missing |
| --- | --- | --- |
| C1 | Met | `path/thing.ts` does X, covered by `thing.test.ts` (Ran it) |
| C2 | Not met | The happy path is done, and the error case in <area> isn't handled |
| C3 | Not met | Needs a manual check of <thing>, and no change addresses it |

### What's left

The remaining work, in order, or "Nothing. All criteria are met."

### Beyond the checklist

Scope drift, missing tests, regressions or loose ends, or "None spotted."
```

## 5. Record the verdict

The verdict is **Complete** only when every criterion is met, and the check beyond the checklist found no regression and no failed safety fact. Then tick every `- [ ]` acceptance box, and set the `Status` line to `Completed`.

> Those are the only edits, and the parent spec stays unchanged.

When the ticket was the last open one in its phase, the `[P<x>]` prefix its title shares with the tickets beside it, say so. Name the phase's verifiable state from the spec's `## Phases` map, because this is the moment the user can check the feature end to end.

On **Not done**, change nothing, and leave the report as the answer.

## 6. Point to the next ticket

On **Complete**, name the next ticket by `next-ticket`'s selection rules, with one line on why it is next, and stop. Planning it is `next-ticket`'s job, once the user asks. When this ticket closed its phase, the end-to-end check comes first, and the next ticket second.

On **Not done**, name no next ticket.
