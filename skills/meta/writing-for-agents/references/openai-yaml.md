# agents/openai.yaml

Codex reads `agents/openai.yaml` beside a skill's `SKILL.md`. It is config for the harness, not text for the agent, and Codex ignores Claude's `disable-model-invocation`. So a user-invoked skill that runs in Codex needs this file, or Codex fires it on its own.

```yaml
interface:
  display_name: "Retro"
  short_description: "Look back at a session for what to improve"
  default_prompt: "Use $retro to look back at this session for friction."

policy:
  allow_implicit_invocation: false
```

1. **`interface.display_name`**: The name shown in Codex's skill lists.
2. **`interface.short_description`**: A blurb for the list, 25 to 64 characters.
3. **`interface.default_prompt`**: Optional. One example prompt that names the skill as `$name`.
4. **`policy.allow_implicit_invocation`**: Set it to `false` for a user-invoked skill, so the skill stays out of the model's context until the user types `$name`. A model-invoked skill leaves the `policy` block out, because the default is `true`.
5. **`dependencies.tools`**: Optional. The MCP servers the skill needs, each with `type: "mcp"`, `value`, `description`, `transport` and `url`.

Quote every string value, and leave the keys unquoted.
