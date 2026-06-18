---
description: "Periodic curation pass — log stale memories, merge duplicates, sharpen the index, keep the store small and high-signal"
---

> [!important] THIS FILE IS AN INSTRUCTION. WHEN REFERENCED IT IS MEANT TO BE TAKEN AS AN ACTION.

Run `flint shard start mem` if you haven't already.

# Workflow: Consolidate Memory

The deliberate "reflection" pass that keeps memory curated: surface stale and duplicate memories, log or merge them with human review, and rebuild a lean index. Never automatic — a human approves before anything is logged or merged.

# Input

- (Optional) A focus area, or "everything" for a full sweep

# Actions

## Stage 1: Survey

- Read `Mesh/Memories/(Memory) INDEX.md`, then scan the active memory notes (`state: active`).
- Build a candidate list:
  - **Stale** — memories that are likely no longer true or no longer useful to a future session.
  - **Duplicates / overlaps** — multiple active notes covering the same concept (memory should be atomic).
  - **Weak index lines** — descriptions that don't clearly say *what* and *when*.
  - **Orphans / drift** — notes missing from the index, or index lines pointing at logged/missing notes.
- Present the candidate list to the user with a recommended action per item (log, merge, revise, or sharpen-index).
- Once the user approves the plan, progress to the next stage.

## Stage 2: Apply

- For each approved item:
  - **Stale** → log it via [[dev-sk-mem-log]] (set `superseded-by` if a successor exists).
  - **Duplicate** → keep the best note, fold any unique content into it via [[dev-sk-mem-revise]], and log the rest (`superseded-by` the keeper).
  - **Weak/incorrect content** → [[dev-sk-mem-revise]].
- Make no destructive change the user did not approve. Prefer logging over deleting.
- Once all approved items are handled, progress to the next stage.

## Stage 3: Reindex and Confirm

- Run [[dev-sk-mem-index]] to rebuild `(Memory) INDEX.md` from the now-current active notes.
- Summarize what was logged, merged, and revised, and present the refreshed index to the user.
- Once the user confirms, the pass is complete.

# Output

- A smaller, higher-signal set of active memories
- Stale/duplicate memories moved to `log` (not deleted), with the trail preserved
- A rebuilt, lean `(Memory) INDEX.md`
