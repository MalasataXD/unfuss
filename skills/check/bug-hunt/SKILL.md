---
name: bug-hunt
description: Use when a change needs hunting for bugs.
---

# Bug hunt

Find the inputs and states where the code does the wrong thing, judged against what its names, types, tests, docs and callers promise. The hunt reads well beyond the target, so run steps 2-4 in one sub-agent to keep the reading out of the conversation, and relay its report. When you are already a sub-agent, do them yourself.

## 1. Pin the target

Settle what you are checking, such as a ref, a path or the working tree, and what it is compared against. With an empty diff, say so and stop.

## 2. Name the safety fact

Name the change's safety fact, as the `prove-it` skill defines it. When you can't name one, say so in the report, because the change isn't understood yet.

## 3. Hunt

Read past the target into everything it reaches:

1. **Callers**: Every caller of what changed, and what each one assumes about it.
2. **Failure paths**: Errors, timeouts, retries and partial failure, and the state each one leaves behind.
3. **Edges**: Empty, missing, boundary and duplicate inputs, and ordering and concurrency.
4. **Existing data**: Rows, files and messages the old code wrote, and whether the new code still reads them correctly.
5. **Outside consumers**: Public APIs, events, serialized formats and anything else outside the repository that relies on the old behaviour.
6. **Rollback**: Whether the change can be reverted once it has written data.
7. **Tests**: The changed behaviour that no test covers.

This step is done when every changed entry point has been read along with its callers and failure paths.

## 4. Prove each finding

Every finding names its scenario: what goes in, what happens, and what should happen.

A bug is a behavioural claim, so aim for **Ran it** on the `prove-it` ladder: a test or script that shows the failure. Write it outside the working tree, or delete it afterwards, so the working tree ends as it started. When it can't run, state the rung the evidence reached. A finding below **Walked through** is recommended as **Noted**, so a suspicion doesn't read like a confirmed bug.

This step is done when every finding has a scenario and a rung.

## 5. Report

Present the report in the conversation:

1. **Target**: What was checked, and what it was compared against.
2. **Safety fact**: The fact and the rung it reached, or why none could be named.
3. **Findings**: Each one as a finding block from the `review` skill's `references/finding-format.md`, with the category `Bugs` and no score, ordered Act on, Consider, Noted. With none, write "No findings."
4. **Closing line**: What ran, and anything you couldn't read or run.
