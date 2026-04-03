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

---

### fonts

An opinionated Claude Code skill for picking and setting up typefaces. Surfaces a curated font list, asks about your project context with interactive prompts, and handles the full setup — imports, CSS custom properties, framework config.

---

## Install

Symlink any skill into your Claude Code skills directory:
```bash
ln -s ~/skills/fonts/ ~/.claude/skills/fonts
```

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
