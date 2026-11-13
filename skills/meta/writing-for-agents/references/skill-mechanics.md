# Skill mechanics

What changes when the document is a skill: its shape, the frontmatter, who invokes it, how it names other skills, and router skills. Everything else about the writing is in `SKILL.md`.

## Shape

1. **Frontmatter**: `name`, a `description`, and `disable-model-invocation: true` when only the user invokes it.
2. **Title and purpose**: A `#` title, then one paragraph on what the skill does and why.
3. **Hard rule**: When the skill has one, a `>` callout under the purpose, for the guardrail the agent must hold throughout, such as "Suggest only."
4. **References**: A list of `**When this happens**: Read [file](references/file.md)`, one entry per branch that needs the file.
5. **Steps**: When the skill is a procedure, numbered `## 1.` headings. Each ends on "This step is done when …".
6. **Items**: Numbered `**Label**: text` items, so a rule can be named and quoted.
7. **Output**: When the skill produces a report or file, its shape as a fenced block filled with placeholders, instead of a description of it.

A skill that is all reference, such as a vocabulary, has no steps. It keeps the purpose paragraph and the references list.

## Invocation

Two choices, trading the two loads:

1. **Model-invoked**: The skill keeps a description, so the agent can fire it on its own, and other skills can name it. The user can still type its name. The description is the skill's top-level context pointer, always loaded, so write it by the pointer rules in `SKILL.md`: start with "Use when", and list the trigger branches. A model-invoked skill that is all reference can also be the home for reference that several skills need, since each of them can name it.
2. **User-invoked**: Set `disable-model-invocation: true`. Only the user typing its name can invoke it, and no other skill can. It costs no context, but the user has to remember it exists. Its description is a one-line "Use when the user asks for …", for the human reading the skill list. Codex ignores this frontmatter, so a skill that also runs in Codex needs an `agents/openai.yaml` as well: read [openai-yaml.md](openai-yaml.md).

Pick model invocation only when the agent or another skill must reach the skill on its own. A skill that only ever fires by hand is user-invoked, and costs nothing until then.

Split off a model-invoked skill when it has a distinct leading word that should trigger it alone, one you actually use in your prompts, or when another skill must reach it. Its description adds context load for good, so that reach has to be worth it.

## Naming other skills

1. **Name the skill, never a file inside it**: The file may not be installed, and it moves whenever the other skill is reorganized.
2. **Shared text has copies**: When several skills need the same text and none can name a reference skill for it, each keeps its own copy. Record where the copies are, so they change together.
3. **Sub-agents**: When a step hands work to a sub-agent, say what it returns, such as "the findings, not the log". Add the fallback "When you are already a sub-agent, do it yourself", because a sub-agent often can't start another.

## Router skills

When user-invoked skills multiply past what the user can remember, a **router skill** cures the cognitive load: one user-invoked skill that names the others and when to reach for each. It can only point, never fire them, because nothing but the user can invoke a user-invoked skill.
