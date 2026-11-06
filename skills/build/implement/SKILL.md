---
name: implement
description: Use when building an approved plan or ticket.
---

# Implement

Build approved work by running other skills in order, and keep the tests green between them. `tdd` builds where the code has a seam to test at, `simplify` always tidies, and `review` checks a big change.

The work is already understood and agreed when this starts: a plan the user approved, or one presented with no open questions left, whether it lives in a ticket, a spec or the conversation.

> The go-ahead that started this skill covers the whole run. The only stops along the way are the decisions in [When the plan runs out](#when-the-plan-runs-out), and the points where the plan says the user is needed.

## When the plan runs out

Plans are approved with gaps in them, and a gap can surface at any step. Sort each one by kind:

1. **A fact**: How the existing code behaves, what an interface accepts, or which pattern the project already uses. Finding it is your job: read the code, run the tests or check the docs, then carry on.
2. **A decision**: Anything with more than one defensible answer that the plan didn't settle. That one is the user's.

On a decision, stop before writing code that assumes an answer. Put it to the user: what you hit, the options and your recommendation. Several tangled decisions are a `grilling` session rather than a question. Resume where you stopped once it is settled.

Record what comes back where it belongs. A settled term goes to the `domain-modeling` skill, and a hard-to-reverse choice to the `decision-records` skill. A change to what the plan promised is named in the report, rather than quietly absorbed.

## 1. Read the plan

When the plan and the code it touches are already in context, start building. Otherwise, read the plan first, along with any spec it comes from.

Note the points where the plan says the user is needed, such as a design review or a credential, and bring the user in at each one.

Note the commit you start from, so the build's commits can be named later.

This step is done when you can name what will be built and where the user is needed.

## 2. Build it

Where the code has a seam the project already tests at, such as a service, a handler, a reducer or a pure function, run the `tdd` skill.

Where it has none, such as UI components, styling, wiring, configuration or a rename, build it directly. Verify it the way the project verifies that kind of change, such as the type checker, a lint pass or running the app.

Say in one line which path you took, and why.

This step is done when the build is green.

## 3. Tidy it

Run the `simplify` skill over the code written in this session. It runs every time, because `tdd` leaves refactoring out of its loop.

This step is done when the tests and the type checker are green again.

## 4. Commit it

Run the full suite, not just the affected files, and once it passes, commit the paths the build touched with the `commit` skill. Committing before the review keeps the review's fixes in their own commit.

This step is done when the build is committed.

## 5. Review it when the change is big

Run the `review` skill when any of these hold:

1. **Size**: The change touches more than five files, or crosses a module boundary.
2. **Contracts**: It changes a schema, a public interface or an API contract.
3. **The run**: A decision came up during the build, or the user asked for a review.

Otherwise, skip it with a one-line note. When it runs, fix what it marks **Act on**, and say plainly why any finding is left. Run the full suite again, and commit the fixes on their own.

This step is done when the review ran and its fixes are committed, or the skip is noted.

## 6. Report

Report what was built, the commits it made from the starting commit, the path each part took, and the rung its evidence reached on the `prove-it` ladder. Name any change to what the plan promised.

When the work came from a ticket, continue into `complete-ticket` to judge it against its acceptance criteria.
