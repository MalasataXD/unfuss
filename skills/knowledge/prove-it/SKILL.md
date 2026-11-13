---
name: prove-it
description: Use when judging whether work is done, or backing a claim.
---

# Prove it

The one ladder this workspace uses to grade evidence. It serves two moments, and sharing it means "verified" means the same thing wherever it appears:

- **Reporting**, such as in a review finding: State the rung each claim's evidence reached, and enforce nothing.
- **Enforcing**, such as when judging whether work is done: Require a minimum rung before a claim counts as met.

## The safety fact

Before grading anything, name the one fact the change is safe because of: the claim that, if false, breaks it.

<examples>
<example>The migration is idempotent.</example>
<example>No caller passes null here.</example>
<example>The old endpoint has no remaining consumers.</example>
</examples>

A change whose safety fact can't be named isn't understood yet, so naming it comes before the first rung.

## The ladder

Each rung is the same claim, held with more proof than the one below it:

1. **Asserted**: The claim was stated, and nothing backs it.
2. **Cited**: A specific line, config or doc was pointed at, such as `path/file.ts:42`.
3. **Walked through**: The logic was traced end to end: this input takes this path and produces this result, shown step by step.
4. **Ran it**: A test, script or command was run and its output captured. The output is quoted, not summarized.
5. **Reproduced live**: The real surface was driven, such as the app launched, the endpoint called or the page rendered, and the behaviour observed there.

State the rung by name. "Verified" without a rung is only **Asserted**.

## Which rung is enough

The riskier the claim, the higher the rung it needs:

- **Cited or Walked through**: Enough for a claim the type checker or a cited line settles outright.
- **Ran it**: The minimum for any behavioural claim, that the code does X when Y happens, because a walk-through is where hidden assumptions hide.
- **Reproduced live**: The minimum for a claim only the real surface can settle, such as rendering, an integration or the environment.

A claim below its minimum isn't met, however convincing the reasoning. When the minimum can't be reached, report the rung the claim did reach and what blocked the next one. A rung counts only when its evidence reaches the claim's conditions: a passing test that never exercises the case proves nothing about it.
