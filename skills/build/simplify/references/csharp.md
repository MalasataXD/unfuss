# C# and .NET

Apply these on top of the principles in the skill. The project's own standards still win. These are opinions about clarity, not a restatement of the standard C# conventions.

1. **Guard clauses**: Validate and return or throw early, and keep the happy path unindented.
2. **Readable LINQ**: A short, single-purpose chain is fine. Break a long or multi-stage pipeline into named steps, or use a `foreach` when it reads better. Keep LINQ shallow and free of side effects.
3. **Honest nullability**: Enable nullable reference types and model intent in the signatures. The null-forgiving `!` needs a justification right where it's used.
4. **Async done right**: No `async` over sync, and no `.Result`, `.Wait()` or `.GetAwaiter().GetResult()`. Pass `CancellationToken`s through, and follow the project's `ConfigureAwait` stance.
5. **Pattern matching**: `switch` expressions for multi-branch logic, never nested ternaries.
6. **Expression-bodied members**: Only when the body stays trivially readable. Anything with real logic gets a block body.
7. **`var` when the type is obvious**: From the right-hand side, such as `new()`, a cast or a clear factory call. Otherwise, write the type.
8. **Modern features that cut ceremony**: `record` or `record struct` for data-carrying types, instead of boilerplate classes or tuples whose elements carry meaning. Primary constructors to trim constructor noise, and collection expressions (`[..]`) and target-typed `new` where they read cleanly. Follow the project on namespace style.
9. **Naming**: `PascalCase` for members and types, and `_camelCase` for private fields, or the project's existing convention.
