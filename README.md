# skills-v2

Version 2 of my personal skills for Claude Code and Codex. Skills move here from [`cc-plugins`](https://github.com/MalasataXD/cc-plugins) one at a time, and each one passes a [blind model review](docs/porting.md) on the way.

## Install

```shell
npx skills add MalasataXD/skills-v2 -g -s bug-hunt code-smells commit complete-ticket complexity decision-records diagnosing-bugs domain-modeling file-pr grilling implement model-attribution next-ticket prototype prove-it research review security-check simplify tdd to-spec to-tickets vet-tickets
```

The repository is private, so the install needs access to it.

## Skills

| Skill | Use it when |
| --- | --- |
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
| `implement` | Building an approved plan or ticket |
| `model-attribution` | Posting anything under the user's name |
| `next-ticket` | Picking up the next piece of work or the next open ticket |
| `prototype` | Trying something out before it's built for real |
| `prove-it` | Judging whether work is done, or backing a claim with evidence |
| `research` | Researching an answer that needs real digging, or is worth keeping |
| `review` | Reviewing code, or running a quality check |
| `security-check` | Checking code for security problems, or code that touches untrusted input |
| `simplify` | Simplifying or cleaning up code, or applying project standards to it |
| `tdd` | Building a feature or fixing a bug test-first |
| `to-spec` | Writing up the current conversation as a spec |
| `to-tickets` | Breaking work, a plan or a spec down into tickets |
| `vet-tickets` | Checking tickets, or a breakdown, for gaps a builder would stall on |

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
│   ├── review/
│   └── security-check/
├── knowledge/
│   ├── code-smells/
│   ├── complexity/
│   └── prove-it/
├── plan/
│   ├── decision-records/
│   ├── domain-modeling/
│   ├── grilling/
│   ├── prototype/
│   ├── research/
│   ├── to-spec/
│   ├── to-tickets/
│   └── vet-tickets/
└── toolbox/
    ├── commit/
    ├── file-pr/
    └── model-attribution/
```

Folders without a skill yet hold a `.gitkeep`, because git doesn't track empty folders. Delete it when the first skill lands in that folder.

## License

[MIT](LICENSE)
