---
name: complexity
description: Use when shaping a plan, a module's interface or a seam, or when something feels like a shortcut or special case.
---

# Complexity

**Complexity is anything about a system's structure that makes it hard to understand or modify.** It is the cost of change, not size or sophistication: a clever 50-line solution can be more complex than an obvious 200-line one. This skill is the vocabulary for spending complexity on purpose, and the ledger for checking a plan against it.

## References

- **Proposing a module, shaping its interface, or judging whether one is shallow**: Read [deep-modules.md](references/deep-modules.md).
- **Placing a seam, judging whether one is justified, or testing a module across its dependencies**: Read [seams.md](references/seams.md).

## Symptoms

How complexity is felt by whoever works on the system next:

1. **Change amplification**: One conceptual change forces edits in many places. In existing code, it shows up as shotgun surgery.
2. **Cognitive load**: How much a developer must hold in their head to finish a task correctly. A shorter solution that demands more knowledge is the more complex one.
3. **Unknown unknowns**: It isn't obvious which code must change, or what you must know to change it safely, until the bug report arrives. This is the worst of the three.

## Causes

Every symptom traces back to one of two causes. Name the cause, rather than an umbrella term such as "technical debt".

1. **Dependencies**: Code that can't be understood or changed in isolation, because other code relies on it or shapes it. Dependencies can't be eliminated, only made fewer, simpler and more obvious.
2. **Obscurity**: Important information that isn't evident from the code, such as an unstated invariant, a unit hidden in an int, or a name that reveals nothing. It is the cheaper cause to fix, and the easier one to plan away.

## Mindsets

- **Tactical programming**: Optimizing for the next milestone with the shortcut, the special case, "just this once". Each step is small, and their sum is where complexity comes from.
- **Strategic programming**: Optimizing for a structure that keeps future change cheap, and treating working code as not enough. The investment is small and continuous, not a big redesign.

## Principles

1. **Complexity is incremental**: A system becomes complex through hundreds of small decisions, not one big one, so "it's only a little hack" is no argument. The ledger applies to small things too.
2. **The reader decides**: Complexity is what someone arriving cold experiences. The author's familiarity is no evidence of simplicity.
3. **Pull complexity downwards**: When complexity is unavoidable, the implementer absorbs it, not the caller. A simple interface over a hard implementation beats the reverse.
4. **Somewhat general-purpose**: The interface serves more than today's exact need, while the implementation does only what today needs. This guards against both special cases and speculative generality.

## The ledger

Run every element a plan adds through these questions before the plan is approved: each new module, dependency, config option and special case.

1. **What does it cost?** The dependencies it creates, and what will be obscure to a cold reader.
2. **Which symptom will the cost show up as?** Change amplification, cognitive load or unknown unknowns.
3. **What pays for it?** A benefit named in the plan, not "we might need it".
4. **Can the same benefit be bought cheaper?** Fewer dependencies, less obscurity, or complexity pulled downwards.

An element that can't answer question 3 is tactical programming. Cut it, or say in the plan that the shortcut is deliberate.

The ledger is the test behind "keep it simple": a simpler option is one that comes out cheaper on these four questions.

The ledger is done when every element has answered all four questions, been cut, or been marked as deliberate.
