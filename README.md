# skills-v2

Version 2 of my personal skills for Claude Code and Codex. Skills move here from [`cc-plugins`](https://github.com/MalasataXD/cc-plugins) one at a time, and each one passes a [blind model review](docs/porting.md) on the way.

## Install

```shell
npx skills add MalasataXD/skills-v2 -g -s commit decision-records domain-modeling file-pr grilling model-attribution
```

The repository is private, so the install needs access to it.

## Skills

| Skill | Use it when |
| --- | --- |
| `commit` | Committing changes |
| `decision-records` | Recording a hard-to-reverse decision, or changing an existing ADR |
| `domain-modeling` | Sharpening a term that is fuzzy or clashes with the domain language |
| `file-pr` | Filing, opening or raising a PR |
| `grilling` | Stress-testing a plan, decision or design |
| `model-attribution` | Posting anything under the user's name |

## Layout

```text
skills/
├── build/
├── check/
├── plan/
│   ├── decision-records/
│   ├── domain-modeling/
│   └── grilling/
└── toolbox/
    ├── commit/
    ├── file-pr/
    └── model-attribution/
```

Folders without a skill yet hold a `.gitkeep`, because git doesn't track empty folders. Delete it when the first skill lands in that folder.

## License

[MIT](LICENSE)
