---
name: babysit-pr
description: Use when the user asks to babysit, watch or monitor a PR.
disable-model-invocation: true
---

# Babysit PR

Carry an open pull request to green CI. Invoking this skill is the user's go-ahead to push to the pull request's branch. Merging it stays with the user.

> Fix the cause of each failure, and keep every test and check as strong as it was. When a test itself looks wrong, stop and report it.

## Waiting

Use one watcher per pull request, and never a polling or `sleep` loop of your own:

1. **In T3 Code**: Call `watch_pull_request`, and end your turn. It wakes you when a check fails, the checks pass, someone comments or reviews, or the branch conflicts.
2. **Elsewhere**: Run `python <this skill's directory>/scripts/pr-wait <n> --until green`, by its absolute path. It prints one line per real event, such as a failed check, all checks finished, a new comment or review, base commits that touch this pull request's files, or a conflict. It exits 0 when the checks are green or the pull request merged, 1 when it needs you, and 2 on bad input. Run it with a monitor that wakes you on each line where the harness has one, such as Claude Code's Monitor tool, and otherwise as one long background command. Start it again after each push, and after a re-run once `gh pr checks <n>` shows the re-run check as pending. It remembers what it already reported, and refuses to start a second watcher on the same pull request.

The same rules apply to both:

1. **Only the owner watches**: A delegated child task never watches. The thread that started this skill owns the pull request.
2. **A wake is news**: Read the pull request's state yourself before acting on it.
3. **Quiet when nothing changed**: When an event needs no action, end your turn without a message.
4. **Unwatch before you stop**: Before you report, ask the user anything or hand back, call `unwatch_pull_request` in T3 Code, or stop a `pr-wait` that is still running.
5. **Rate limits**: `gh pr view` and `gh pr checks` use GitHub's GraphQL limit, and REST has its own. When a read fails on the limit, use `gh api repos/<owner>/<repo>/pulls/<n>` and `gh api repos/<owner>/<repo>/commits/<sha>/check-runs` instead. Check `gh api rate_limit --jq .resources.graphql` before watching again, and wait for its reset with one background command.

## 1. Pin the pull request

Resolve its number, base, head branch, head SHA and author:

```bash
gh pr view <n> --json number,baseRefName,headRefName,headRefOid,mergeable,mergeStateStatus,author
```

Work on the head branch. With no pull request, point to the `file-pr` skill and stop. With uncommitted changes, stop and offer the `commit` skill. Confirm before pushing to a pull request the user didn't open.

This step is done when you are on the head branch with a clean tree.

## 2. Bring it up to date

When the pull request conflicts with its base or is behind it, merge the base into the branch, resolve any conflicts, run the project's checks and push. `gh api repos/<owner>/<repo>/compare/<base>...<head> --jq .behind_by` shows how far behind it is. A merge of the base costs no round, but a busy base can keep moving: after two merges, stop updating and report it.

A rebase rewrites history, so it waits for the user. Stop and report a conflict that needs a decision about which behaviour wins.

This step is done when the branch is neither conflicting with its base nor behind it.

## 3. Wait

Wait as described in [Waiting](#waiting). Act only on checks newer than the latest push. When the pull request has no checks, report that and stop.

1. **Green**: The required checks pass, or every check when none are required. When the branch has fallen behind its base meanwhile, go back to step 2. Otherwise, read all the checks with `gh pr checks <n>`, list any optional check that is failing or pending under **Left for you**, and go to step 5.
2. **A conflict, or the base moved**: Go back to step 2.
3. **A red check**: Go to step 4.
4. **A review or comment**: Note it for the report, and keep waiting. The conversation is the user's.

## 4. Fix a red check

When the failure has nothing to do with the diff and looks flaky, re-run it once with `gh run rerun <run id> --failed`, and go back to step 3. That costs no round.

Otherwise, find the cause with the `diagnosing-bugs` skill, fix it, run the project's checks, commit with the `commit` skill and push. Each pushed fix is one round. Go back to step 3.

> Two rounds is the cap. When CI is still red after the second, stop. Stop sooner when a fix needs a design decision.

## 5. Report

When an overlapping pull request makes this one obsolete, stop, report it, and ask before closing anything.

Otherwise, unwatch the pull request, then report in the conversation, leaving out any line with nothing to say:

```markdown
## Babysit PR: <pull request URL>

**CI:** Green / Red at `<short sha>`

**Fixes:** Each fix and its `<short sha>`, with the rounds used.

**Left for you:** What is still red or undecided, and why it stopped.

**Review comments:** Any that arrived, listed without action.

**Ship:** Pushed changes moved the head, so an earlier `ship` verdict no longer covers this branch.
```
