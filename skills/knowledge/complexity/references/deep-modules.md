# Deep modules

How to shape a module so it pulls complexity downwards.

## Terms

1. **Module**: Anything with an interface and an implementation, at any scale: a function, a class, a package, or a slice spanning tiers.
2. **Interface**: Everything a caller must know to use the module correctly. That is the type signature, but also invariants, ordering constraints, error modes, required configuration and performance characteristics. It is wider than the `interface` keyword or a class's public methods.
3. **Depth**: The behaviour a caller or test gets per unit of interface it has to learn. A module is **deep** when a lot of behaviour sits behind a small interface, and **shallow** when the interface is nearly as complex as the implementation.

```text
Deep                         Shallow
┌──────────────┐             ┌──────────────────────────┐
│  interface   │             │        interface         │
├──────────────┤             ├──────────────────────────┤
│              │             │  implementation          │
│implementation│             └──────────────────────────┘
│              │
└──────────────┘
```

Depth pays out twice: **leverage** for callers, because one implementation serves every call site and test, and **locality** for maintainers, because change, bugs and verification concentrate in one place.

## Principles

1. **Depth belongs to the interface**: A deep module can be built from small, swappable parts, as long as they stay behind its interface.
2. **The deletion test**: Imagine deleting the module. If the complexity vanishes, it was a pass-through, so fold it into its caller. If the complexity reappears across its callers, it was earning its keep.
3. **Depth is leverage, not a line ratio**: A longer implementation doesn't make a module deeper. Hiding more of what callers would otherwise need to know does.

## Shaping an interface

Ask, in order:

1. Can I reduce the number of entry points?
2. Can I simplify the parameters, or give the common case defaults?
3. Can I hide more inside, such as an ordering constraint, an error the module could handle, or configuration it could derive?

Before settling, sketch a second interface that differs in kind, such as one with fewer entry points or one built around the most common caller, and keep whichever is deeper.

## Deepening moves

1. **Merge a shallow cluster**: Several small modules that are understood and changed together become one module with one interface.
2. **Fold a pass-through into its caller**: When the deletion test says it carries nothing.
3. **Absorb what callers repeat**: Setup, validation or error handling copied across call sites moves behind the interface.

How the deepened module is tested depends on its dependencies, which [seams.md](seams.md) covers.
