# Claude Code Skills

Generic, reusable skills for Claude Code.

## Skills

| Skill | Description |
|-------|-------------|
| `ci-cd` | Infrastructure, CI/CD, deployment on tuls.me |
| `document` | Update project documentation (CLAUDE.md + docs/) |
| `session` | Save/resume work sessions |

## Installation

Each skill is a directory with a `SKILL.md` file. Install by symlinking into `~/.claude/skills/`:

```bash
ln -sf /data/projects/claude-skills/skills/<skill> ~/.claude/skills/<skill>
```

## Other skill sources

Skills are spread across repos, each linked into `~/.claude/skills/`:

| Source | Skills |
|--------|--------|
| `/data/oto/skills/` | oto-browser, oto-cli, oto-google, oto-search, oto-whatsapp |
| `/data/howto/skills/` | brainstorm, product, strategy |
| `/data/pro/skills/` | pdf-generator |
| `/data/projects/project-tracker/skills/` | projects |
