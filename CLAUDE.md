# Skills Repo

## Philosophy
Skills are curated. A skill earns its place by being useful across multiple projects over time. Don't create a skill for a one-off task or something a prompt can handle. Durability over cleverness.

## Structure
Each skill is a top-level directory (e.g., `fonts/`, `handoff/`). No `skills/` parent folder.
Every skill has a `SKILL.md` with frontmatter metadata and implementation rules.
Additional data files live alongside SKILL.md as needed.

## Adding a Skill
Follow the existing pattern: create a top-level directory with a SKILL.md.
Install via symlink: `ln -s ~/skills/SKILL/ ~/.claude/skills/SKILL`

## README
Concise and direct. One-liner table summarizing all skills, then a short paragraph per skill.
No feature lists, badges, or filler. When editing, make minimal targeted changes.
