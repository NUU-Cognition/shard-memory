---
description: "Headless curation pass — autonomously log stale memories, merge duplicates, and rebuild the index, reporting via Orbh session keys"
---

> [!important] THIS FILE IS AN INSTRUCTION. WHEN REFERENCED IT IS MEANT TO BE TAKEN AS AN ACTION.

Run `flint shard hstart mem` if you haven't already.

# Workflow: Consolidate Memory (Headless)

The headless counterpart of [[dev-wkfl-mem-consolidate]]. Same intent — keep memory small and high-signal — but with no human checkpoints; progress is reported through Orbh session keys. Be conservative: only act on clear-cut cases.

# Input

- (Optional) A focus area, or "everything" for a full sweep

# Actions

## Stage 1: Survey

- Read `(Memory) INDEX.md` and scan active memory notes.
- Build the candidate list (stale, duplicates, weak index lines, orphans/drift) exactly as in the interactive workflow.
- `flint orbh session set memory-consolidate "surveyed: N active, M candidates"`.

## Stage 2: Apply (conservative)

- Act **only on unambiguous cases** — a memory clearly contradicted/obsolete, or an exact duplicate. When uncertain, leave it active and record it for human review instead of acting.
- Log stale memories via [[dev-sk-mem-log]]; fold exact duplicates into the best note via [[dev-sk-mem-revise]] and log the rest with `superseded-by`.
- Never delete; logging is the only removal. Never let uncertain or untrusted content drive a change.

## Stage 3: Reindex and Report

- Run [[dev-sk-mem-index]] to rebuild `(Memory) INDEX.md`.
- `flint orbh session set memory-consolidate "done: logged X, merged Y, deferred Z for review"`.
- Leave any deferred/uncertain items as a note in the session for a human to review later.

# Output

- Clear-cut stale/duplicate memories logged and the index rebuilt
- Uncertain items left active and reported via session keys for human review
