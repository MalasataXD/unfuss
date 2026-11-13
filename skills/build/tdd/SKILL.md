---
name: tdd
description: Use when building or fixing test-first, or when TDD comes up.
---

# Test-driven development

Build in a red-green loop: one failing test, then only the code that makes it pass, one slice at a time. The tests the loop leaves behind are worth keeping only when each one checks behaviour through a public interface, so this skill holds both the loop and what makes a test good.

Before exploring the code, read `GLOSSARY.md` or `GLOSSARY-MAP.md` when either exists, and the `Accepted` ADRs covering the area, so test names and interfaces use the project's language.

## Good tests

A good test checks behaviour through a public interface and reads like a specification: "user can check out with a valid cart" says what capability exists. The code behind it can change entirely, and the test still passes. [tests.md](references/tests.md) has worked examples.

Three kinds of test look like coverage without protecting anything:

1. **Implementation-coupled**: It mocks an internal collaborator, tests a private method, asserts on call counts, or checks through a side channel, such as querying the database instead of using the interface. The tell is a test that breaks under a refactor that changed no behaviour.
2. **Tautological**: The assertion recomputes the expected value the way the code does, such as `expect(add(a, b)).toBe(a + b)`, so it can never disagree with the code. Take expected values from an independent source: a known-good literal, a worked example or the spec.
3. **Horizontal**: All the tests are written first, then all the code. Bulk tests check imagined behaviour, test the shape of things rather than what callers see, and lock in a structure before the implementation is understood.

Mock only at system boundaries, such as third-party APIs, time and randomness. For a database or a filesystem, prefer a local stand-in, such as a test database. Run code you own for real.

## 1. Agree the seams

A seam is where a module's interface lives: the place a test observes behaviour without reaching inside. Not everything can be tested, and agreeing the seams first puts the effort on the critical paths and the complex logic, instead of on every edge case.

Write down the seams you plan to test, and confirm them with the user: "What's the public interface, and which seams should we test?" A plan or ticket that already names them counts as confirmed.

Prefer an existing seam, and take the highest one that still observes the behaviour. When you need a new one, design it with the `complexity` skill.

> Write tests only at confirmed seams.

This step is done when every seam you will test is written down and confirmed.

## 2. Red

Write one failing test at one seam. For a bug, the test reproduces it. Run it, and check that it fails because the behaviour is missing, as its failure message shows, rather than because of a typo or broken setup.

Red means the test fails against the code as it stood before your change. When a test arrives after its fix, prove red on the parent commit in a separate worktree.

> Keep finished code intact in the shared working tree. A deliberate break reaches the dev server and every other agent working there, and an interrupted run leaves it behind.

When no seam can reach the behaviour without scaffolding outside the ticket's scope, an executable substitute is allowed, such as a script or a command with its output captured. The report says why a test was impractical, and "it was faster" never qualifies.

This step is done when the failing run is quoted and shows the missing behaviour.

## 3. Green

Write only enough code to pass the test in front of you, and run the tests again. Build nothing for tests that haven't been written yet.

This step is done when the new test and every test before it pass, and the run is quoted.

## 4. Repeat

Go back to step 2 for the next slice: one seam, one test and one minimal implementation per cycle. Let what the last cycle taught you shape the next test.

Stop at green, and leave refactoring out of the loop.

This step is done when every agreed behaviour has a passing test.

## 5. Report

Report each cycle with its real output: the failing run before the code, and the passing run after it. Quoted output is what **Ran it** means on the `prove-it` ladder, and "tests pass" without it is only **Asserted**. Name any substitute for a test, and why a test was impractical.

Say that the code is as the loop left it, with no refactoring yet.
