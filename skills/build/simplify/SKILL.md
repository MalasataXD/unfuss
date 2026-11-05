---
name: simplify
description: Use when simplifying or cleaning up code, or applying project standards to it.
---

# Simplify

Make code clearer, more consistent and easier to change, without changing what it does. Readable, explicit code beats compact cleverness, and that balance is the point of this skill.

> Behaviour stays exactly as it was: every feature, output, side effect and public signature. When a cleanup would change any of them, leave it out and raise it as a question instead.

## 1. Settle the scope

By default, the scope is the code you wrote or changed in this session. When this session changed nothing, take the working tree's changes from `git status` and `git diff`, and name them in the report. A wider scope, such as a named file, a directory or "the whole module", needs the user to ask for it.

Run the project's tests and type checker once, so step 5 has a baseline to compare against.

This step is done when you can name the files and sections in scope, and the baseline is recorded. When you can't name the scope, ask the user to point at the target, rather than rewriting code nobody changed.

## 2. Gather the standards

Standards apply in three layers, highest precedence first:

1. **The project's own**: `AGENTS.md` or `CLAUDE.md`, `CODING_STANDARDS.md`, `CONTRIBUTING.md`, style guides, and the conventions of the surrounding code. They override the layers below.
2. **The language reference**: The matching file in `references/`, when one exists: [typescript-react.md](references/typescript-react.md), [python.md](references/python.md) or [csharp.md](references/csharp.md).
3. **The principles**: The clarity rules in step 3.

Match the surrounding code's idioms instead of bringing your own.

## 3. Refine

Make the edits directly, rather than describing them.

**Clarity**

1. **Flatten**: Reduce nesting and unneeded complexity.
2. **Remove**: Delete redundant code and abstractions.
3. **Name**: Rename until the intent is obvious.
4. **Group**: Bring related logic together.
5. **Branch plainly**: Use `switch` or `if`/`else` chains for several conditions, never nested ternaries.

**Structure**

For structural changes, such as extracting duplication, deepening a shallow module or moving logic to the data it uses, work from the `code-smells` skill. `review` judges against the same list, so a smell fixed here isn't reopened there.

**Comments**

Treat an inline comment as evidence about the code beneath it, and sort it by what it explains:

1. **How the code works, or why this shape is good**: The comment is standing in for code that isn't clear enough. Rewrite the code until the comment is unnecessary, then delete it.
2. **Why an outside constraint exists**: Such as an upstream bug being worked around, a regulatory rule, or a measured reason for an odd shape. No refactor can recover that knowledge, so keep the comment, and sharpen it if it's vague.

Documentation comments, such as JSDoc, TSDoc and docstrings, describe a public interface for its callers, and stay. A "why" that is hard to reverse, surprising and a real trade-off has outgrown a comment: offer the `decision-records` skill.

## 4. Check the balance

Keep a change only when the result is simpler to read and change, not just shorter. Undo any change that:

1. **Hides intent**: A clever solution that is hard to follow, or a dense one-liner.
2. **Crowds a unit**: Too many concerns crammed into one function or component.
3. **Removes structure that helps**: An abstraction that organises the code, even if it could be inlined.
4. **Makes it harder to debug or extend**.

## 5. Verify and report

Run the project's tests and type checker, and compare them with the state before your edits. A failure your edits caused means a behaviour changed, so undo or fix that edit.

Then check each edit the tests don't cover: its signatures, outputs and side effects stay the same.

This step is done when the checks are as green as they were before you started. Then report briefly:

1. **Changes**: Any change that affects how the code is understood, such as a rename, a moved responsibility or a deleted abstraction.
2. **Left out**: Any cleanup you held back because it would change behaviour, as a question for the user.
