---
description: "Revise an active memory in place and append a changelog line"
orb:
  - "@v1/nuucognition/session/17a6a729-0275-4ec6-ae83-70b1340527ff#flint/nuu-orbh"
---

> [!important] THIS FILE IS AN INSTRUCTION. WHEN REFERENCED IT IS MEANT TO BE TAKEN AS AN ACTION.

Run `flint shard start mem` if you haven't already.

# Skill: Revise

Update an existing active memory's content when the truth refines or changes — latest truth wins, with the history preserved in the changelog.

# Input

- The memory to revise (title or wikilink)
- What changed

# Actions

1. **Open the memory note** in `Mesh/Memories/`. Confirm it is `state: active`. If it is already a log, do not edit it in place — `remember` a fresh active memory instead (and optionally `superseded-by`-link the log via [[dev-sk-mem-log]]).
2. **Edit the body** so the top of the note reflects the new current truth. Keep it atomic and front-loaded. Do NOT add an `updated` field — there is none.
3. **Append a changelog line:** `- <today> — <what changed>.` (append-only, oldest → newest).
4. **Refresh the INDEX line** in `(Memory) INDEX.md` if the gist or the "when to load it" changed. Leave it as-is otherwise.
5. **Track the edit:** stamp your session in `orb:`: run `flint address me`, and append its `by` address (quoted) to `orb:`. Add nothing only when `orb:` or `orbh-sessions` already has an item that is exactly your `by` address, `@v1/<org>/session/<uuid>`, `[[<uuid>]]`, or your bare `<uuid>`. Old notes keep `orbh-sessions`: never remove it, and add no new item to it. See [[knw-f-artifacts]] § Session Tracking. Add yourself to `authors` if identity is set and not already listed.

# Output

- The memory note updated in place, with a new `## Changelog` entry
- The INDEX line refreshed if the gist changed
