---
name: to-tickets
description: Use when breaking work, a plan or a spec down into tickets.
---

# To tickets

Break a plan into **phases** of tickets. A phase is a group of tickets that, once all are complete, leaves the feature in a state the user can verify or demo. Phases run in order. The tickets inside a phase are independent unless one says otherwise, so every unblocked ticket in the current phase can be picked up at once.

## 1. Gather the context

Work from what is already in the conversation. When the user passes a path, a spec or a plan, read all of it before grouping. Specs live under `<work folder>/specs/`. The project's `AGENTS.md` or `CLAUDE.md` names the work folder, and when it names none, use `.ai/` at the repository root.

When the work has no spec yet, stop and say so, and offer the `to-spec` skill to write one first. The tickets and the phases map need a spec as their parent.

When you haven't explored the code yet, give the reading to sub-agents, one for each area the spec touches, and have each return the modules, seams and patterns it found, not the files it read. When you are already a sub-agent, do the reading yourself.

Read `GLOSSARY.md` or `GLOSSARY-MAP.md` when either exists, and the `Accepted` ADRs covering the area, and name things the way they do. When a ticket adds a module, a dependency or a special case the plan didn't account for, run it through the ledger in the `complexity` skill before writing it.

This step is done when you can name the modules each part of the plan touches.

## 2. Settle the open questions

Settle every open question before drafting phases, because an answer can change them. A ticket carries no open question, either as its own work or as an assumption hidden inside a build.

1. **Facts**: Anything to look up in docs, APIs or standards, including each question in the spec's `## Required research` section, goes to the `research` skill, one agent per question, in parallel. Wait for the findings, because the choices may rest on them.
2. **Choices**: A choice nobody has made goes to a `grilling` session with the user, with the findings in hand. A choice best judged from throwaway code gets a `prototype` first, and a hard-to-reverse outcome is recorded with the `decision-records` skill. The outcome lands in the tickets it shapes.
3. **Questions only the work can answer**: A question that can only be answered once some of the work has landed means the spec is too big. Stop and say so, rather than ticketing around a guess.

A ticket that rests on a findings file or an ADR links to it, rather than restating it.

This step is done when no question is open.

## 3. Group the work into phases

Draft the phases first, then the tickets inside each one:

1. **Phases**: Each one ends in a **verifiable state**, something that can be demoed, tested or checked end to end. Name the phase after that state.
2. **Tickets**: A ticket is the work one agent finishes in one session. Prefer several small tickets to one large one. Keep the tickets in a phase off the same code, where a different split allows it.
3. **Blockers**: A ticket that touches the same code as another in its phase, or uses what another builds, is **blocked by** it. Block only within a phase, because the phases are already in order.

## 4. Confirm with the user

Present the phases as a list: each phase's number, name and verifiable state, then each ticket's title, type and blockers. Ask one question: approve it, or say what to move, merge or split. Iterate until it is approved.

This step is done when the user has approved the phases.

## 5. Write the tickets

Tickets are local Markdown files, kept out of any external tracker.

1. **Folder**: Write them to `<work folder>/tickets/<spec-slug>/`, and create the folder when it is missing.
2. **Files**: One file per ticket in the shape below, named `<phase>-<n>-<slug>.md`, such as `1-2-stock-lists-hook.md`, so the phase and order show at a glance. When a file with that name already exists, confirm with the user before overwriting it.
3. **Order**: Write blockers first, so `Blocked by` can name the real filename.
4. **Phases map**: Append a `## Phases` section to the parent spec, as a map of the breakdown: one heading per phase with its verifiable state, then its ticket files as a list.

> The `## Phases` map is the only edit to the spec, and every other source stays unchanged.

Write `What to build` without file paths or code snippets, because they go stale fast. The exception is a prototype snippet that states a decision more precisely than prose can, such as a state machine, a reducer, a schema or a type shape. Trim it to the parts that carry the decision, and note that it came from a prototype.

```markdown
# [P<x>] <title>

Parent: specs/<slug>.md
Type: RFA
Status: Not started

## What to build

Two to five sentences of end-to-end behaviour.

## Acceptance criteria

- [ ] A concrete, verifiable criterion

## Blocked by

- 1-1-<slug>.md, or `None`
```

1. **Type**: `RFA` (ready for agent) can be built and merged with no one in the loop. `RFH` (ready for human) needs a person during the work, such as for a design review or a credential. Prefer `RFA`.
2. **Status**: `Not started`, `In progress` or `Completed`. A new ticket is always `Not started`, and the skills that pick up and check tickets move it on.

Keep `Type` and `Status` on one line each, because the skills that pick up and check tickets read them there.

This step is done when every ticket is written, the spec has its map, and you have given the user the paths.

## 6. Vet them once

The files are what a builder picks up, so vet the files, cold, with sub-agents running the `vet-tickets` skill. Give each sub-agent about five tickets in a row. When there are more than five, add one more sub-agent over the whole set that runs only the checks across the set. With more than five, set aside the slices' findings across the set, because the whole-set sub-agent owns those checks.

Then sort every finding yourself:

1. **Answered already**: When the answer is in the spec or the conversation, edit the ticket and list the edit.
2. **Needs a decision**: Only a finding that needs a decision nobody has made becomes a question for the user.

Re-vet at most once: only the tickets you edited, and only when one of them was **Blocked**. After that, report what is left rather than looping again.

This step is done when every finding is edited in, put to the user or reported.
