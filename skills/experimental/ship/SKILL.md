---
name: ship
description: Use when deciding whether finished work is ready to ship.
---

# Ship

A **gate** for finished work. `review` and `complete-ticket` judge the work, and this skill reads their results and derives **Ready** or **Not ready**. Each gate is a yes or no read from the record, so two runs reach the same verdict. The review's score stays out of it, because a weighted average can hide a critical finding.

> Verdict only. The user decides what happens next.

## 1. Pin the branch

Take `HEAD` as the head, and its merge base with the target branch as the base. The target is the branch the user names, or otherwise the default branch. When the diff between them is empty, say so and stop.

Only committed work is judged. Note any uncommitted files for the report.

This step is done when you can name the base and the head.

## 2. Find a current review

Look in `<work folder>/reviews/` for the newest report whose **Scope** shows this base and head. The project's `AGENTS.md` or `CLAUDE.md` names the work folder, and when it names none, use `.ai/` at the repository root.

When no report matches, run the `review` skill on the pinned range. A report on the working tree or on an older head doesn't count, because the code it judged may not be the code being shipped.

This step is done when you have a report for exactly this base and head.

## 3. Apply the gates

Read the report and apply every gate. A gate passes only on what the record states, so a missing or unclear line fails it.

| Gate | Passes when | When it fails |
| --- | --- | --- |
| **Findings** | No finding in the report has the verdict **Act on**. | Fix the findings, commit, and run `ship` again. |
| **Checks** | **Coverage** shows that every check the project defines, in its scripts or CI workflow, ran and passed. | Fix a failed check, commit, and run `ship` again. Run `review` again for a check that didn't run. |
| **Coverage** | **Coverage** records no dimension as not assessed because its reviewer failed. | Run `review` again. |
| **Spec** | **Spec compliance** is **Met**, or "Not assessed" because no spec was found or a ticket exists. | Close the gap, commit, and run `ship` again. |
| **Tickets** | Every ticket that **Spec compliance** names has the `Status` `Completed`. With no ticket named, it passes. | Run `complete-ticket`. |

This step is done when every gate has a result.

## 4. Report the verdict

The verdict is **Ready** only when every gate passes. Present it in the conversation:

```markdown
## Ship check: <branch> (<short base>..<short head>)

**Verdict:** Ready / Not ready

**Review:** `<report path>`

| Gate | Result | Evidence |
| --- | --- | --- |
| Findings | Pass / Fail | The titles of the **Act on** findings, or "None". |
| Checks | Pass / Fail | Each check and its result. |
| Coverage | Pass / Fail | The dimensions not assessed, or "All assessed". |
| Spec | Pass / Fail | The verdict, or why it was not assessed. |
| Tickets | Pass / Fail | Each ticket and its status, or "None named". |

**Next:** The `file-pr` skill when **Ready**. Otherwise, the fix for each failed gate, in table order.

**Uncommitted:** The files left out of this check. Leave the line out when there are none.
```
