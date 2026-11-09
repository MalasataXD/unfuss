# Glossary format

A glossary records the ubiquitous language of one context: what each term means, in the words the domain uses. Code, data shapes, APIs, file paths, decisions and their rationale live elsewhere.

## Header

Open with the context the glossary covers:

```markdown
# Glossary: Ordering Context

The shared language for the ordering context. Each term below means exactly
one thing here; the same word may mean something different in another context.
```

## Entries

One `##` heading per term, in alphabetical order. Terms are domain nouns, cased the way the team says them, such as `Order` or `Line Item`.

```markdown
## Order

A customer's confirmed request to purchase one or more items. An Order exists
only after checkout completes; before that it is a Cart.

- **Synonyms:** Purchase (deprecated; prefer "Order")
- **Not to be confused with:** Cart (pre-checkout), Shipment (fulfilment unit)
- **States:** Placed → Paid → Fulfilled → Closed; may be Cancelled before Fulfilled
```

1. **Definition**: One or two present-tense sentences in domain language. This is the only required part.
2. **Synonyms** (*Optional*): Other words the team uses for the concept, with the canonical one marked.
3. **Not to be confused with** (*Optional*): Sibling terms people mix up with this one.
4. **States** (*Optional*): The lifecycle, when the term has one that matters.

## Rules

- **One meaning per term**: A word with two meanings is two terms. Split and rename until each one is unambiguous.
- **Cross-links**: Name related terms as they appear in the glossary, so a reader can follow the language.
- **Edits in place**: A sharper definition replaces the old one. The glossary describes the language as it is now.
