---
name: orient
description: "Start-of-session orientation that reads context files and recent git activity, then presents a structured summary. Use when beginning a session, after /clear, or when picking up a project cold."
allowed-tools: Read Bash(git *)
---

# Orient

Orient before acting. Read the project's context files and recent git activity, synthesize a structured summary, and present it. Do not start any work.

## Recent git activity

!`git log --oneline -5 2>/dev/null || echo "(no commits found)"`

!`git status --short 2>/dev/null || echo "(not a git repo)"`

## Rules

0. **This is read-only — do not start working.** The entire purpose of this skill is to orient. Read files, synthesize, present, and stop. Do not create, edit, or fix anything. Do not run tests. Do not write code. Wait for the user to direct next steps.

1. **Require the scaffold files.** If `state.md`, `decisions.md`, or `scratch.md` don't all exist in the project root, stop and tell the user to run `/scaffold` first. Do not partially orient.

2. **Read everything before speaking.** Read `state.md`, `decisions.md`, `scratch.md`, and `CLAUDE.md` (if it exists) in full before producing any output. Combine them with the injected git context above to build the complete picture.

3. **Lead with what to do next.** The "Next session should start with" section of `state.md` is the most important signal. Put it first in the summary. The user wants to know the next action, not a recap.

4. **Be concrete, not narrative.** Use file paths, function names, error messages, and specific next steps. No "let's pick up where we left off" or "good progress was made." If `state.md` contains vague content, surface it as-is — don't embellish.

5. **Flag staleness.** If the injected git log shows commits that postdate the context in `state.md` (e.g., commits from a different session or manual work), call this out explicitly. The context files may be outdated.

6. **Flag emptiness.** If `state.md` sections are all "None" (the scaffold default), say so plainly: the context files exist but haven't been filled in yet. There's nothing to orient from — ask the user what they'd like to work on.

7. **Surface scratch.md only if non-empty.** Handoff wipes scratch.md clean. If it contains content, that's unusual — include it in the summary under a separate note. Otherwise omit it.

## Output format

Present the orientation as a single structured block:

```
## Session Orientation

### Next step
[Contents of "Next session should start with" from state.md — or "No next step recorded" if empty/None]

### Where we left off
[Contents of "Where we ended" from state.md]

### What's working
[Contents of "What's working" from state.md]

### What's broken / in-progress
[Contents of "What's broken / in-progress" from state.md — omit if None]

### Landmines
[Contents of "Landmines" from state.md — omit if None]

### Recent decisions
[Last 3 entries from decisions.md, or "No decisions logged yet" if empty]

### Git since last session
[Summary of injected git log and git status — uncommitted changes, recent commits. Omit if clean and nothing notable.]
```

After the block, print one line:

**Ready when you are. What should we work on?**

Do not say anything else after that line.

## Execution order

1. Check that `state.md`, `decisions.md`, and `scratch.md` all exist in the project root — if any are missing, tell the user to run `/scaffold` and stop
2. Read `state.md` in full
3. Read `decisions.md` in full
4. Read `scratch.md` in full
5. Read `CLAUDE.md` if it exists (skip without error if it doesn't)
6. Combine file contents with the injected git log and git status above
7. Check for staleness: compare git log against state.md content
8. Produce the orientation block in the output format above
9. Stop. Wait for user direction.
