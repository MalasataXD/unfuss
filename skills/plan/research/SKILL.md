---
name: research
description: Use when an answer needs real digging, or is worth keeping.
---

# Research

Spin up a **background agent** to do the research, so you can keep working while it researches.

## The agent's job

1. **Sources**: Investigate the question against **primary sources** (official docs, source code, specs, first-party APIs), not a secondary write-up of them. Follow every claim back to the source that owns it. When `<work folder>/research/` already has a file on the same question, start from it and update it instead of writing a second one.
2. **Write it up**: Write the findings into a single Markdown file, citing each claim's source. A claim with no primary source is marked as unverified, with the best source found. When sources disagree, or a source may be out of date, say so and give the version or date each one applies to. This step is done when every claim either cites a primary source or is marked as unverified.
3. **Save it**: Save it to `<work folder>/research/<slug>.md`, with a kebab-case slug from the question, and create the folder if it is missing. The project's `AGENTS.md` or `CLAUDE.md` names the work folder; when it names nothing, use `.ai/` at the repository root.
4. **Report**: Return the path and the answer in two or three lines, naming any unverified claim the answer rests on, so the conversation keeps the conclusion without the file.
