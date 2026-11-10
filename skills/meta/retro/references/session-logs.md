# Session logs

Each tool keeps its sessions in logs on this machine:

1. **Claude Code**: `~/.claude/projects/<project folder>/<session id>.jsonl`, with sub-agent transcripts in `<session id>/subagents/`.
2. **Codex**: `~/.codex/sessions/` and `~/.codex/archived_sessions/`, mostly compressed as `.jsonl.zst`.
3. **Grok**: `~/.grok/sessions/<encoded folder>/<session id>/chat_history.jsonl`, with failed calls in `events.jsonl` beside it.
4. **opencode**: The SQLite database `~/.local/share/opencode/opencode.db`. Copy it and its `-wal` file to a temporary folder and read the copy, because opening the live database can write to it.

When the current session was compacted, its log holds what the summary dropped.

Give the log to a sub-agent. A log is mostly tool output, so have it read the user's messages and the agent's messages and tool calls first. Open a tool result when it failed, when it is large, or when it sits around a friction moment. Have it return each moment with its turn and a quote, not the log. When you are already a sub-agent, read it yourself.
