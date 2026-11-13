---
name: diagnosing-bugs
description: Use when something is broken or slow for no obvious reason.
---

# Diagnosing bugs

A discipline for bugs whose cause isn't obvious. Each step ends at a done condition, and a step is skipped only with a stated reason.

Before exploring the code, read `GLOSSARY.md` or `GLOSSARY-MAP.md` when either exists, and the `Accepted` ADRs covering the area, so your model of the modules matches the project's.

## Redact

This skill has you show commands, outputs and captured artifacts. Redact every secret first, such as API keys, tokens, passwords, cookies, session IDs, connection strings and signed URLs, and write `<REDACTED>` in its place.

1. **Show a command's shape, not its credential**: `curl -H "Authorization: Bearer $API_TOKEN" …`. The variable reference is the redacted form, and it still runs.
2. **Build loops against environment variables**: The credential stays in the environment, out of the files you write and the output you quote.
3. **Redact captured artifacts**: HAR files, log dumps and request payloads carry auth headers. Quote only the lines that carry the signal.

When the redacted output isn't enough to diagnose the bug, say so and ask the user.

## 1. Build a feedback loop

This step is the skill. With a pass/fail signal that goes red on this bug, you will find the cause, because bisection, hypotheses and instrumentation all just consume that signal. Without one, reading code won't find it. Spend most of your effort here, and try another kind of loop before giving up on the first.

Ways to build one, roughly in this order:

1. **Failing test**: At whatever seam reaches the bug, whether unit, integration or end-to-end.
2. **HTTP script**: `curl` or a script against a running dev server.
3. **CLI invocation**: A fixture input, with stdout diffed against a known-good snapshot.
4. **Headless browser script**: Playwright or Puppeteer driving the UI, asserting on the DOM, console or network.
5. **Replayed trace**: A real request, payload or event log saved to disk and replayed through the code path in isolation.
6. **Throwaway harness**: A minimal subset of the system, such as one service with its dependencies mocked, that reaches the bug's code path with a single call.
7. **Property or fuzz loop**: For "sometimes wrong output", run a thousand random inputs and look for the failure.
8. **Bisection harness**: When the bug appeared between two known states, such as commits, datasets or versions, automate "set up state X, check, repeat" so `git bisect run` can drive it.
9. **Differential loop**: The same input through the old and new version, or two configs, with the outputs diffed.
10. **Human in the loop**: The last resort, when a person must click. Give one instruction per step and ask one question per answer you need, so their replies come back in a form you can use.

Once you have a loop, tighten it:

1. **Faster**: Cache setup, skip unrelated initialisation, and narrow the test scope.
2. **Sharper**: Assert on the specific symptom, not on "didn't crash".
3. **Deterministic**: Pin the time, seed the random number generator, and isolate the filesystem and network.

A flaky 30-second loop is barely better than none, and a deterministic 2-second one makes the rest of the work fast.

For a **non-deterministic bug**, aim for a higher reproduction rate instead of a clean repro. Loop the trigger 50 times, run it in parallel, add stress, narrow the timing window, or inject sleeps. A bug that shows up half the time can be debugged, and one that shows up once in a hundred can't, so keep raising the rate.

> When you can't build a loop, stop and say so, listing what you tried. Ask the user for access to an environment that reproduces it, a redacted captured artifact (such as a HAR file, log dump, core dump or timestamped screen recording), or permission to add temporary production instrumentation. Hypotheses wait until a loop exists.

This step is done when you can name one command, such as a script, a test invocation or a `curl`, that you have already run at least once, with its invocation and redacted output shown, and that is:

1. **Red-capable**: It drives the bug's real code path and asserts the user's exact symptom, so it goes red on this bug and green once it's fixed.
2. **Deterministic**: It gives the same verdict every run, or a pinned, high reproduction rate for a flaky bug.
3. **Fast**: It runs in seconds, not minutes.
4. **Agent-runnable**: You can run it unattended, with a person in the loop only as the structured last resort.

Reading code to build a theory before this command exists is the failure this skill prevents, so the theories wait for step 3.

## 2. Reproduce and minimise

Run the loop and watch it go red. Confirm three things:

1. **The user's bug**: It is the failure the user described, not a different one nearby. The wrong bug leads to the wrong fix.
2. **Repeatable**: It fails across several runs, or at a rate high enough to debug against.
3. **Captured**: The exact symptom, such as the error message, wrong output or timing, is recorded, so step 5 can check the fix against it.

Then shrink the repro to the smallest scenario that still goes red. Cut inputs, callers, config, data and steps one at a time, re-running the loop after each cut. A minimal repro leaves fewer parts to suspect in step 3, and becomes the regression test in step 5.

This step is done when every remaining element is load-bearing: removing any one of them turns the loop green.

## 3. Hypothesise

Write three to five ranked hypotheses before testing any of them, because a single hypothesis anchors on the first plausible idea.

Each one is falsifiable, so it states the prediction it makes:

<example>
If the cache key ignores the locale, then clearing the cache between requests makes the bug disappear, and requesting two locales back to back makes it worse.
</example>

A hypothesis with no prediction gets sharpened or dropped.

Show the ranked list to the user before testing. They often know something that re-ranks it, such as a recent deploy, or a cause they've already ruled out. Carry on with your ranking when nobody is there to answer.

This step is done when the list is ranked, and every hypothesis on it has a prediction.

## 4. Instrument

Each probe tests one prediction from step 3, and changes one variable at a time.

1. **Debugger or REPL first**: When the environment supports it, one breakpoint beats ten logs.
2. **Targeted logs next**: Log only at the boundaries that tell the hypotheses apart, rather than logging everything and searching the output.
3. **Tag every debug log**: Give each one a unique prefix, such as `[DEBUG-a4f2]`, so cleanup is a single search.

For a **performance regression**, measure instead of logging. Take a baseline with a timing harness, a profiler or a query plan, then bisect. The fix comes after the measurement.

This step is done when one hypothesis is confirmed by its prediction, and the others are ruled out or left ranked below it.

## 5. Fix and lock it down

Write the regression test before the fix, at a **correct seam**: one where the test exercises the real bug pattern as it occurs at the call site, as the `complexity` skill's seams describe. Name the seam and confirm it with the user before writing the test. When nobody is there to answer, pick the seam and name it in your report.

A seam that's too shallow, such as a single-caller test for a bug that needs several callers, gives false confidence. When no correct seam exists, the missing seam is itself a finding for step 7, and the fix goes in without a regression test.

With a correct seam:

1. Turn the minimised repro into a failing test at that seam, and watch it fail.
2. Apply the fix, and watch the test pass.
3. Re-run the step 1 loop against the original, un-minimised scenario.

This step is done when the original loop is green, and the regression test passes or the missing seam is recorded.

## 6. Clean up

1. **Instrumentation**: Search for the debug prefix and remove every hit.
2. **Throwaway code**: Delete harnesses and prototypes, or move them to a clearly marked debug location.
3. **The confirmed hypothesis**: State it in the commit or PR message, so the next person debugging this area learns from it.

This step is done when the search for the prefix comes back empty, no throwaway code is left outside a marked debug location, and the confirmed hypothesis is in the commit or PR message.

## 7. Ask what would have prevented it

Make the recommendation now that the fix is in, because you know more than when you started:

1. **A structural problem**, such as a missing seam, tangled callers or hidden coupling: Report it against the `code-smells` baseline, or offer the `review` skill over the area when it's worth a full pass.
2. **A decision worth recording**: When the fix encodes a trade-off a future reader would question, offer the `decision-records` skill.

## After the fix

A bug fix is code like any other: tidy it with `simplify`, check it with `review`, and commit it with `commit`. When the fix belongs to a ticket, `complete-ticket` judges it and gates the commit instead. This skill ends at a diagnosed and fixed bug, not a merged one.
