# Porting a skill

Every skill moves from `cc-plugins` to skills-v2 through the same steps, ending in a blind review by three models. The review keeps authorship out of the comparison.

## Steps

1. **Write.** MalasataXD writes the v2 version as `SKILL.md`.
2. **Review.** Compare it with the `cc-plugins` version using `writing-for-agents` and the guide to [prompting Claude Opus 5.5](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5-5). Report where it is better, worse or different.
3. **Draft.** Write the changes to `SKILL.draft.md` beside `SKILL.md`, and leave `SKILL.md` untouched.
4. **Blind review.** Run the round described below.
5. **Apply.** Fix the gaps the review confirmed. A decision made on purpose stands when a reviewer objects without knowing the reason, as with past-tense titles and removing the `unslop` call.
6. **Promote.** When MalasataXD says so, rename the draft to `SKILL.md` and commit it.

## Blind review

1. Copy the three versions into `.blind-test/A`, `B` and `C`, each as `SKILL.md`, along with any reference files. Neutral names hide authorship, which paths like `SKILL.draft.md` and `cc-plugins` would give away.
2. Use a different letter order from the last round.
3. Send the brief below to fresh, separate runs of Opus 5.5, GPT-6.1 Sol at `high` effort and Grok 4.7.
4. Check each report's reasons against the files before counting its ranking. In the `file-pr` round, Grok swapped the labels A and B, and only its reasons showed that.
5. Delete `.blind-test/` and add the round to the results below.

Keep the brief free of the scenarios the draft was built to fix. The `commit` round named unattended runs and PowerShell, which favoured the draft.

```text
Three versions of an agent skill for <what the skill does> are in `<repo path>\.blind-test\`: `A/SKILL.md`, `B/SKILL.md` and `C/SKILL.md`. You don't know who wrote which. Judge only these files, and don't look anywhere else on disk for related material. Other skills they name aren't included, so assume those exist. This is read-only, so don't edit or create anything, and don't run any git or `gh` command that changes state.

Read all three as the agent who would have to follow them on a real <task>, then rank them from best to worst. Judge how reliably each would produce a correct, useful <output> across the different situations you'd expect this skill to meet. Use your own judgement about what matters most.

Reply with exactly this, in at most 200 words:
1. Your ranking, such as "B > A > C", and a one-sentence reason for your winner.
2. The biggest weakness of each version, in one line each.
3. Anything from a lower-ranked version that the winner should take, or "nothing".

Be honest and pick a clear winner. Ties aren't allowed.
```

## Reading the results

A ranking is an opinion formed from reading the text. No model has followed these skills on real work during a round. A gap that two or more models flag is worth fixing, and a gap only one model flags is judged on its merits. Grok runs at its default `high` effort. Sol's default is `low`, so it is set to `high` to match. The `commit`, `file-pr` and `model-attribution` rounds ran Sol at `low`, so its reports in those rounds are shallower.

## Results

| Skill | Date | Ranking from all three models | Notes |
| --- | --- | --- | --- |
| `commit` | 2026-11-02 | Draft, then original, then MalasataXD's version | The brief favoured the draft. Opus and Sol found that already-staged changes can slip into a commit, and that gap is still open. |
| `file-pr` | 2026-11-02 | Draft, then original, then MalasataXD's version | Used the neutral brief. Grok's ranking is corrected for its swapped labels. All three flagged the missing rule for several stacking candidates, which is now fixed. |
| `model-attribution` | 2026-11-03 | Draft, then original, then MalasataXD's version | Letters: A was MalasataXD's version, B the draft and C the original. Sol ran at `low`. No gap was flagged by two models. Grok's point that `gh` writes outside the six listed commands had no rule is now fixed. |
