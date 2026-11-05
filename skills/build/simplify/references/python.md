# Python

Apply these on top of the principles in the skill. The project's own standards still win. These are opinions about clarity, not a restatement of PEP 8.

1. **Guard clauses**: Return early on the invalid or edge case, instead of wrapping the happy path in an `if`.
2. **One-level comprehensions**: A single `[x for x in xs if ...]` is clear. A nested or multi-`for` comprehension becomes an explicit loop.
3. **Type hints**: On public functions and dataclass fields, and not on obvious locals.
4. **Errors by context**: EAFP (`try`/`except`) for I/O and genuinely exceptional cases, and LBYL (check first) for cheap, predictable conditions. Catch the specific exception, never a bare `except:`, and let errors surface rather than swallowing them to make the code look cleaner.
5. **Named data**: A `dataclass` or `NamedTuple` instead of tuples or dicts whose fields carry meaning.
6. **f-strings**: Instead of `%`, `.format()` or concatenation.
7. **Plain over clever**: A named loop or helper instead of walrus chains, dense lambdas or `reduce`.
8. **Plain branching**: `match` or `if`/`elif` chains for several conditions, with no stacked ternaries.
