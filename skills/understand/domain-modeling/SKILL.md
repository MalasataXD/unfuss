---
name: domain-modeling
description: Use when a term is fuzzy or clashes with the domain language.
---

# Domain modeling

One term, one meaning. This skill keeps the project's **ubiquitous language** sharp: the vocabulary the team, the docs and the code all share. The **glossary** is where that language is written down.

## Where the glossary lives

Most repositories have one context and one `GLOSSARY.md` at the root.

A `GLOSSARY-MAP.md` at the root means the repository has several contexts, and the map lists the glossary for each:

```text
/
├── GLOSSARY-MAP.md
└── src/
    ├── ordering/
    │   └── GLOSSARY.md
    └── billing/
        └── GLOSSARY.md
```

In a mapped repository, **the glossary** is the one the map lists for the context you are working in. A term from a context the map doesn't cover gets a new glossary beside the existing ones, and its own row in the map.

Create a glossary with its first resolved term, not before.

## Sharpening the language

Raise each of these the moment it appears, not at the end:

- **Clash with the glossary**: Name both meanings and ask which one holds. "Your glossary defines 'cancellation' as X, but you seem to mean Y. Which is it?"
- **Fuzzy or overloaded term**: Propose one precise term. "You're saying 'account'. Do you mean the Customer or the User?"
- **A term about to be written down**: Invent a concrete scenario on its edge, such as a case between it and a neighbouring term, and ask which term it falls under.
- **Claim about how something works**: Check whether the code agrees, and raise the contradiction when it doesn't. "Your code cancels whole Orders, but you said partial cancellation is possible. Which is right?"

## Capturing it

Write each term into the glossary as soon as it is resolved, in the format in [GLOSSARY-FORMAT.md](references/GLOSSARY-FORMAT.md).

The glossary holds what terms mean, and nothing about how they are built. A decision that comes up along the way belongs to the plan or spec it shapes. When it might be hard to reverse, load `decision-records` and let its test decide whether it earns an ADR.

## Reading it back

When another skill needs the project's vocabulary, the glossary is the source: name things the way it names them.
