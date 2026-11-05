# TypeScript and React

Apply these on top of the principles in the skill. The project's own standards still win.

## TypeScript

1. **ES modules**: Sorted imports, with explicit file extensions.
2. **The `function` keyword**: For top-level and named declarations. Arrow functions are fine for short inline callbacks.
3. **Return types**: Annotate exported and top-level functions, and let inference handle locals.
4. **Errors without `try`/`catch`**: Prefer a cleaner path, such as result types, narrow guards or validation at the boundary. Keep `try`/`catch` where it belongs.
5. **No `any`**: Use a real type, or `unknown` with narrowing.
6. **Plain branching**: `switch` or `if`/`else` chains for several conditions, never nested ternaries.
7. **Naming**: `camelCase` for values, and `PascalCase` for types and components.

## React

1. **A `Props` type**: Every non-trivial component gets a named one, instead of an inline anonymous shape.
2. **Focused components**: Split one that juggles too many concerns.
3. **Derived state**: Derive it instead of duplicating it, and lift it only when it's genuinely shared.
4. **Honest effects**: Correct dependency arrays, and no logic that belongs in render or an event handler.
5. **Hooks for repeats**: Extract a hook or helper when the same logic repeats, but leave a one-off inline.
