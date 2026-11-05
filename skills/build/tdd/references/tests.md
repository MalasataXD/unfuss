# Good and bad tests

Worked examples for the skill's "Good tests" section. They are in TypeScript, and the same shapes hold in any language.

## Through the interface

```typescript
// Good: checks behaviour a caller sees
test("user can check out with a valid cart", async () => {
  const cart = createCart();
  cart.add(product);
  const result = await checkout(cart, paymentMethod);
  expect(result.status).toBe("confirmed");
});
```

It uses only the public API, makes one logical assertion, and its name says what happens rather than how.

## Coupled to the implementation

```typescript
// Bad: checks how the code works, not what it does
test("checkout calls paymentService.process", async () => {
  const mockPayment = jest.mock(paymentService);
  await checkout(cart, payment);
  expect(mockPayment.process).toHaveBeenCalledWith(cart.total);
});
```

It mocks a collaborator the project owns and asserts on a call, so it breaks when the payment code is restructured, even when checkout still works.

## Through a side channel

```typescript
// Bad: goes around the interface to check the result
test("createUser saves to database", async () => {
  await createUser({ name: "Alice" });
  const row = await db.query("SELECT * FROM users WHERE name = ?", ["Alice"]);
  expect(row).toBeDefined();
});

// Good: checks the result through the interface
test("createUser makes the user retrievable", async () => {
  const user = await createUser({ name: "Alice" });
  const retrieved = await getUser(user.id);
  expect(retrieved.name).toBe("Alice");
});
```

## Mocking a boundary

A boundary is easy to mock when its dependency is passed in, and when each outside operation has its own function:

```typescript
// Easy to mock: the client is passed in
function processPayment(order, paymentClient) {
  return paymentClient.charge(order.total);
}

// Hard to mock: the client is built inside, so a test can't swap it out
function processPayment(order) {
  const client = new StripeClient(process.env.STRIPE_KEY);
  return client.charge(order.total);
}

// Each operation is its own function, so each mock returns one shape
const api = {
  getUser: (id) => fetch(`/users/${id}`),
  getOrders: (userId) => fetch(`/users/${userId}/orders`),
  createOrder: (data) => fetch("/orders", { method: "POST", body: data }),
};
```

A single generic `fetch(endpoint, options)` forces conditional logic into the mock, and hides which endpoints a test exercises.
