---
name: code-smells
description: Use when reviewing or refining code.
---

# Code smells

The one list of structural problems this workspace looks for. It serves two moments, and sharing the list means a cleanup pass doesn't leave behind what a review would reopen:

- **Cleaning up**: Fix what you find, rewriting the code without changing its behaviour.
- **Reviewing**: Report what is left, without fixing it.

## Scope

1. **Judgement calls**: A smell is a reason to look closer, not a violation. The code may still be right.
2. **Project standards win**: Documented project standards override anything here.
3. **Tools come first**: Anything a formatter, linter or type checker already enforces is out of scope.
4. **Exposed code counts**: Nearby code that the change shows to be wrong is in scope, even when it predates the work.

## Naming and abstraction

1. **Mysterious name**: A name that doesn't say what the thing is or does. Rename it until it does.
2. **Primitive obsession**: A domain concept carried as a string, number or map. Give it a type, named the way the glossary names it.
3. **Data clumps**: The same few fields or parameters travelling together everywhere. Bundle them into one type.
4. **Speculative generality**: An abstraction, hook or parameter with no present caller. Remove it until something needs it.

## Structure

1. **Duplicated code**: The same logic in more than one place. Extract it once the third copy appears, or sooner when the copies must change together.
2. **Long method**: A function doing several things at several levels of abstraction. Break it up, keeping the tests on its public interface.
3. **Shallow module**: An interface nearly as wide as the implementation behind it, so callers carry the complexity. Imagine deleting it: if the complexity vanishes, fold it into its caller; if it reappears across the callers, it was earning its keep, so deepen it instead.
4. **Feature envy**: A method that keeps reaching into another object's data. Move it to the data.
5. **Middle man**: A class that only delegates. Let callers talk to the real thing.
6. **Message chains**: `a.getB().getC().getD()`. Ask the first object for what you actually want.
7. **Dead code**: A branch, function, parameter or flag that nothing reaches any more, often left behind by a refactor. Check that nothing outside the repository relies on it, such as a public API or a switched-off flag, then delete it, since version control keeps it.

## Change patterns

1. **Divergent change**: One module changed for unrelated reasons. Split it along those reasons.
2. **Shotgun surgery**: One conceptual change forcing edits across many modules. Bring together what belongs together.
3. **Repeated switches**: The same conditional cascade in several places. Replace it with polymorphism or a shared map.
4. **Refused bequest**: A subclass inheriting things it doesn't want. Use composition instead.
