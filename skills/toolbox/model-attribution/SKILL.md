---
name: model-attribution
description: Use before posting anything under the user's name.
---

# Model attribution

A post goes out under the user's name to people who cannot see this session, so it opens with a note saying which model wrote it and for whom. The note goes first, so the reader knows who they are reading before the content, however long the post. Every post carries the note unless the user says to leave it out of that post.

## The note

```markdown
> [!NOTE]
> Filed by <model name> on behalf of <name>.

<the post>
```

1. **`<model name>`**: The running model's display name, such as Fable 5.1, Opus 5.5 or GPT-6.1 Sol, rather than the API slug or the vendor alone.
2. **`<name>`**: The name the user gave for attribution, otherwise `git config user.name`. Outside a repository with no name given, ask.

## Posts

A post is any text the agent sends through a tool to a place other people read: issues, pull requests, reviews and comments on GitHub or Linear, Slack messages and emails. An edit keeps the note of the post it edits, where it is.

The note marks posts, not work product: commit messages, code and files in the repository carry none, and `commit` owns the rule for commit messages.

## By destination

1. **GitHub**: Use the alert above for every `gh` command that creates or edits text, such as `gh pr create`, `gh pr comment`, `gh pr review`, `gh issue create`, `gh issue comment` and their `--edit` forms. Pass the body with `--body-file` so the blank line and the `>` survive the shell.
2. **Everywhere else**: Use the sentence alone as the first line, followed by a blank line. The alert syntax is GitHub's, and a plain sentence reads correctly anywhere.
