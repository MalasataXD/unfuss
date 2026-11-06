---
name: commit
description: Use when committing changes.
---

# Commit

## 1. Get the state

Run these in parallel.

```bash
git status
git diff
git diff --cached
git log -3 --format="%h %s"
```

## 2. Group the changes

Review every staged and unstaged change: what changed, why, and what it affects. Then sort the changes into atomic groups, one per commit, so each commit can be reverted on its own.

This step is done when every changed file belongs to one group or is left out on purpose.

- **Paths scoped by a calling skill**: Commit only those paths and leave the rest unstaged.
- **Several groups**: Make one commit per group and report the split. Commits are local and cheap to undo, so splitting needs no approval.
- **Ambiguous grouping**: Ask if someone is there to answer. Otherwise, take the grouping you can defend and name it in the report.

## 3. Draft the message

Every message explains *what* changed and *why* it matters, to someone who wasn't in the session. Local planning files, such as tickets, specs and plans, stay out of it, and so does any hint of them, such as a ticket number or "as planned".

### Title

1. **Past tense verbs**: Use "Added", "Fixed", "Refactored", "Migrated" or "Removed" to describe what was done, even where the log shows another style.
2. **Short and concise**: Typically under 60 characters, with no period.

### Body

1. **Opening paragraph**: Give the context and the overall purpose of the change.
2. **`Changes:` section**: List each specific modification as a `-` bullet in past tense.
   - Name the exact files, functions or components.
   - Keep related changes next to each other.
   - Show renames in arrow notation: `oldName -> newName`.
   - Mention additions, updates and deletions.
   - Include line count reductions with percentages, when relevant.
3. **Closing line** (*Optional*): `Fixes <issue>` or a summary of the impact.

The message ends at the closing line, so it carries no "Generated with" or "Co-Authored-By" lines.

### Examples

<examples>
<example>
Fixed the SKILL link in writing-for-agents' SKILL-MECHANICS

SKILL-MECHANICS.md linked to SKILL.md as a sibling, but it sits one level up from references/.

Changes:
- Updated the writing-for-agents link in skills/meta/writing-for-agents/references/SKILL-MECHANICS.md: SKILL.md -> ../SKILL.md

Fixes the broken link back to the writing-for-agents skill.
</example>

<example>
Added the babysit-pr skill

An unattended CI loop that pushes needs firm limits, so the flow now has
a user-invoked skill whose invocation is the go-ahead to push.

Changes:
- skills/ship/babysit-pr: added the loop that clears conflicts, waits for CI, and fixes the cause of red checks, capped at two rounds
- .claude-plugin/marketplace.json: registered babysit-pr in the main tier
- README.md: added babysit-pr to the install command and skills table

Pull requests can now be carried to green CI without weakening a test or merging.
</example>
</examples>

## 4. Stage and commit

Stage the group's files by path, leaving out secrets, `.env` files, credentials and local planning files.

Pass the message as one literal block, because stacked `-m` flags turn each line into its own paragraph and a double-quoted block corrupts `$var` and backticks.

In bash:

```bash
git add file1.ts file2.tsx file3.json
git commit -m "$(cat <<'EOF'
<message/>
EOF
)"
git status
```

In PowerShell, with the closing `'@` at the start of its line:

```powershell
git add file1.ts file2.tsx file3.json
git commit -m @'
<message/>
'@
git status
```

## 5. Confirm success

Display the following:

- The hash and title of each commit
- How many files each commit changed
- Branch status (ahead/behind remote)
- Any changed files left out, and why
