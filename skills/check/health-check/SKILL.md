---
name: health-check
description: Use when the user asks for a health check of the codebase.
disable-model-invocation: true
---

# Health check

Find where reducing complexity would pay off, and report every finding, ranked by evidence from the codebase's history. The history decides where to look, so the check stays affordable: code that changes often is where complexity costs the most.

> Suggest only: refactor nothing. The report is the only file you write.

## 1. Settle the scope

1. **The hot spots**: The default. The history picks the areas with the strongest signal, and only those are read.
2. **A named area**: When the user names a module or folder, such as "health check `src/billing`". Every file in it is read, whatever its history.

This step is done when you can name the scope.

## 2. Load the lenses

Load the `complexity` and `code-smells` skills. Read `GLOSSARY.md` or `GLOSSARY-MAP.md` when either exists, and the `Accepted` ADRs, so the findings use the project's names and don't re-suggest what an ADR settled.

## 3. Find the hot spots

Give the history to a sub-agent: the last few hundred commits or the last few months of `git log --name-only`, whichever is shorter. Have it return the areas with the strongest signal and their evidence, not the log. When you are already a sub-agent, read it yourself.

1. **Churn**: The files that change most often.
2. **Co-change**: Files that keep changing in the same commits. This is change amplification in the wild.
3. **Fix clusters**: Fix commits concentrated in one area.

For the hot spots, keep the handful of areas with the strongest signal. For a named area, the history only supplies the evidence for its findings. When the history is too short to show a signal, say so, and ask the user to name an area.

This step is done when each area to read is listed with its evidence.

## 4. Read each area

Give each area to its own sub-agent, or a few small areas to one, and run them in parallel, so the reading stays out of this conversation. Give each one the co-change evidence that reaches outside its area, so it can follow a concept across the border. Each sub-agent loads the `complexity` and `code-smells` skills itself, reads every file in its area through the lenses below, and returns every finding in the shape from step 5 without **Evidence**, or the word "Clean". When you are already a sub-agent, read the areas yourself, one at a time.

1. **Shallow modules**: Apply the deletion test from the `complexity` skill. Does the module concentrate complexity, or only move it?
2. **Scattered concepts**: One concept spread across modules that always change together.
3. **Unreachable behaviour**: Logic no seam can reach without mocking internals or testing past the interface.
4. **Smells**: The structural entries in the `code-smells` baseline.

Then gather the findings yourself:

1. **Merge duplicates**: A scattered concept turns up in several areas, so merge it into one finding that names all of them.
2. **Attach the evidence**: Add each finding's history evidence from step 3.
3. **Check the ADRs**: A finding that goes against an ADR stays only when the friction is real enough to reopen the decision, and it names the ADR.

This step is done when every area has returned its findings or "Clean", and the findings are merged.

## 5. Write the report

Rank the findings by strength first, then by their history evidence. Save the report to `<work folder>/health-checks/YYYY-MM-DD.md`, or `YYYY-MM-DD-<area>.md` for a named area, creating the folder when it is missing and confirming before overwriting a file. The project's `AGENTS.md` or `CLAUDE.md` names the work folder, and when it names none, use `.ai/` at the repository root.

```markdown
# Health check: <scope>, <date>

**Coverage:** The hot spots / <area>, with the areas read.

## Summary

The number of findings by strength, and the one you would tackle first, and why.

## Findings

### <n>. <title: name the problem, not the lens>

**Strength:** Strong / Worth exploring
**Where:** The modules involved, named as the glossary names them, and their files.
**Evidence:** The churn, co-change or fix commits behind it.
**Problem:** One sentence naming the symptom: change amplification, cognitive load or unknown unknowns.
**Move:** One sentence: deepen it, fold it into its caller, or move the seam.

## Speculative

One line each, for the findings too uncertain to rank: the area and the problem.

## Clean

The areas read that produced no findings.
```

This step is done when the report is saved. Then give the user its path, the number of findings by strength and the one you would tackle first, and offer the `to-spec` skill on it.
