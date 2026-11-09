---
name: to-spec
description: Use when the current conversation needs writing up as a spec.
---

# To spec

Turn what the conversation already settled, and what you know of the code, into a spec saved as a local Markdown file. The conversation is the source, so write it up rather than interviewing the user. The user is asked only to approve a refactor or the seams, and to settle a decision the conversation left open.

## 1. Read the code

Explore the parts of the codebase the feature touches, unless you already have. Read `GLOSSARY.md` or `GLOSSARY-MAP.md` when either exists, and the `Accepted` ADRs covering the area, and name things the way they do throughout the spec.

When the conversation left a decision open, ask it rather than settling it in the spec. Several tangled ones are a `grilling` session.

This step is done when you can name the modules the feature touches, and every decision the spec needs is either settled or waits on a research question.

## 2. Look for a preparatory refactor

Apply the `complexity` skill's deletion test to the modules the feature touches. When deepening one would make the feature cheaper to build or test, propose it to the user as a refactor to do first. An approved one goes into the spec as its own decision. When nothing qualifies, move on without comment.

## 3. Agree the seams

Sketch the seams the feature will be tested at. Prefer an existing seam, and take the highest one that still observes the behaviour. When a new one is needed, propose it at the highest point you can, designed with the `complexity` skill.

Check with the user that the seams match what they expect.

This step is done when the user has confirmed the seams.

## 4. Check each decision

Run each implementation decision through the ledger in the `complexity` skill. Every new module, dependency, config option and special case names what pays for it before it goes in the spec.

## 5. Write and save it

Write the spec in the shape below, and save it to `<work folder>/specs/<slug>.md`. The project's `AGENTS.md` or `CLAUDE.md` names the work folder, and when it names none, use `.ai/` at the repository root. The slug is short kebab-case named after the feature, such as `specs/account-balance-display.md`. When that file already exists, confirm with the user before overwriting it.

The spec is read by the agents building the feature, so it carries decisions, not narrative. Each section is a short paragraph or a bullet list, and a spec longer than a screen or two is carrying something the tickets should carry instead.

```markdown
# <Feature name>

## Goal

One paragraph: what is wrong or missing today, and what is true once this ships.

## Decisions

One bullet per decision, stating the decision and why it beat the alternative. Cover the modules built or changed and their interfaces, schema changes, API contracts, architectural choices, and any clarification the user gave. Each new module, dependency, config option or special case names what pays for it.

## Testing

The seams the feature is tested at, what a good test there looks like, judged by external behaviour only, and prior art for such tests in the codebase.

## Required research

One bullet per question that has to be answered from a primary source, such as official docs, a third-party API or a standard, before dependent work can be built. Each names the decision that waits on its answer. When the spec is broken down, `to-tickets` answers each one with the `research` skill before writing any ticket. Leave this section out when there are none.

## Out of scope

One bullet for each thing this spec leaves out on purpose.
```

Leave file paths and code snippets out of the decisions, because they go stale fast. The exception is a prototype snippet that states a decision more precisely than prose can, such as a state machine, a reducer, a schema or a type shape. Inline it in its decision, trimmed to the parts that carry the decision, and note that it came from a prototype.

This step is done when the spec is saved and you have given the user its path.
