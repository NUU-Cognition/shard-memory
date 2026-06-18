---
description: "Rebuild (Memory) INDEX.md from the active memory notes on disk"
---

> [!important] THIS FILE IS AN INSTRUCTION. WHEN REFERENCED IT IS MEANT TO BE TAKEN AS AN ACTION.

Run `flint shard start mem` if you haven't already.

# Skill: Index

Regenerate the always-loaded index from the active memory notes — repairs drift, drops logged entries, and keeps descriptions high-scent. Use after bulk edits or when the index and the notes have diverged.

# Input

- (none — operates on the current `Mesh/Memories/` contents)

# Actions

1. **Scan.** List every `(Memory) NNN <Topic>.md` in `Mesh/Memories/` and read each one's frontmatter `state` and its title + opening claim.
2. **Partition.** Keep only `state: active` notes for the index. Exclude `state: log` notes and the `INDEX.md` file itself.
3. **Write one line per active memory:** a wikilink to the note plus a high-scent description — *what* it covers and *when* to load it (derive from the note's title and BLUF first sentence; preserve any good existing description).
4. **Order sensibly** (e.g. by number, or grouped by area if the store is large) and keep the file lean — the index is loaded every session, so trim filler.
5. **Overwrite** `Mesh/Memories/(Memory) INDEX.md` with the rebuilt list, keeping its header intact.

# Output

- `(Memory) INDEX.md` rebuilt to exactly match the active memories on disk
