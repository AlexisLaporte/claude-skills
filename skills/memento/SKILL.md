---
name: memento
description: Capture session learnings into project memory (CLAUDE.md + docs/). Use after significant work to keep docs in sync.
disable-model-invocation: false
argument-hint: "[claude|docs|all]"
---

# Memento — Project Memory

Leave a note for future-you. Two layers:
1. **CLAUDE.md** — concise project map (always loaded)
2. **docs/** — detailed per-concept docs (loaded on demand)

Default scope (`all`): both. `claude` = CLAUDE.md only. `docs` = docs/ only.

---

## 0. Reflect first

Before touching any file, reflect on the session:

1. **What context was missing?** — commands discovered, config quirks, gotchas
2. **What patterns worked?** — testing, code style, environment setup
3. **What would help a fresh session?** — knowledge that isn't obvious from the code

Don't just sync the diff — capture what you *learned*.

---

## 1. CLAUDE.md — The Map

### Gather

- Read current `CLAUDE.md` (if exists)
- Explore: directories, config, key modules
- Review session history for what changed

### Write

Target: **under 500 lines**. Every line earns its place.

```markdown
# <Project Name>

<1-2 line purpose>

## Stack
<bullet list: language, frameworks, key deps>

## Architecture
<tree, folders only, with inline comments>
project/
├── src/           # Main application code
├── scripts/       # Build and deploy scripts
└── tests/         # Test suites

## Commands
<build, test, lint, deploy — commands only>

## Conventions
<style, naming, patterns — only what prevents mistakes>

## Key Concepts
<domain terms, business logic Claude must know>

## Docs
Detailed docs in `docs/`:
- `architecture.md` — <1-line description>
- `data-model.md` — <1-line description>
```

### Rules

- NO verbose explanations — Claude infers
- NO duplicating docs/ content — reference with short description
- `## Docs` section MUST list all docs/ files with 1-line description
- Preserve existing custom instructions (git workflow, env vars, etc.)
- Ask before removing existing content
- Use `@docs/filename.md` import if a doc should always be loaded

### What NOT to add

- Obvious code info (`UserService handles user operations` — the name says it)
- Generic best practices (universal, not project-specific)
- One-off fixes unlikely to recur
- Verbose explanations — prefer `Auth: JWT with HS256` over a paragraph

---

## 2. docs/ — The Territory

### Process

1. **Assess**: review what was done (conversation history, git diff)
2. **Identify**: which existing docs are affected, or if a new doc is needed
3. **Propose**: tell the user which docs to update/create and what changes
4. **Validate**: get user approval before writing
5. **Sync**: ensure CLAUDE.md `## Docs` section lists any new file

If nothing warrants doc updates, say so and move on.

### Location

Path from existing CLAUDE.md, or default `docs/` at project root.

### Structure

Flat, 1 file per concept:

```
docs/
  architecture.md    # Components, interactions, data flow
  api.md             # Endpoints, request/response
  data-model.md      # Schema, models, relationships
  setup.md           # Prerequisites, installation, env vars
  deployment.md      # Deploy process, environments
  <concept>.md       # Domain-specific as needed
```

### File format

```markdown
# <Concept>

<1-line purpose>

## <Section>
<content: headers, tables, code blocks — no prose paragraphs>
```

### Rules

- Max 500 lines per file — split if larger
- Include file paths with line refs (`src/auth/login.ts:42`)
- Update incrementally, don't rewrite
- If a doc is accurate and unaffected, don't touch it
- Accuracy over coverage: only document what's verifiable from code

---

## 3. Output

After running, print:
- Files created/modified
- CLAUDE.md changes (sections added/updated/removed)
- Docs updated/created (or "no doc changes needed")
