# Skills

A curated collection of Claude Code skills built for how I work.

## Philosophy

- **Durability over cleverness.** When frontier models get 30% better, does this still make sense? If the value is just a system prompt, it's not a useful skill.
- **Skills that learn.** The best skills aren't shortcuts, but should compound over time.
- **Curated, not collected.** Quality and purpose over quantity.

## What's here

| Skill | What it does |
|-------|-------------|
| [fonts](#fonts) | Curated, opinionated list of fonts that aren't Inter. |
| [scaffold](#scaffold) | Three-file context system for session continuity. |
| [handoff](#handoff) | End-of-session update to state, decisions, and scratch. |

## Install

Symlink any skill into your Claude Code skills directory:
```bash
ln -s ~/skills/fonts/ ~/.claude/skills/fonts
ln -s ~/skills/scaffold/ ~/.claude/skills/scaffold
ln -s ~/skills/handoff/ ~/.claude/skills/handoff
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

You'll get prompts to select:

1. **Project type** — marketing site, SaaS app, editorial/blog, portfolio
2. **Vibe** — clean/minimal, bold/expressive, editorial/serious, friendly/warm
3. **Role** — full pairing, headings only, body only

Pick from the chips or choose "Other" to type your own answer.

The skill filters down to 2–4 font recommendations presented as selectable options. Once you pick, it detects your stack and sets everything up automatically — no confirmation step.

For **SaaS app**, the skill will also recommend a monospace font for code blocks, data tables, and technical UI after you've chosen your main pairing.

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

## Usage

Invoke directly:

```
/scaffold
```

Or trigger it naturally:

```
Set up the context system for this project
```

No questions asked — it checks what exists, creates what's missing, and reports back.

---

# handoff

The complement to scaffold. Runs at the end of a session before `/clear` to update the three context files so the next session can start cold.

- **state.md** — overwritten with where you ended, what's working, what's broken, landmines, and the first thing to do next session
- **decisions.md** — appends significant decisions (if any were made) with rationale and rejected alternatives
- **scratch.md** — wiped clean

## Usage

Invoke directly:

```
/handoff
```

Or trigger it naturally:

```
Let's wrap up
```

No questions asked — it reads the session context and git diff, writes the files, and shows you the result for a quick review before you `/clear`.

---
