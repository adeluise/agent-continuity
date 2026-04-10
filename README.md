# Skills

A curated collection of Claude Code skills built for how I work.

## Philosophy

Skills are curated. A skill earns its place by being useful across multiple projects over time. Don't create a skill for a one-off task or something a prompt can handle. Durability over cleverness.

## What's here

| Skill | What it does |
|-------|-------------|
| [fonts](#fonts) | Curated, opinionated list of fonts that aren't Inter. |
| [orient](#orient) | Start-of-session orientation from context files and git. |
| [preserve](#preserve) | End-of-session update to state, decisions, and scratch. |
| [scaffold](#scaffold) | Three-file context system for session continuity. |

## Install

Symlink any skill into your Claude Code skills directory:
```bash
ln -s ~/skills/fonts/ ~/.claude/skills/fonts
ln -s ~/skills/orient/ ~/.claude/skills/orient
ln -s ~/skills/preserve/ ~/.claude/skills/preserve
ln -s ~/skills/scaffold/ ~/.claude/skills/scaffold
```

---

# fonts

An opinionated Claude Code skill for picking and setting up typefaces. Surfaces a curated font list, asks about your project context with interactive prompts, and handles the full setup — imports, CSS custom properties, framework config.

## Usage

Invoke the skill directly:

```
/fonts
```

Or trigger it naturally in conversation:

```
I need a font pairing for a marketing landing page
```

## Customizing

Edit `fonts.md` to add, remove, or reorder fonts. The skill reads this file every time it's invoked — no restart needed. Follow the existing table format and tag fonts with `heading`, `body`, `code`, or a combination.

---

# scaffold

Scaffolds a three-file, the persistent context layer for working with Claude Code across sessions.

| File | Purpose |
|------|---------|
| `decisions.md` | Append-only log of major decisions: what, when, why, and what was rejected |
| `state.md` | Context bridge between sessions — replace before ending so the next session can pick up cold |
| `scratch.md` | Ephemeral working notes, ideas, open questions — wiped between sessions |

These sit alongside `CLAUDE.md` (which should already exist) in the project root and creates a minimal `CLAUDE.md` if one doesn't exist.

state.md is tracked so its history is versioned. On teams, expect merge conflicts — preserve does a full replacement, and a merge strategy will come when there's a real team case.

## Usage

Invoke directly:

```
/scaffold
```

Or trigger it naturally:

```
Set up the context system for this project
```

It checks what exists and sets up what's missing.

---

# preserve

The complement to scaffold. Runs at the end of a session before `/clear` to update the three context files so the next session can start cold.

- **state.md** — overwritten with where you ended, what's working, what's broken, landmines, and the first thing to do next session
- **decisions.md** — appends significant decisions (if any were made) with rationale and rejected alternatives
- **scratch.md** — wiped clean

## Usage

Invoke directly:

```
/preserve
```

Or trigger it naturally:

```
Let's wrap up
```

It reads the session context and git diff, writes the files, and shows you the result for a quick review before you `/clear`.

---

# orient

The complement to preserve. Reads state.md, decisions.md, and recent git activity, then presents a concise orientation summary. Shows you where things stand and waits for direction.

## Usage

Invoke directly:

```
/orient
```

Or trigger it naturally:

```
Let's pick this back up
```

It reads the context files and git history, synthesizes a summary with the next step front and center, and waits for you to say what to work on.

---
