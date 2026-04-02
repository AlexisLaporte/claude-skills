---
name: session-copy
description: "Duplicate (fork) the current Claude Code session into an independent copy."
argument-hint: "[session-id]"
---

# Session Copy

Duplicate a Claude Code session by copying its JSONL transcript with a new UUID. Both sessions then evolve independently from the same point.

## Usage

Run the copy script. Without arguments, it copies the **current session**.

```bash
# Copy current session (auto-detects session ID from $CLAUDE_SESSION_ID or latest .jsonl)
~/.claude/skills/session-copy/scripts/session-copy

# Copy a specific session
~/.claude/skills/session-copy/scripts/session-copy <session-id>
```

Output: the new session ID and the `claude --resume <id>` command to open it.

After running the script, show the user the resume command so they can open the copy.
