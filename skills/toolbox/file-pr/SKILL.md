---
name: file-pr
description: Use when filing, opening or raising a PR.
---

# File PR

## 1. Gather the state

The **base** is the branch the user named. Without one, use the repository's integration branch (`dev` when it exists, otherwise the default branch).

Run `git fetch origin` first, because GitHub compares against the remote base. Then run these in parallel.

```bash
git status -sb  # branch, upstream, anything uncommitted
git log --format="%h %s" origin/<base>..HEAD   # commits this PR carries
git diff --stat origin/<base>...HEAD   # files it touches
gh pr list --state open --json number,headRefName,baseRefName,title
```

> Uncommitted changes stop the skill: say so and offer the `commit` skill. Nothing is pushed or opened with a dirty tree.

> An open PR with this branch as its head also stops the skill: report its URL.

## 2. Detect stacking

Skip this step when the user named the base.

If an open PR's head branch is an ancestor of `HEAD` and isn't yet merged into the base, **stack** this PR on it: use that head as the base, so the diff shows only the commits on top. When several match, stack on the nearest one.

## 3. Read the diff, not the log

Write the body from the diff, not from the log or your memory of the conversation.

- **Full diff**: Read `git diff origin/<base>...HEAD`, plus the files it touches where the diff can't explain the change.
- **Commits as a guide**: Use commits for grouping, not as a source of truth. A commit can describe an intent the final diff no longer matches.
- **What stays out**: Squashed or reverted work, and anything the conversation planned that the diff doesn't show.

This step is done when every file in the diff is accounted for.

## 4. Establish verification

Verification tells the reviewer what confirms the state of the PR, so it states only what was actually run at the branch head, with the command and the result.

Reuse results from this session when they came from the branch head. Otherwise, run the project's checks now (test suite, type checker, linter, build). Name what was **not** exercised, such as a UI that was never clicked through, so the reviewer knows where to look. Never claim a pass you did not see.

> When the PR is a performance change, show the same measurement before and after.

## 5. Draft the PR

### Title

1. **Past tense verbs**: Use "Added", "Fixed", "Refactored", "Migrated" or "Removed" to describe what was done, even where the log shows another style.
2. **Short and concise**: Typically under 70 characters, with no period.

### Body

1. **Attribution note**: Open with the note from `model-attribution`.
2. **Stacked note** (*Optional*): When stacked, name the PR it sits on and say to merge that first. A blocked PR instead names what it waits on and why merging early would break.
3. **Breaking changes** (*Optional*): Use a `> [!WARNING]` callout that names each change and what a client must do about it.
4. **Opening paragraph**: 2-3 sentences that explain what the PR establishes in present tense ("This pull request adds..."), closing with "The most important changes are:".
5. **Tree** (*Optional*): When the PR changes structure, such as a file move or split or a new call path, use a short file or call tree. Use a `diff` block when it changes an existing shape, and show only what the change touches.
6. **Groups**: Group changes by **what** they change (a rule, seam, tool or contract), not by file or commit. Each group is a bold heading with its entries below it. Each entry is one change, and a change that needs a paragraph is two entries:
   - Open with a past tense verb.
   - Name the real identifier in backticks.
   - End with the reason or the consequence, when either isn't obvious.
   - Show renames in arrow notation: `oldName -> newName`.
   - Include numbers where they change what the reviewer does, such as test counts, caps and line reductions.
7. **Testing**: List the coverage this PR adds.
8. **Verification**: List what was run and its result, then what was not exercised. This section is separate from Testing.

The body ends at the Verification section, so it carries no "Generated with" or "Co-Authored-By" lines.

### Example

<example>
> [!NOTE]
> Filed by <model name> on behalf of <name>.

> Stacked on #41 (`orders-module`). Merge that first; this PR only contains the commits on top of it.

> [!WARNING]
> `calculatePrice` -> `priceOrder`. Clients must now pass a currency, because `defaultCurrency` is gone.

This pull request moves pricing out of checkout and into the orders module, so every caller shares one calculation. Checkout no longer keeps its own copy of the discount rules. The most important changes are:

```diff
 src/
 ├── orders/
+│   └── pricing.ts
 └── checkout/
-    └── pricing.ts
```

**Pricing seam**
- Added `priceOrder` in `src/orders/pricing.ts` and routed `CheckoutForm`, `BasketSummary` and `InvoiceJob` through it; the private copies are removed.
- Removed the `defaultCurrency` fallback, so a missing currency now fails explicitly.

**Testing**
- Added unit coverage for `priceOrder` and a contract test for `InvoiceJob`.

**Verification**
- `pnpm test` at `a1b2c3d`: 214 passed.
- `pnpm typecheck` at `a1b2c3d`: no errors.
- Not exercised: the checkout page in a browser. Click through a discounted basket.
</example>

## 6. Get the go-ahead

Show the title and body in the conversation and wait. A PR is visible to the whole team, so this is the one confirmation the skill takes, and nothing is pushed or filed without it.

## 7. File it

Push the branch, with `-u` when it has no upstream, because `--head` makes `gh` skip pushing. Write the body to a temporary file and pass it with `--body-file`, so quoting survives every shell:

```bash
gh pr create --base <base> --head <branch> --title "<title>" --body-file <body-file>
```

Report the PR URL, the base it targets, and the number it stacks on, if any.
