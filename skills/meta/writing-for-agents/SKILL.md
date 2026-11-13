---
name: writing-for-agents
description: Use when writing or editing a document agents will read, such as a skill, AGENTS.md or CLAUDE.md.
---

# Writing for agents

A skill, an `AGENTS.md` or `CLAUDE.md`, and a doc reached through a pointer are packaged differently, but they are written the same way. The levers below make a document **predictable**: the agent takes the same process every run, even when the output differs.

## References

- **Writing or editing a skill**: Read [skill-mechanics.md](references/skill-mechanics.md) for its shape, the frontmatter, the choice of who invokes it, and how it names other skills.
- **A skill that also runs in Codex**: Read [openai-yaml.md](references/openai-yaml.md) for its `agents/openai.yaml`.

## Context pointers

A **context pointer** sits in the agent's context, names material outside it, and says when to reach for that material. A skill's description is one, and so is a line in `AGENTS.md` that names a doc. The pointer's wording decides when the agent reaches the material, and how reliably. When must-have material sits behind a weak pointer, sharpen the wording first, and inline the material only when that fails.

A pointer says what the material is, and lists the **branches** that should trigger it. A branch is a distinct case the document handles, so different runs take different paths through it. An always-loaded pointer costs context on every turn, so prune it harder than the body:

1. **Front-load the leading word**: The pointer is where it does its triggering work.
2. **One trigger per branch**: Synonyms for one branch are that branch written twice. Keep only the branches that are genuinely distinct.
3. **Cut what the body already says**: The pointer triggers, and the body explains.

## The two loads

Every document and pointer spends one of two budgets:

1. **Context load**: The cost to the agent of material that is always loaded, such as an `AGENTS.md` line or a skill description. It spends tokens and attention every turn, whether or not it fires.
2. **Cognitive load**: The cost to the human of knowing which documents exist and when to reach for each. It is the price of human control, so spend it where human judgement matters.

Material behind a pointer costs only the pointer's line. Material with no pointer at all rides entirely on the human remembering it.

## Information hierarchy

A document mixes **steps**, the actions the agent performs in order, with **reference**, the definitions and rules it consults when needed. Each piece sits on a ladder, ranked by how soon the agent needs it:

1. **Steps in the file**: What the agent does, in order.
2. **Reference in the file**: Consulted when needed. A flat set of peers, such as every rule of a review, is fine here.
3. **Reference behind a pointer**: Moved to another file and loaded only when its pointer fires. That file can sit beside the document or anywhere else.

An `AGENTS.md` or `CLAUDE.md` is loaded into every agent's context, so it sits at the top of the ladder whether it should or not. Keep it to navigation pointers and the few rules every task needs.

**Progressive disclosure** moves material down the ladder, so the top stays readable. Branching decides it: inline what every branch needs, and move what only some branches reach behind a pointer. Move too much, and the agent never sees material it needed. Reference left inline in a document with steps buries the steps, and whether the agent follows them becomes a coin flip.

**Co-location** decides what sits together once a piece has its rung: keep a concept's definition, rules and caveats under one heading, so reading one part brings the rest. A grouped document reads like documentation written for the agent, and a scattered one doesn't.

**Sprawl** is a document that is too long, even when every line is live and unique. Attention thins across it. The cure is the ladder: move reference behind pointers, and split by branch or sequence so each path carries only what it needs.

## Steps

End every step on a **completion criterion**, such as "This step is done when every finding has a fix or a reason". It has two properties:

1. **Clarity**: Can the agent tell done from not done? A vague bound, such as "understanding reached", invites **premature completion**: the agent ends the step early, pulled by the steps still ahead. Sharpen the bound first. When it can't be sharpened and you see the rush, split the steps ahead into a hand-off or a sub-agent, because only a real context boundary hides them. Calling another skill inline leaves them in view.
2. **Demand**: How much does it ask for? "Every modified model accounted for" drives more digging than "produce a change list". Demand also binds reference: "every rule applied" gives a flat list of rules a bar for thoroughness.

The strongest criteria are checkable and exhaustive.

Split a run of steps when the steps ahead tempt the agent to rush the one in front of it. Merging two sequences does the opposite: each step now sees more steps ahead.

## Leading words

A **leading word** is a concept the model already knows from pretraining, which the agent thinks with while it runs the document, such as _lesson_, _fog of war_ or _tracer bullets_. Repeated as a word, never as a sentence, it anchors a whole region of behaviour in a few tokens. A word you coin works when you define it, but it recruits no priors, so reach for an existing word first.

It anchors twice:

1. **In the body**: The agent reaches for the same behaviour each time the word appears, and inside a flat list it points attention at a class of thing to look for.
2. **In a pointer**: When the same word lives in your prompts, docs and code, the agent links them and reaches the material more reliably.

Hunt for passages that collapse into one word, such as a triad spelled out in three places, or a pointer spending a sentence on one idea:

- "Fast, deterministic, low-overhead" becomes _tight_, as in a _tight_ loop.
- "A loop you believe in" becomes _red_. A fuzzy gate becomes a state the agent can observe: the loop goes _red_ on the bug, or it doesn't.

**Negation** is the failure beside this lever. A prohibition puts the forbidden behaviour into context and makes it more available, so a ban half-reads as an instruction. State the target behaviour instead, such as "write one-line comments". Keep a prohibition only as a hard guardrail you can't phrase positively, and pair it with the target even then.

## Pruning

1. **Single source of truth**: Keep each meaning in one place, so a change is a one-place edit. **Duplication** costs maintenance and tokens, and inflates a meaning's rank. A leading word repeats a word on purpose, never the meaning. The one place stops at the edge of what installs together: skills install one at a time, so text that several skills need is copied into each, and the copies are recorded so they change together.
2. **The environment is a source too**: `package.json` scripts, config files, the folder layout and `--help` output already say what they say. A document that restates them is a **cache**, and earns its place only when the lookup is expensive. Cache what the agent can't find by looking: the unwritten convention, the reason behind a choice, the gotcha no config confesses.
3. **Relevance**: Each line bears on what the document does. A line loses relevance by never bearing on the task, or by going stale as the code or the world changes. Without pruning, a document gathers **sediment**: stale layers that settle because adding feels safe and removing feels risky.
4. **No-ops**: An instruction the model already follows by default pays load to say nothing. Test each sentence against the default, and delete a failing sentence whole instead of trimming it. The test depends on the model, not the reader, so settle a disagreement by running the document. A leading word too weak to beat the default, such as _be thorough_ for an agent that already is, is a no-op too, and the fix is a stronger word, such as _relentless_.

## Review pass

Before you finish a document, go through it in this order:

1. **Pointer**: The description or pointer line front-loads its leading word, with one trigger per branch.
2. **Ladder**: Every piece sits on the rung its branches call for, and each concept is under one heading.
3. **Steps**: Each step ends on a completion criterion that is clear and demanding.
4. **Words**: Restatements are collapsed into leading words, and each prohibition is either a hard guardrail or rewritten as its target.
5. **Pruning**: Each meaning has one home, and every sentence changes behaviour against the default.

This pass is done when every line has passed the no-op and relevance tests. When a sentence's effect is in doubt, run the document on a real task instead of guessing the default.
