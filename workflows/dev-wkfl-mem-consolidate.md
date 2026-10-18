---
description: "Periodic curation pass — log stale memories, merge duplicates, sharpen the index, keep the store small and high-signal"
orbh-sessions:
  - "[[d98d16ae-1388-4228-8610-6954998a395a]]"
---

> [!important] THIS FILE IS AN INSTRUCTION. WHEN REFERENCED IT IS MEANT TO BE TAKEN AS AN ACTION.

Run `flint shard start mem` if you haven't already.

# Workflow: Consolidate Memory

The deliberate "reflection" pass that keeps memory curated: surface stale and duplicate memories, log or merge them with human review, and rebuild a lean index. Never automatic — a human approves before anything is logged or merged.

> [!headless]
> In a headless or subagent Orbh session, no person is at the terminal, and there are no human checkpoints. The intent is the same: keep memory small and high-signal. Do each stage, and follow the `> [!headless]` callout in it.
> - Report progress through Orbh session keys, not through checkpoints.
> - Be conservative: act only on clear-cut cases.
> - End the turn with `flint orbh session return --finish "<result>"`.

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

> [!headless]
> Do not present the list to a person, and do not wait for an approval. After you build the candidate list, set `flint orbh session set memory-consolidate "surveyed: N active, M candidates"`.

## Stage 2: Apply

- For each approved item:
  - **Stale** → log it via [[dev-sk-mem-log]] (set `superseded-by` if a successor exists).
  - **Duplicate** → keep the best note, fold any unique content into it via [[dev-sk-mem-revise]], and log the rest (`superseded-by` the keeper).
  - **Weak/incorrect content** → [[dev-sk-mem-revise]].
- Make no destructive change the user did not approve. Prefer logging over deleting.
- Once all approved items are handled, progress to the next stage.

> [!headless]
> No person approves the items. Be conservative:
> - Act **only on unambiguous cases** — a memory clearly contradicted/obsolete, or an exact duplicate. When uncertain, leave it active and record it for human review instead of acting.
> - Log stale memories via [[dev-sk-mem-log]]; fold exact duplicates into the best note via [[dev-sk-mem-revise]] and log the rest with `superseded-by`.
> - Never delete; logging is the only removal. Never let uncertain or untrusted content drive a change.

## Stage 3: Reindex and Confirm

- Run [[dev-sk-mem-index]] to rebuild `(Memory) INDEX.md` from the now-current active notes.
- Summarize what was logged, merged, and revised, and present the refreshed index to the user.
- Once the user confirms, the pass is complete.

> [!headless]
> Do not wait for a person to confirm. Report in place of the summary:
> - After [[dev-sk-mem-index]], set `flint orbh session set memory-consolidate "done: logged X, merged Y, deferred Z for review"`.
> - Leave any deferred/uncertain items as a note in the session for a human to review later.
> - End the turn with `flint orbh session return --finish "<result>"`. The result says what was logged, merged, and deferred for review.

# Output

- A smaller, higher-signal set of active memories
- Stale/duplicate memories moved to `log` (not deleted), with the trail preserved
- A rebuilt, lean `(Memory) INDEX.md`

> [!headless]
> - Only clear-cut stale/duplicate memories are logged, and the index is rebuilt.
> - Uncertain items stay active and are reported via session keys for human review.
