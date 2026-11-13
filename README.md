# skills-v2

Version 2 of my personal skills for Claude Code and Codex. Skills move here from [`cc-plugins`](https://github.com/MalasataXD/cc-plugins) one at a time, and each one passes a [blind model review](docs/porting.md) on the way.

## Install

```shell
npx skills add MalasataXD/skills-v2 -g -s babysit-pr bug-hunt code-smells commit complete-ticket complexity decision-records diagnosing-bugs domain-modeling file-pr grilling handoff health-check implement model-attribution next-ticket peer-review prototype prove-it research retro review security-check ship simplify tdd to-spec to-tickets vet-tickets wait-what writing-for-agents writing-for-humans
```

The repository is private, so the install needs access to it.

## Skills

| Skill | Use it when |
| --- | --- |
| `babysit-pr` | Carrying an open PR to green CI, when you ask for it |
| `bug-hunt` | Hunting for bugs in a change, beyond what a review checks |
| `code-smells` | Reviewing or refining code |
| `commit` | Committing changes |
| `complete-ticket` | Checking a finished ticket against its acceptance criteria |
| `complexity` | Shaping a plan, a module or a seam, or weighing a shortcut or special case |
| `decision-records` | Recording a hard-to-reverse decision, or changing an existing ADR |
| `diagnosing-bugs` | Diagnosing something broken, failing or slow whose cause isn't obvious |
| `domain-modeling` | Sharpening a term that is fuzzy or clashes with the domain language |
| `file-pr` | Filing, opening or raising a PR |
| `grilling` | Stress-testing a plan, decision or design |
| `handoff` | Handing the current conversation over to a fresh agent, when you ask for it |
| `health-check` | Finding where reducing complexity would pay off, when you ask for it |
| `implement` | Building an approved plan or ticket |
| `model-attribution` | Posting anything under the user's name |
| `next-ticket` | Picking up the next piece of work or the next open ticket |
| `peer-review` | Getting a cold review from other models, when you ask for it |
| `prototype` | Trying something out before it's built for real |
| `prove-it` | Judging whether work is done, or backing a claim with evidence |
| `research` | Researching an answer that needs real digging, or is worth keeping |
| `retro` | Looking back at a session for what to improve in the agent's environment, when you ask for it |
| `review` | Reviewing code, or running a quality check |
| `security-check` | Checking code for security problems, or code that touches untrusted input |
| `ship` | Deciding whether finished work is ready to ship |
| `simplify` | Simplifying or cleaning up code, or applying project standards to it |
| `tdd` | Building a feature or fixing a bug test-first |
| `to-spec` | Writing up the current conversation as a spec |
| `to-tickets` | Breaking work, a plan or a spec down into tickets |
| `vet-tickets` | Checking tickets, or a breakdown, for gaps a builder would stall on |
| `wait-what` | Re-explaining a message that lost you |
| `writing-for-agents` | Writing or editing a skill, an `AGENTS.md` or a `CLAUDE.md` |
| `writing-for-humans` | Writing or tightening a document people will read |

## Layout

```text
skills/
├── build/
│   ├── diagnosing-bugs/
│   ├── implement/
│   ├── next-ticket/
│   ├── simplify/
│   └── tdd/
├── check/
│   ├── bug-hunt/
│   ├── complete-ticket/
│   ├── health-check/
│   ├── review/
│   └── security-check/
├── experimental/
│   ├── peer-review/
│   └── ship/
├── knowledge/
│   ├── code-smells/
│   ├── complexity/
│   └── prove-it/
├── meta/
│   ├── retro/
│   └── writing-for-agents/
├── plan/
│   ├── decision-records/
│   ├── to-spec/
│   ├── to-tickets/
│   └── vet-tickets/
├── toolbox/
│   ├── babysit-pr/
│   ├── commit/
│   ├── file-pr/
│   ├── handoff/
│   ├── model-attribution/
│   ├── wait-what/
│   └── writing-for-humans/
└── understand/
    ├── domain-modeling/
    ├── grilling/
    ├── prototype/
    └── research/
```

Folders without a skill yet hold a `.gitkeep`, because git doesn't track empty folders. Delete it when the first skill lands in that folder.

## License

[MIT](LICENSE)
