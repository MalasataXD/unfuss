---
name: next-ticket
description: Use when picking up the next piece of work or open ticket.
---

# Next ticket

Pick up the next open ticket from a local breakdown, plan it against the real code, and start the build. The plan is the user's chance to catch a mistake before any code is written: an open question stops the run until they answer it, and a plan with none goes straight to `implement`, left on screen so they can interrupt.

## 1. Find the tickets

1. **A ticket file**: When the user names one, use it and go to step 3.
2. **A folder or glob**: When the user names one, search there.
3. **Otherwise**: Look in `<work folder>/tickets/`. The project's `AGENTS.md` or `CLAUDE.md` names the work folder, and when it names none, use `.ai/` at the repository root. Each breakdown is a subfolder named after its spec. With one subfolder, use it, and with several, ask which.

When there are no tickets, say so and stop.

This step is done when you have one breakdown, or one ticket file.

## 2. Pick the next open ticket

Read only the headers: the `[P<x>]` phase in each title, the `Type` and `Status` lines under it, and the `## Blocked by` section. A ticket is **open** when its status is `Not started` or `In progress`.

1. **Started work first**: When a ticket is `In progress`, finishing it beats starting new work. Name it, and confirm the user wants to continue it.
2. **The lowest phase**: Otherwise, stay in the lowest phase with an open ticket, because a later phase waits until every ticket in the current one is `Completed`. In that phase, pick the lowest-numbered `Not started` ticket whose blockers are all `Completed`.
3. **Everything blocked**: When every open ticket in the phase waits on unfinished work, name what has to finish first, and stop.

This step is done when you have stated which ticket you picked, and why.

## 3. Read the ticket

Read the whole ticket, and skim its `Parent` spec for the context the ticket assumes.

The `Type` decides where the user is needed. An `RFA` ticket (ready for agent) runs end to end with no one in the loop. An `RFH` ticket (ready for human) needs the user at a specific point, such as a design review or a credential, and the plan names exactly where.

A ticket can still hold an open question, such as a fact nobody looked up or a choice nobody made. Settle it with the `research` or `grilling` skill before planning, rather than planning around a guess.

This step is done when the ticket holds no open question.

## 4. Explore the code

Ground the plan in the code it will touch: the modules, the seams and the patterns already in use, so each step names a real integration point.

1. **Language**: Read `GLOSSARY.md` or `GLOSSARY-MAP.md` when either exists, and the `Accepted` ADRs covering the area, and name things the way they do.
2. **Complexity**: Run anything the plan adds, such as a new module, a dependency or a special case, through the ledger in the `complexity` skill.

Give the reading to sub-agents, one for each area the ticket touches, and have each return the modules, seams and patterns it found, not the files it read. Write the plan yourself, from their reports. When you are already a sub-agent, do the reading yourself.

This step is done when every step of the plan can name the code it touches.

## 5. Present the plan

Present the plan in the conversation in this shape:

```markdown
## Next ticket: <filename> (<title>)

**Type:** RFA / RFH

**Goal:** One sentence on what "done" looks like, drawn from the acceptance criteria.

### Plan

1. A step tied to an integration point (*covers: acceptance criterion X*)
2. …

### Where I'll need you

The specific decision or review point, and why it can't be automated. Leave this section out for an RFA ticket.

### Verification

How the finished ticket will be tested or demoed on its own, mapped to the acceptance criteria, with the `prove-it` rung each one needs.

### Open questions

Every point whose answer would change the plan, such as a criterion you would have to interpret, or a choice between approaches the code doesn't settle. Otherwise, "None."
```

The open questions decide what happens next:

1. **None**: Continue to step 6 in the same turn. Write "None." only when every step of the plan follows from the ticket and the code, because it is what starts the build.
2. **Any question**: Stop, and wait for the user to answer, adjust the plan or redirect.

## 6. Start the build

The go-ahead is a plan with no open questions, or the user's answers to them. Set the ticket's `Status` line to `In progress`, and run the `implement` skill on the plan.

> The status line is the only edit to the ticket here. The acceptance criteria are ticked by `complete-ticket`.
