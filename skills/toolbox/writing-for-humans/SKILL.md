---
name: writing-for-humans
description: Use when writing or tightening a document a human will read, such as a spec, a report, a README or a PR description.
---

# Writing for humans

The pass over prose a person will read. Documents agents read, such as skills and `AGENTS.md`, follow the `writing-for-agents` skill instead. A document both read, such as a README, follows this one.

The standard is the reader's time: every sentence helps the reader decide, act or understand, or goes. Apply each rule as a rewrite where the text breaks it, not as a checklist to report on. A sentence that already follows the rules stays as it is.

## Shape

1. **Lead with what the reader came for**: A report or an answer opens with its conclusion. A README or a how-to opens with what the thing is and who it is for. The reasoning and detail follow, for the readers who want them.
2. **Full sentences**: Brevity comes from cutting redundant sentences, never from dropping the words that carry meaning. Write out what fragments, `A → B → fails` arrow chains and invented abbreviations would compress.
3. **Say it once**: Put the point where it belongs and trust it. A summary that restates the section above it is that section written twice.
4. **Answer, don't narrate**: Cut restatements of the request, accounts of the process, such as "I then examined…", and announcements of what the document is about to say.
5. **Prose for reasoning**: Use headings, lists and tables for what the reader scans, such as steps, options and parallel facts. Keep an argument in sentences, because a chain of reasoning broken into bullets loses its "because".

## Sentences

1. **Cut puffery**: Adjectives that grade the work, such as "robust", "comprehensive" or "seamless", claim what the content should show. Keep the fact, and let it show.
2. **Start at the point**: Cut ramps such as "It's worth noting that" and "Additionally, it should be mentioned".
3. **Cut chatbot phrases**: "Great question", "I hope this helps" and "Feel free to" are small talk, and a document has none.
4. **Commit or attribute**: Replace a hedge, such as "might potentially", with the claim and its evidence, or with a named uncertainty: what is unknown, and what would settle it.
5. **Rewrite each em-dash**: Split the sentence with a period, or use a comma, a colon or parentheses, or drop the aside. Pick what a careful writer would, rather than a comma every time.
6. **Concrete over abstract**: "Leverages advanced capabilities" says nothing. Name the mechanism, the number or the behaviour.

## Kept exact

Identifiers, file paths, commands and quoted output are written in full, exactly as they are. Precision is the one place where length wins.

This pass is done when every sentence helps the reader decide, act or understand, and the facts, numbers and commands say what they said before the pass.
