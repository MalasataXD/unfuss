---
name: grilling
description: Use when the user wants a plan, decision or design stress-tested, or says "grill".
---

# Grilling

Interview the user relentlessly about the plan, decision or design until you both hold the same understanding of it. The questions are the work, so nothing is built, edited or committed until the user confirms the summary in step 4.

Map the plan as a **design tree**: every decision branches into the decisions that hang off it. The **frontier** is every open decision whose prerequisites are settled: the questions answerable now, without guessing at answers you haven't heard yet. Each answer settles a decision and pushes the frontier outward.

A decision belongs on the tree only when its answer changes what gets built. Smaller calls are yours to make, and the summary in step 4 lists them.

## 1. Draw the frontier

Read what the user gave you, and `GLOSSARY.md` or `GLOSSARY-MAP.md` when either exists, then draw the tree and its first frontier.

Facts are yours to find, and decisions are the user's to make. Look up only the facts the frontier's questions need, such as code, files and docs, and leave the rest until a question reaches it.

This step is done when every question on the frontier needs a decision, not a fact.

- **Slow lookups**: Dispatch a subagent and ask the rest of the frontier now. A running lookup is an unsettled prerequisite, so only the questions downstream of it wait.
- **Glossary**: Hold the user to the vocabulary it defines, and offer the `domain-modeling` skill so resolved terms and hard-to-reverse decisions are recorded as you go.

## 2. Ask a round

A round is 3-5 numbered questions from the frontier, each with your recommended answer. It has to fit in the user's head, so pick the questions that unblock the most decisions downstream and hold the rest for later rounds. A question that depends on another one in the same round waits for a later round.

Each recommendation is a real pick with the reason it wins. When a question has options, list only the ones you would defend.

Put a `---` line between questions:

```markdown
❓ **Q1** - **<question title>**: <question body, which can run several paragraphs and list options>

➡️ <your recommended answer and why>

---

❓ **Q2** - **<question title>**: <question body>

➡️ <your recommended answer and why>
```

Then wait for the answers.

## 3. Settle the answers

Fold the answers into the tree and recompute the frontier.

- **"You decide"**: Your recommendation becomes the decision.
- **Skipped question**: It stays on the frontier for the next round.
- **An answer you think is wrong**: Say why once, in the next round, and hold the questions downstream of it until the user replies. Then take their decision.
- **An answer that contradicts an earlier one**: Make the conflict the first question of the next round.
- **An answer that raises a new fact**: Look it up as in step 1 before asking anything that depends on it.

Repeat steps 2 and 3 until the frontier is empty: every branch visited, and nothing left silently assumed. When the user wants to stop earlier, go to step 4.

## 4. Confirm the understanding

Summarize the settled tree: each decision with its answer, grouped by branch, the smaller calls you made yourself, and anything still open with the reason. Ask the user to confirm it.

The session ends when the user confirms. A correction reopens the decision it touches and returns to step 3.
