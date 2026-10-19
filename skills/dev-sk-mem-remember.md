---
description: "Capture a new memory — create an active (or born-as-log) memory note and add it to the INDEX"
orb:
  - "@v1/nuucognition/session/17a6a729-0275-4ec6-ae83-70b1340527ff#flint/nuu-orbh"
---

> [!important] THIS FILE IS AN INSTRUCTION. WHEN REFERENCED IT IS MEANT TO BE TAKEN AS AN ACTION.

Run `flint shard start mem` if you haven't already.

# Skill: Remember

Capture a durable fact, decision, preference, or convention as a new memory note and register it in the index.

# Input

- The thing to remember (a claim, decision, or past event)
- (Optional) Whether it is a live fact (→ `active`, default) or a past event recorded as history (→ born-as-log)

# Actions

1. **Apply the importance gate (NOOP first).** Is this durable and useful to a *future* session? Transient task state, one-off facts, or anything obvious from the codebase/docs do NOT belong in memory. When in doubt, don't write — say so and stop. See [[dev-knw-mem-model]] § What Belongs in Memory.
2. **Check for an existing memory on this topic.** Search `Mesh/Memories/` (grep the titles/INDEX). If an active memory already covers it, use [[dev-sk-mem-revise]] instead of creating a duplicate.
3. **Get the number.** `flint helper type newnumber Memory`.
4. **Create the note** from [[dev-tmp-mem-memory-v0.1]] at `Mesh/Memories/(Memory) NNN <Topic>.md`:
   - Title = the memory as a standalone claim (e.g. `Flint CLI is not run on WSL`), or `YYYY-MM-DD — <event>` for a born-as-log record.
   - `state: active` and `created: <today>` for a live fact. For born-as-log, set `state: log`, `created: <today>`, `logged: <today>`.
   - Front-load the essential claim (BLUF), keep it atomic and short.
   - Seed `## Changelog` with `- <today> — Created.` (or `Recorded.` for born-as-log).
   - Stamp your session in `orb:`: run `flint address me`, and append its `by` address (quoted) to `orb:`. The template has the field. See [[knw-f-artifacts]] § Session Tracking.
   - Add `authors` from `.flint/identity.json` (omit if none).
5. **Update the INDEX** (`Mesh/Memories/(Memory) INDEX.md`) — only if the memory is `active`: add one high-scent line linking the note with a description saying *what* it covers and *when* to load it. (Born-as-log notes are NOT indexed.) Keep the INDEX lean.

# Output

- A new `(Memory) NNN <Topic>.md` note in `Mesh/Memories/`
- The INDEX updated with its line (for active memories)
