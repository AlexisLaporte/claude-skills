---
name: session
description: "Manage work sessions: save (close), resume (open), list, delete. Structured snapshots in sessions.json. Optional deep lookup in raw transcripts."
argument-hint: "[close|open|list|delete|lookup] [title or keyword]"
---

# Session

Manage structured work sessions stored in `sessions.json`. Save context when pausing, resume it later.

## Commands

| Command | Description |
|---------|-------------|
| `/session close [title]` | Save current session snapshot |
| `/session open [title or #]` | Resume a saved session |
| `/session list` | List all saved sessions |
| `/session delete [title or #]` | Delete a saved session |
| `/session update` | Update current session's snapshot |
| `/session lookup [keyword]` | Deep search in raw Claude transcripts |

Default (no subcommand): `list`.

## Storage

File: `~/.claude/projects/<project-slug>/sessions.json`

Project slug = cwd with `/` replaced by `-`, leading `-` stripped.

Format:
```json
{
  "id": "<session-uuid>",
  "title": "Short descriptive title",
  "branch": "main",
  "cwd": "/data/dev/project",
  "savedAt": "2026-02-13T14:30:00Z",
  "summary": ["Did X", "Fixed Y"],
  "pending": ["Still need A"],
  "keyFiles": ["src/foo.py", "docs/bar.md"],
  "note": "Optional context"
}
```

---

## close

Save a snapshot of the current session.

1. **Gather** from conversation:
   - **title**: from argument, or ask user
   - **branch**: `git branch --show-current`
   - **summary**: what was accomplished (2-5 bullets)
   - **pending**: what remains (bullets, can be empty)
   - **keyFiles**: important files read/modified (max 10, relative paths)
   - **note**: optional free-form

2. **Get session ID**: current session transcript is the most recent `.jsonl`:
   ```bash
   ls -t ~/.claude/projects/$(pwd | sed 's|/|-|g; s|^-||')/*.jsonl 2>/dev/null | head -1
   ```
   Extract UUID from filename.

3. **Save** to `sessions.json`:
   - Read existing (or `[]`)
   - If same ID exists: update in-place
   - Else: append
   - Write back

4. **Confirm**: show title + session ID

## update

Same as `close` but keeps the existing title. Finds the entry matching current session ID and updates summary/pending/keyFiles/note/savedAt.

## open

Resume a previously saved session.

1. **Load** `sessions.json`. If empty/missing: "No saved sessions."

2. **List** sessions (most recent first):
   ```
   1. [2026-02-13] Title here (branch)
   2. [2026-02-12] Other title (main)
   ```

3. **Select**: if argument matches title (partial) or number, auto-select. Otherwise ask.

4. **Inject context** as briefing:
   ```markdown
   ## Resuming: <title>
   **Branch:** `<branch>` | **Saved:** <date>

   ### What was done
   - <summary items>

   ### What's pending
   - <pending items>

   ### Key files
   - `<file1>`

   ### Notes
   <note>
   ```

5. **Read key files** silently (don't dump contents).

6. **Check branch**: if current branch differs, warn and ask to switch.

7. **Ready**: confirm context loaded.

## list

Show all saved sessions, most recent first. One-liner per session:
```
1. [2026-02-19] Campagnes unsubscribe (main)
2. [2026-02-18] Audit créances CSV (main)
```

## delete

Remove a session entry from `sessions.json` by title (partial match) or number.

## lookup

Deep search in raw Claude Code transcripts (slower, last resort).

Run the search script:
```bash
# Current project
~/.claude/skills/session/scripts/session-search

# All projects
~/.claude/skills/session/scripts/session-search --global

# By keyword
~/.claude/skills/session/scripts/session-search "keyword" --global

# By date
~/.claude/skills/session/scripts/session-search --from "2026-02-01" --to "2026-02-05"

# Limit results (default: 5)
~/.claude/skills/session/scripts/session-search "keyword" -n 1
```

Output shows transcript sessions with `claude --resume <id>` commands.

Only use lookup when sessions.json doesn't have what the user needs.
