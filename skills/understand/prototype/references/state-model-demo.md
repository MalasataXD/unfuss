# State model demo

A single, self-contained HTML file that lets anyone drive a state model by clicking buttons. Use it when the question is about **business logic, state transitions or data shape**: the kind of thing that looks reasonable on paper, and only feels wrong once it's pushed through real cases.

It is one file with nothing to install, so it can go to a designer, a product manager or a domain expert, and they can feel the model for themselves. It speaks their language, not the code's.

## 1. Isolate the logic in a pure module

Put the logic that answers the question in one `<script>` block, written as a small, pure module that could be lifted into the real code later. The page around it is throwaway, and this module isn't.

Pick the shape that fits the question, not the one easiest to wire to a page:

1. **A pure reducer**: `(state, action) => state`, when actions are discrete events and the state is a single value.
2. **A state machine**: Explicit states and transitions, when "which actions are legal right now" is part of the question.
3. **Pure functions over a plain data type**: When there's no current state, only transformations.
4. **A module with a clear method surface**: When the logic genuinely owns ongoing internal state.

Keep it pure: no DOM, no `document`, and no button handlers reaching inside it. The page calls into the module, and nothing flows the other way.

## 2. Build the HTML file

One file of plain HTML, CSS and JavaScript, with everything inline. It needs no framework, bundler or server, so it opens with a double-click and survives being emailed around.

Write it for someone who isn't a developer. Every label uses the domain's language, so the buttons and the state read like the business, not the reducer.

Lay it out from top to bottom:

1. **Title and question**: What this demo lets you explore, in one line.
2. **Current state**: The full relevant state as a readable panel with labelled fields, not raw JSON, re-rendered after every click. Where it helps, call out what just changed.
3. **Free play**: One button per action, always available, so anyone can poke at the model in any order.
4. **Guided walkthroughs**: One scenario per tab. Each tab describes the situation in plain words and what to watch for, then lists the buttons to press, in order. Each step is a real button that performs its action and moves to the next step. Starting a walkthrough resets to a known state, so it runs the same way every time.

Choose scenarios that show the awkward cases: the happy path, a tricky edge case, and an attempt at something that should be illegal.

Keep it clean and restrained: clear typography, generous spacing, one accent colour, and no animations competing with the state and the buttons.

## 3. Capture and promote it

The pure module is the part worth keeping. When the answer is built for real, it can be the starting point for the real module, with its tests written as it lands. The HTML shell stays on the prototype branch, where it remains one file anyone can open again.
