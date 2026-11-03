---
name: decision-records
description: Use when a hard-to-reverse decision is made, or an existing ADR needs to change.
---

# Decision records

An **ADR** (architecture decision record) captures one decision a future reader would question: the context that forced it, the choice made and the consequences accepted. ADRs are rare on purpose, and most decisions never get one.

The user decides every ADR. You propose, and nothing in `docs/adr/` is written or changed until the user says yes. The one exception is a `Proposed` ADR written when no one is there to answer (step 2).

## Where they live

- **System-wide decisions**: `docs/adr/` at the root.
- **Decisions for one context**: That context's own `docs/adr/`, when a `GLOSSARY-MAP.md` at the root lists several contexts.
- **Filename**: `NNNN-short-kebab-title.md`, with the next zero-padded number in that folder.

Create `docs/adr/` with its first ADR. The folder listing is the index, so read the titles and statuses there before any change: the decision may already have an ADR, or clash with one.

## Statuses

`Proposed` → `Accepted` → `Superseded by NNNN` or `Deprecated`.

Only a `Proposed` ADR can be edited. In an `Accepted` one, only the status line changes, through the operations below. Fixing a typo or a broken link is the one exception.

## Propose an ADR

1. **Test it**: An ADR is warranted only when all three hold:
   - **Hard to reverse**: Changing your mind later is costly.
   - **Surprising without context**: A future reader will ask why it was done this way.
   - **A real trade-off**: There were genuine alternatives, and this one won for specific reasons.
2. **Put it to the user**: Name the decision and answer each test in one line. When a test fails, say so and name where the decision belongs instead, such as the spec, the commit message or a code comment. With no one there to answer, write it as `Proposed` and name it in the report.
3. **Write it**: After the user's yes, write it with the template below. It starts as `Accepted` when the user has made the decision, and as `Proposed` while it waits on someone else, such as a team review.

## Change an ADR

- **Accept**: When the user confirms a `Proposed` ADR, set its status to `Accepted`.
- **Reject**: When the user turns down a `Proposed` ADR, delete it. It was never a decision, so there is no history to keep.
- **Supersede**: When a new decision replaces an accepted one, propose the new ADR as above, with `Supersedes [NNNN](NNNN-....md)` in its header and the reason for the change in its Context. Once it is `Accepted`, set the old one's status to `Superseded by [MMMM](MMMM-....md)`.
- **Deprecate**: When a decision stops applying and nothing replaces it, set its status to `Deprecated`, with the date and a one-line reason.

## Template

```markdown
# NNNN. Short imperative title ("Use Postgres for the write model")

- **Status:** Proposed | Accepted | Superseded by [NNNN](NNNN-....md) | Deprecated (YYYY-MM-DD): <reason>
- **Date:** YYYY-MM-DD
- **Supersedes:** [NNNN](NNNN-....md) (only when it replaces an ADR)

## Context

The problem, the constraints and the relevant facts, written for a reader who wasn't there. Name the genuine alternatives.

## Decision

"We will …", with the reasons this option beat the alternatives.

## Consequences

What becomes easier and what becomes harder: the costs accepted, the constraints added and the follow-on work created.
```

One decision per ADR. Two choices are two ADRs.

## Respecting ADRs

Before changing an area, read the `Accepted` ADRs that cover it. A change that contradicts one names it and goes to the user as a proposal to supersede it.
