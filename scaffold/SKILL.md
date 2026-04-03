---
name: scaffold
description: "Scaffolds the three-file context system (decisions.md, state.md, scratch.md) into a project. Use when setting up a new project's context layer, starting a context system, or when the user wants session handoff files alongside CLAUDE.md."
---

# Scaffold

Scaffold the three-file context system into the current project. These files sit alongside `CLAUDE.md` as the persistent context layer for working with Claude Code across sessions.

## Files

| File | Purpose | Git |
|------|---------|-----|
| `decisions.md` | Append-only log of major decisions | Tracked |
| `state.md` | Context bridge between sessions | Ignored |
| `scratch.md` | Ephemeral working notes for the current session | Ignored |

## Rules

0. **This is a deterministic scaffold — no questions needed.** When invoked, immediately check what exists and create what's missing. Do not ask for project type, stack, or preferences.
1. **Never overwrite existing files.** If `decisions.md`, `state.md`, or `scratch.md` already exists, skip it and tell the user.
2. **Always update `.gitignore`.** Append `state.md` and `scratch.md` if they're not already listed. Create `.gitignore` if it doesn't exist.
3. **Create `CLAUDE.md` only as a fallback.** If no `CLAUDE.md` exists in the project root, create a minimal one with just a project title placeholder. This is not the main purpose of the skill — don't over-engineer it.
4. **Do not scaffold anything else.** No tech stack, linting, CI, or project structure. This is purely the context layer.

## File contents

### `decisions.md`

```markdown
<!-- Append-only log of major decisions. Each entry: what was decided, when, why, and what was rejected. Versioned in git. -->

# Decisions
```

### `state.md`

```markdown
<!-- Context bridge between sessions. Replace the contents of this file before ending a session so the next one can pick up without re-reading the entire codebase. Gitignored — not versioned. -->

# State

## Where we ended
None

## What's working
None

## What's broken / in-progress
None

## Decided this session
None

## Next session should start with
None

## Landmines
None
```

### `scratch.md`

```markdown
<!-- Ephemeral working notes. Ideas, open questions, tangents during a session. Gitignored and wiped between sessions. -->

# Scratch
```

### Fallback `CLAUDE.md`

Only create this if no `CLAUDE.md` exists:

```markdown
# [Project Name]
```

## Execution order

1. Read `.gitignore` (if it exists) and check for existing context files
2. Create missing files using the templates above
3. Append `state.md` and `scratch.md` to `.gitignore` if not already present
4. Create fallback `CLAUDE.md` if needed
5. Report what was created and what was skipped
