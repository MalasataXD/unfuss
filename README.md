# unfuss

Skills that help a coding agent take a piece of work from a rough idea to a merged pull request, with no more fuss than the work needs. They run in Claude Code and Codex. I wrote them for my own projects, so they follow how I work: an idea is stress-tested before it's built, work is split into tickets, and nothing ships without a review.

This is the second version. Skills moved here one at a time from [`cc-plugins`](https://github.com/MalasataXD/cc-plugins), and each one passed a [blind review by three models](docs/porting.md) on the way.

## Install

The repository is private, so either route needs access to it. Use one route per agent, because in Claude Code the two together load every skill twice.

For Claude Code, Codex and other agents, with the [`skills` CLI](https://github.com/vercel-labs/skills). Leave out the names of any skills you don't want:

```shell
npx skills add MalasataXD/unfuss -g -s babysit-pr bug-hunt code-smells commit complete-ticket complexity decision-records diagnosing-bugs domain-modeling file-pr grilling handoff health-check implement model-attribution next-ticket peer-review prototype prove-it research retro review security-check ship simplify tdd to-spec to-tickets vet-tickets wait-what writing-for-agents writing-for-humans
```

For Claude Code only, as the plugin `unfuss`, whose skills you type with a prefix, as in `/unfuss:commit`:

```shell
claude plugin marketplace add MalasataXD/unfuss
claude plugin install unfuss@unfuss
```

## How the skills fit together

A typical piece of work goes through the skills in this order:

1. `grilling` stress-tests the idea until its open questions are settled. `research` and `prototype` help when a question needs digging or a quick try.
2. `to-spec` writes the result up as a spec, and `to-tickets` breaks the spec into tickets, which `vet-tickets` checks for gaps.
3. `next-ticket` picks up the next ticket, and `implement` builds it, with `tdd`, `simplify` and `review` along the way.
4. `complete-ticket` checks the result against the ticket's acceptance criteria.
5. `commit` and `file-pr` open the pull request, and `babysit-pr` carries it to green CI.

You can start at any step. A small fix can go straight to `implement`, and a bug with no obvious cause starts with `diagnosing-bugs`.

## Skills

Most skills also start on their own when the agent sees a task they fit. Five run only when you ask for them: `babysit-pr`, `handoff`, `health-check`, `peer-review` and `retro`.

### Understand: working out what to build

| Skill | Use it when |
| --- | --- |
| `domain-modeling` | Sharpening a term that is fuzzy or clashes with the domain language |
| `grilling` | Stress-testing a plan, decision or design |
| `prototype` | Trying something out before it's built for real |
| `research` | Researching an answer that needs real digging, or is worth keeping |

### Plan: turning a settled idea into work

| Skill | Use it when |
| --- | --- |
| `decision-records` | Recording a hard-to-reverse decision, or changing an existing ADR |
| `to-spec` | Writing up the current conversation as a spec |
| `to-tickets` | Breaking work, a plan or a spec down into tickets |
| `vet-tickets` | Checking tickets, or a breakdown, for gaps a builder would stall on |

### Build

| Skill | Use it when |
| --- | --- |
| `diagnosing-bugs` | Diagnosing something broken, failing or slow whose cause isn't obvious |
| `implement` | Building an approved plan or ticket |
| `next-ticket` | Picking up the next piece of work or the next open ticket |
| `simplify` | Simplifying or cleaning up code, or applying project standards to it |
| `tdd` | Building a feature or fixing a bug test-first |

### Check

| Skill | Use it when |
| --- | --- |
| `bug-hunt` | Hunting for bugs in a change, beyond what a review checks |
| `complete-ticket` | Checking a finished ticket against its acceptance criteria |
| `health-check` | Finding where reducing complexity would pay off |
| `review` | Reviewing code, or running a quality check |
| `security-check` | Checking code for security problems, or code that touches untrusted input |

### Toolbox: everyday tasks around the work

| Skill | Use it when |
| --- | --- |
| `babysit-pr` | Carrying an open pull request to green CI |
| `commit` | Committing changes |
| `file-pr` | Filing, opening or raising a pull request |
| `handoff` | Handing the current conversation over to a fresh agent |
| `model-attribution` | Posting anything under your name |
| `wait-what` | Re-explaining a message that lost you |
| `writing-for-humans` | Writing or tightening a document people will read |

### Knowledge: reference that other skills load

| Skill | Use it when |
| --- | --- |
| `code-smells` | Reviewing or refining code |
| `complexity` | Shaping a plan, a module or a seam, or weighing a shortcut or special case |
| `prove-it` | Judging whether work is done, or backing a claim with evidence |

### Meta: working on the agent's own setup

| Skill | Use it when |
| --- | --- |
| `retro` | Looking back at a session for what to improve in the agent's setup |
| `writing-for-agents` | Writing or editing a skill, an `AGENTS.md` or a `CLAUDE.md` |

### Experimental: not proven yet

These install and work like the rest. They stay here until they've earned a place in my own workflow.

| Skill | Use it when |
| --- | --- |
| `peer-review` | Getting a cold review from other models |
| `ship` | Deciding whether finished work is ready to ship |

## Working on the library

Each skill is a folder under `skills/<group>/` that holds its `SKILL.md`, an `agents/openai.yaml` for Codex, and sometimes a `references/` folder.

- [`docs/porting.md`](docs/porting.md) is how a skill is drafted, reviewed and promoted. [`docs/ideas.md`](docs/ideas.md) holds changes that wait for a later port.
- A new skill needs a line in `.claude-plugin/marketplace.json`, or the Claude Code plugin won't load it. Add its name to the `npx skills` command above as well.
- After you change a skill's `description` or `disable-model-invocation`, run `scripts/Sync-OpenAiYaml.ps1` to rewrite its `agents/openai.yaml`. With `-Check`, it lists the files that drifted instead.

## Credits

Several skills are taken or adapted from [mattpocock/skills](https://github.com/mattpocock/skills) (MIT).

## License

[MIT](LICENSE)
