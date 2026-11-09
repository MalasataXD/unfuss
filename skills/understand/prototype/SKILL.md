---
name: prototype
description: Use when the user wants to try something out before it's built for real.
---

# Prototype

A prototype is **throwaway code that answers one question**, such as whether a state model holds up, what a page should look like, whether a new way of testing pays off, or whether a library fits. The question decides the shape, and the answer is what you keep.

> A prototype stays local: its branch is never pushed, and never merged. The real implementation is built separately, once the answer is in.

## 1. State the question

Take the question from the user's prompt or the surrounding code, or ask when the user is around. Write it down in one paragraph, somewhere the user sees it when they run the prototype, such as a visible intro or the first lines of its output. A prototype that answers the wrong question is wasted, and a written question can be checked later.

Then pick the shape:

1. **Logic or state**: Does this state model, data shape or set of rules hold up? Read [state-model-demo.md](references/state-model-demo.md).
2. **UI**: What should this look like? Read [ui-variants.md](references/ui-variants.md).
3. **Anything else**: Such as a testing approach, a performance spike or a library trial. Build the smallest runnable thing that answers the question.

When the question is ambiguous and the user isn't around, pick the shape that matches the surrounding code, such as logic for a backend module or UI for a page, and state that assumption beside the question.

This step is done when the question is written down and the shape is picked.

## 2. Set up the branch

Create a worktree on a new local branch, `prototype/<slug>`, placed wherever worktrees normally go in this project. Leave the branch without an upstream. Install what the prototype needs to run there, and copy over local files git doesn't track, such as `.env`.

Building on the real code means a UI variant sits on the real page, and a spike runs against the real modules, while your main working tree stays untouched.

This step is done when the worktree has what the prototype needs to run.

## 3. Build it

Build the smallest thing that answers the question:

1. **Trivial to run**: One command, such as `pnpm <name>` or `python <path>`, or a file the user double-clicks.
2. **No writes to real data**: State lives in memory, and anything that would write to a real database or service goes to a stub. When the question is about persistence, use a scratch database or a local file with a clear "PROTOTYPE: wipe me" name.
3. **No polish**: No abstractions, and no error handling beyond what keeps it running. Tests only when the tests are the question, as in a testing prototype.
4. **Visible state**: After every action or switch, show the full relevant state, so the user can see what changed.
5. **Comparable variants**: When the question compares options, commit each one, so they can be diffed and switched between.

This step is done when the prototype runs with one command.

## 4. Hand it over

Tell the user how to run it and what to look for. The interesting moments are when they say "that shouldn't be possible" or "I want the header from B with the sidebar from C", because those are the real answer. When they ask for another case or variant, add it.

This step is done when the user has an answer to the question.

## 5. Capture the answer

Commit the prototype's final state on its branch, so it stays as the primary source.

Record the answer where the question was raised, such as the ticket or spec it informed: the question, the verdict and why, and the branch name as a pointer. A hard-to-reverse outcome is recorded with the `decision-records` skill.

This step is done when the answer is recorded, and points at the branch.

## 6. Promote it

When the user wants the answer built, it goes into the real code like any other change, through a plan and the `implement` skill. Rebuild it properly rather than merging the branch or copying its code, because the prototype was written without tests or error handling. A pure logic module is the exception: it can be the starting point, with its tests written as it lands.

Leave the worktree and the branch until the user deletes them.
