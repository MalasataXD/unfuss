---
name: vet-tickets
description: Use when tickets or a breakdown need checking for gaps.
---

# Vet tickets

Read a set of tickets as the builder who will pick them up **cold**, with none of the conversation that produced them: only the ticket text, what it points at, and the code. Ask one question of each ticket: *where would I stall?* A **stall** is the moment the builder has to guess at a decision the author knew. Everything else is healthy exploration, and healthy exploration isn't a finding.

> Work read-only: leave the tickets unedited, and write no report file. Present the findings in the conversation, and the caller decides what to do with them.

## 1. Find the tickets

1. **A path or glob**: When the user passes a ticket file, a folder or a pattern, use it.
2. **Otherwise**: Use `<work folder>/tickets/`. The project's `AGENTS.md` or `CLAUDE.md` names the work folder, and when it names none, use `.ai/` at the repository root.

When there are no tickets, say so and stop.

A ticket carries its phase as a `[P<x>]` prefix on its title, and its `Type` and `Status` as lines under the title.

## 2. Find the stall in each ticket

Take the tickets one at a time, the way a builder picks them up. For each one, read the ticket, follow only what it points at, its `Blocked by` tickets and its `Parent` spec, and walk the checklist below. Write the verdict down before opening the next ticket, so each ticket is judged on what it and its references say, not on a sibling you read earlier.

Skim the code only far enough to answer one question: can the area this ticket touches be found from its vocabulary and the existing seams? Stop there. A file path or line number you found means the builder will find it too.

Stop at the **first** stall, so each ticket gets one finding. The builder finds a second stall once the first is fixed.

1. **Done**: Can you say in one sentence what "done" looks like? A `What to build` that names a feature in one sentence, without its end-to-end behaviour, is a stall.
2. **Start**: Can you tell which part of the system this touches? Naming files isn't the ticket's job, but naming the area is.
3. **Criteria**: Could two builders disagree on whether an acceptance criterion is met?
4. **Decisions**: Does the ticket assume a choice it never states, such as a data shape, an edge-case behaviour or a UX detail, that you would have to invent?
5. **Verification**: Could you test or demo this ticket on its own?

Report a stall only when the builder would have to **guess**. When they would only have to look something up, the ticket holds. None of these is a stall:

1. **Style**: Wording you would have phrased differently, or a sentence that would be nice to add.
2. **Defined elsewhere**: A term defined in another ticket of the set, in the spec or in the glossary.
3. **In the code**: Anything reading the code answers, such as the right function, an idiom or a name.

Each ticket gets one of two verdicts:

1. **Ready**: No stall. A ready ticket gets its row in the table and nothing else.
2. **Blocked**: One stall, quoted against the ticket, with the question whose answer removes it. Ask the question, and leave the answer to the author.

When the caller asks only for the checks across the set, skip this step.

This step is done when every ticket has a verdict.

## 3. Check the set

With every verdict written down, read the whole set together and check the breakdown:

1. **Phases**: Does each phase end in a state that can be verified or demoed once its tickets are complete?
2. **Blockers**: Does every `Blocked by` name a ticket that exists in the same phase? Do two unblocked tickets in one phase touch the same code, or does one use what the other builds?
3. **Coverage**: Is there a step between tickets that nobody owns?
4. **Overlap**: Do two tickets claim the same work?

## 4. Report

Present the report in the conversation in this shape. Lead with the table, so the state of the set shows at a glance, and quote the ticket and the exact question for each stall:

```markdown
## Ticket vet: <scope> (<n> tickets)

| Ticket | Verdict |
| --- | --- |
| 1-1-account-balance-endpoint.md | Ready |
| 1-2-balance-display.md | Blocked |

### 1-2-balance-display.md: Blocked

- **Stall:** The first question the builder hits, quoted against the ticket.
- **Question for the author:** The decision that removes it.

### Across the set

Phase, blocker, coverage or overlap findings, or "No problems across the set."

### Bottom line

One sentence: ready to hand off, or the one thing to fix first.
```
