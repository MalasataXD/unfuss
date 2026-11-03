# Seams

Where a module's interface lives, and how the module is tested there.

## Terms

1. **Seam** (Michael Feathers): A place where behaviour can be altered without editing in that place. It is where a module's interface lives, and where its tests sit. Where the seam goes is a separate decision from what goes behind it.
2. **Adapter**: A concrete thing that satisfies an interface at a seam, such as a Postgres repository, an in-memory fake or an HTTP client. The word names the role, not the size.

## Principles

1. **The interface is the test surface**: Callers and tests cross the same seam. A test that has to reach past the interface means the module is the wrong shape.
2. **Two adapters make a real seam**: Introduce a seam only when something actually varies across it, typically production and test. A seam with one adapter is just indirection.
3. **Internal seams stay internal**: A module may keep seams inside its implementation for its own tests. Its interface stays the same whether a test uses them or not.
4. **Prefer existing seams**: Use the highest existing seam that still observes the behaviour.

## Dependency categories

The category of a dependency decides how the module is tested across its seam.

1. **In-process**: Pure computation or in-memory state. Test through the interface directly, with no adapter.
2. **Local-substitutable**: Has a local stand-in, such as PGLite for Postgres or an in-memory filesystem. Run the stand-in in the test suite, and keep the seam internal.
3. **Remote but owned**: Your own services across a network. Define a port at the seam. Production gets an HTTP, gRPC or queue adapter, and tests get an in-memory one.
4. **True external**: Third-party services, such as Stripe or Twilio. Inject them as a port, and give tests a mock adapter.

## Designing for testability

1. **Accept dependencies instead of constructing them**: `processOrder(order, paymentGateway)` can be tested. A `processOrder(order)` that creates its own `StripeGateway` can't.
2. **Return results instead of mutating**: `calculateDiscount(cart): Discount` can be asserted on. `applyDiscount(cart): void` has to be inspected from the side.
3. **Keep the surface small**: Fewer entry points mean fewer tests, and fewer parameters mean simpler setup.

## Replace, don't layer

When a cluster is deepened, write tests at the new interface and delete the old tests on the shallow modules it absorbed. Tests that assert on observable outcomes through the interface survive internal refactors. A test that has to change when only the implementation changed is testing past the interface.
