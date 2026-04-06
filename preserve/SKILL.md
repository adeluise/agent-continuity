---
name: preserve
description: "End-of-session context preservation that updates state.md, appends decisions to decisions.md, and wipes scratch.md. Use when wrapping up a session, before /clear, or when the user says they're done for now."
allowed-tools: Read Edit Write Bash(git *)
---

# Preserve

Preserve the current session's context into the three-file system so the next session can start cold. Run this at the end of a session, before `/clear`.

## Scaffold check

!`[ -f state.md ] && [ -f decisions.md ] && [ -f scratch.md ] && echo "SCAFFOLD_OK" || echo "SCAFFOLD_MISSING: run /scaffold first"`

**If the output above says SCAFFOLD_MISSING, stop immediately and tell the user to run `/scaffold`. Do not continue.**

## Session changes

Files changed this session:
!`[ -f state.md ] && { git diff --stat 2>/dev/null; git diff --cached --stat 2>/dev/null; } || true`

Changed file list:
!`[ -f state.md ] && { git diff --name-only 2>/dev/null; git diff --cached --name-only 2>/dev/null; } || true`

## Rules

0. **Don't ask questions — just do it.** Derive everything from the session's conversation, the injected git diff above, and the current state of the three files. Present the result for review when done.

1. **Require the scaffold files.** If the scaffold check above says `SCAFFOLD_MISSING`, stop and tell the user to run `/scaffold` first. Do not create them.

2. **Read before writing.** Before touching anything, read the current `state.md`, `decisions.md`, and `scratch.md` so you know what's already there.

3. **Use the injected context.** The git diff output above was injected at skill load time. Combine it with the conversation history — don't rely on either source alone. Do not re-run git diff unless the injected output is empty.

4. **Be concrete, not reflective.** Every section in `state.md` should contain specific file paths, function names, error messages, or next steps. Never write vague summaries like "made good progress" or "things are working well."

5. **Decisions have a threshold.** Only append to `decisions.md` if a decision meets at least two of these criteria:
   - Closes a door — choosing A over B, and going back would cost real time
   - Future-you would ask "why did we do it this way?" and the answer isn't obvious from the code
   - Something was explicitly rejected
   - Changes the project's direction or constraints

   If nothing qualifies, don't append anything. Don't log variable naming, formatting, or anything reversible in five minutes.

6. **Never remove existing decisions.** `decisions.md` is append-only. Add new entries after the last existing entry.

7. **Wipe scratch.md clean.** Replace its contents with just the header comment and heading — nothing else.

8. **Present the result.** After writing all three files, show the user what was written to `state.md` and what was appended to `decisions.md` (if anything) so they can correct it before the session ends.

## Templates

### `state.md` — full replacement

```markdown
<!-- Context bridge between sessions. Replace the contents of this file before ending a session so the next one can pick up without re-reading the entire codebase. Gitignored — not versioned. -->

# State

## Where we ended
[What was the last thing being worked on. File paths, function names, specific context.]

## What's working
[Features, systems, or components confirmed working this session.]

## What's broken / in-progress
[Anything left incomplete, failing, or partially implemented. Include error messages if relevant.]

## Decided this session
[Quick summary of decisions made — details go in decisions.md.]

## Next session should start with
[The first concrete action for next session. Not a wish list — the single most important next step, then supporting items.]

## Landmines
[Gotchas, surprising behavior, things that look wrong but are intentional, or things that look right but are broken.]
```

### `decisions.md` — append only

```markdown
### YYYY-MM-DD — [Decision title]
**Why:** [Rationale — what drove the decision]
**Rejected:** [What was considered and passed over, and why]
```

### `scratch.md` — wiped

```markdown
<!-- Ephemeral working notes. Ideas, open questions, tangents during a session. Gitignored and wiped between sessions. -->

# Scratch
```

## Execution order

1. Check that `state.md`, `decisions.md`, and `scratch.md` all exist in the project root — if any are missing, tell the user to run `/scaffold` and stop
2. Read current contents of all three files
3. Overwrite `state.md` with filled-in template using the injected git diff and conversation history
4. Review session for decisions that meet the threshold — append to `decisions.md` if any qualify, skip if none do
5. Wipe `scratch.md` back to its empty template
6. Show the user what was written to `state.md` and what (if anything) was appended to `decisions.md`
