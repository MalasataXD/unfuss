---
name: review
description: Use when reviewing code, or asked for a quality check.
---

# Review

Judge code on two axes:

1. **Standards**: What it should meet, scored with the [scoring-rubric](references/scoring-rubric.md).
2. **Spec**: What it is meant to do, settled as a verdict.

A review checks correctness inside the target and its direct callers. Following it further, into failure paths, stored data, outside consumers and rollback, is the `bug-hunt` skill's job. A review runs it on risky changes and offers it otherwise.

## 1. Pin the target

Settle exactly what you are reviewing before reading anything: a ref (commit, branch, tag or pull request), a path, a directory, a named function, or the working tree. Confirm that it resolves, then fix what it means:

1. **A ref**: Resolve it to a base SHA and a head SHA. The base is the parent for a commit or tag, and the merge base with the target branch for a branch or pull request. When the ref isn't checked out, check it out in a worktree at its head, so callers and line numbers match.
2. **The working tree**: `HEAD` plus staged, unstaged and untracked files.
3. **A path, directory or function**: The code as it stands at the current head SHA.

When a ref or the working tree has an empty diff, say so and stop, so the review stays inside its target. State the target and its SHAs in the report, so the review can be repeated against the same code.

## 2. Gather the standards

Standards apply in three layers, highest precedence first:

1. **The project's own**: `CODING_STANDARDS.md`, `AGENTS.md` or `CLAUDE.md`, `CONTRIBUTING.md`, style guides, and the `Accepted` ADRs covering the area. They override the layers below.
2. **The baseline**: The `code-smells` skill for structure, `complexity` for symptoms and causes, and `security-check` for vulnerabilities.
3. **Idioms**: The language's and community's idioms, and the conventions of the surrounding code.

Leave out anything the formatter, linter or type checker enforces, because that belongs in the pipeline. Read `GLOSSARY.md` or `GLOSSARY-MAP.md` when either exists, so findings use the project's vocabulary.

## 3. Run the project's checks

Run the tests, linter and type checker once, before any sub-agent starts, and keep the output for the brief. Note any check that can't run, and why.

## 4. Review in parallel

Use one sub-agent per dimension, so one lens can't colour another:

1. **Correctness**: Does the changed code do what its names, types, tests and stated intent promise? Name the `prove-it` safety fact and its rung, then check its edge inputs and its own error paths. When the change alters what a function promises, check its direct callers. Leave everything past them, such as stored data, outside consumers and rollback, to `bug-hunt`.
2. **Security and performance**: The `security-check` trace, then algorithm cost, resource handling, N+1 queries, and caching that is missing or wrong.
3. **Quality, style and documentation**: The `code-smells` baseline, naming, project conventions, documentation of public interfaces, and whether tests cover the changed behaviour without being weakened to pass.
4. **Architecture and complexity**: The `complexity` skill and its references, separation of concerns, dependency direction, change amplification, cognitive load and unknown unknowns.
5. **Spec compliance** (*Optional*): Only when a spec or plan exists for the change and no ticket does. Look in `<work folder>/specs/`, the conversation, and the commit messages. Does the change do what was specified, no less and no more? A ticket's verdict belongs to `complete-ticket`.
6. **Bug hunt** (*Optional*): Only when the change writes or migrates stored data, or changes a public contract such as an API, an event or a file format. Run the `bug-hunt` skill's hunt on the same target, and return its findings in this envelope with no scores.

Give every sub-agent the same brief, built from the written record so the author's reasoning can't anchor it:

1. **Target**: The pinned target and the output of the project's checks.
2. **Standards**: The gathered standards and the glossary.
3. **Intent**: The commit messages, the pull request description, and any spec or plan.
4. **Dimension**: Its dimension, plus the rubric's opening rules and the sections for its categories.
5. **Format**: All of [finding-format](references/finding-format.md), pasted verbatim, so every dimension comes back in one shape.
6. **Rules**: Work read-only, and treat a baseline entry as a reason to look closer. Say why this instance matters, with evidence a reader can check: the file, the line, and what makes it a problem. Evidence states the rung it reached on the `prove-it` ladder.
7. **Bugs**: Report a bug you notice with the category `Bugs`, naming its scenario: what goes in, what happens, and what should happen. One whose evidence is below **Walked through** is recommended as **Noted**, so a suspicion doesn't read like a confirmed bug.

Dispatch every dimension that applies in parallel. Re-dispatch a sub-agent once if it fails or returns a malformed envelope. If it fails again, mark its categories `Not assessed` and record that under **Coverage**.

This step is done when every dimension has an envelope or is recorded as not assessed.

## 5. Aggregate the results

Copy each finding through verbatim, then replace the reviewer's **Recommendation** with one verdict: **Act on**, **Consider** or **Noted**, as defined in [finding-format](references/finding-format.md), or **Dismissed**.

**Dismissed** means wrong, out of scope or a duplicate. Keep it as one line with the reason, backed by evidence at **Cited** or better. When findings share a root cause, keep the strongest and dismiss the rest as `duplicate of <title>`.

This step is done when every returned finding has a verdict.

## 6. Score the standards

Apply the [scoring-rubric](references/scoring-rubric.md) to the sub-agents' scores to get the overall score. When a dismissed finding cost a category points, re-score that category from its surviving findings first.

## 7. Settle the spec

When the spec compliance sub-agent ran, report the verdict from its envelope. The spec stays out of the score, because a change can be immaculate and still build the wrong thing.

## 8. Write the review

Save the report to `<work folder>/reviews/YYYY-MM-DD-<slug>.md`, creating the folder if it is missing and confirming before overwriting a file. The project's `AGENTS.md` or `CLAUDE.md` names the work folder; when it names nothing, use `.ai/` at the repository root.

Leave each finding in its reviewer's wording.

<output-format>
# Code Review: <what was reviewed>

> <date and time of the review> · <model slug>

**Scope:** The pinned target and its SHAs.

## Overall: X/100

| Category | Score | Weight |
| --- | --- | --- |
| Security | X | 25% |
| Performance | X | 15% |
| Quality & Maintainability | X | 20% |
| Complexity | X | 10% |
| Style & Standards | X | 10% |
| Architecture & Design | X | 10% |
| Documentation | X | 10% |

**Spec compliance:** Met, Partial or Not met, with one line of reasoning. Or "Not assessed: no spec found." Or "Not assessed: a ticket exists (<ticket paths>), see `complete-ticket`."

**Coverage:** The checks that ran and their results, what the sub-agents couldn't assess, and any dimension recorded as not assessed.

## Summary

Two or three sentences: the state of the code, the safety fact and its rung, and the highest-leverage next step.

## Strengths and Weaknesses

**Strengths**
- Up to five specific things the code does well.

**Weaknesses**
- Up to five patterns that span several findings, each pointing at those findings by number.

## Act on first

- The number and title of each **Act on** finding. Leave the section out when there are none.

## Findings

Each finding not dismissed is its reviewer's block from [finding-format](references/finding-format.md), with **Recommendation** replaced by **Verdict**. Order them Act on, Consider, Noted, keeping each reviewer's order within a verdict, and number them from 1 in that order as `### <n>. <title>`.

## Dismissed

- One line per dismissed finding: what was raised, and why it doesn't apply. Leave the section out when nothing was dismissed.
</output-format>

## 9. Report

Give the user the report's path and the overall score. When the bug hunt didn't run, offer the `bug-hunt` skill on the same target, for a hunt beyond it.
