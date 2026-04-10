# agent-continuity

Claude Code skills for surviving `/clear`. A three-file context layer and three skills that manage its lifecycle, so each session can start cold and pick up exactly where the last one left off.

## The problem

Claude Code sessions are ephemeral. `/clear` wipes context. Every new session starts without memory of the previous one. Continuity across sessions requires an explicit handoff — something that captures state before it's wiped and restores it at the start of the next session.

## The system

Three files maintained by three skills, one per stage of the session lifecycle:

| Stage | Skill | What it does |
|-------|-------|--------------|
| Setup (once per project) | [scaffold](#scaffold) | Creates `state.md`, `decisions.md`, and `scratch.md` in the project root |
| Start of each session | [orient](#orient) | Reads the context files and recent git activity, summarizes where things stand |
| End of each session | [preserve](#preserve) | Snapshots the session into `state.md`, appends to `decisions.md`, wipes `scratch.md` |

## Install

Symlink each skill into your Claude Code skills directory:
```bash
ln -s ~/skills/scaffold/ ~/.claude/skills/scaffold
ln -s ~/skills/orient/ ~/.claude/skills/orient
ln -s ~/skills/preserve/ ~/.claude/skills/preserve
```

---

# scaffold

Scaffolds the three-file persistent context layer for working with Claude Code across sessions.

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

# orient

The complement to preserve. Reads `state.md`, `decisions.md`, and recent git activity, then presents a concise orientation summary. Shows you where things stand and waits for direction.

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

# preserve

The complement to orient. Runs at the end of a session before `/clear` to update the three context files so the next session can start cold.

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
