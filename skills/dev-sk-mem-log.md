---
description: "Transition a memory Active to Log — the deliberate forget operation (move, don't delete)"
---

> [!important] THIS FILE IS AN INSTRUCTION. WHEN REFERENCED IT IS MEANT TO BE TAKEN AS AN ACTION.

Run `flint shard start mem` if you haven't already.

# Skill: Log

Age an active memory out to `log` when it goes stale or is superseded. This is the "forget" operation — **move, don't delete** — so the trail is preserved.

# Input

- The memory to log (title or wikilink)
- (Optional) The successor memory, if a fresher active memory now replaces this one
- The reason it is being logged

# Actions

1. **Open the memory note** in `Mesh/Memories/`. Confirm it is `state: active`.
2. **If a fresher fact replaces it:** first `remember` the new active memory (via [[dev-sk-mem-remember]]) so the new truth exists as its own note. Note its wikilink for step 3.
3. **Flip the state:**
   - Set `state: log`.
   - Add `logged: <today>`.
   - If superseded, add `superseded-by: "[[(Memory) NNN <Successor>]]"`. (Plain staleness with no replacement → no `superseded-by`.)
4. **Append a changelog line:** `- <today> — Logged: <reason>[; superseded by [[...]]].`
5. **Remove its line from the INDEX** (`(Memory) INDEX.md`) — logs are never indexed or auto-loaded.
6. **Track it:** append your session id to `orbh-sessions`.

/* Only use `flint helper delete "<name>"` for genuinely erroneous notes that should never have
   existed. Stale-but-true history is logged, not deleted. */

# Output

- The memory note transitioned to `state: log` with `logged` set and a final changelog entry
- Its line removed from the INDEX
