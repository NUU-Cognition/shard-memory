---
required-reading:
  - "[[dev-knw-mem-model]]"
  - "[[dev-knw-mem-lifecycle]]"
---

# Memory

Deliberately maintained, **progressive-disclosure** long-term memory for this Flint: a curated `Mesh/Memories/` folder with a cheap always-loaded index and atomic notes loaded on demand. Memory is the curated standing answer to "what should I already know here?" — durable facts, decisions, preferences, and conventions worth carrying across sessions. Not a chat log, not a vector DB, not an append-everything archive.

The design **borrows the cognitive structure** of mature memory systems (tiered disclosure, importance-gated writes, deliberate consolidation, supersede-don't-delete) and **rejects the retrieval infrastructure** (vector DBs, knowledge graphs). Plain markdown + wikilinks + git + a curated index are enough — and keep memory auditable and simple. Full rationale in [[dev-knw-mem-model]]; mechanics in [[dev-knw-mem-lifecycle]].

## Progressive Disclosure

Memory loads in tiers — read the cheap index first, disclose detail on demand.

| Tier | What | When loaded |
|------|------|-------------|
| 1 — Index | `(Memory) INDEX.md` — one high-scent line per **active** memory | Always, at session start (the only file loaded by default) |
| 2 — Notes | `(Memory) NNN <Topic>.md` — atomic, front-loaded | On demand, when an index line matches |
| 3 — Refs | whatever a note links out to | only if that note is opened |

**The loading contract:** at the start of meaningful work, read `(Memory) INDEX.md` first; open a memory note only when its index line matches the task. **Logs are never auto-loaded** — only `active` memories appear in the index.

## Lifecycle

```
remember           log (stale / superseded)
  ∅ ──► active ──────────────────────────► log
          │ ▲                                ▲
   revise │ └──────────────────────── remember (born-as-log)
          ▼                                  ∅
       active
```

| State | Meaning |
|-------|---------|
| `active` | Current, true now. Listed in the INDEX, loaded on demand. |
| `log` | Aged-out, superseded, or a past event recorded as history. Not indexed, never auto-loaded. |

The one transition is **active → log** — the "forget" operation: *move, don't delete*, preserving the trail in git and the note's `## Changelog`. Frontmatter carries two dates: `created` (always) and `logged` (only when logged); per-edit history lives in the in-note `## Changelog`, not an `updated` field. See [[dev-knw-mem-lifecycle]].

## Operations

| Verb | Skill | Purpose |
|------|-------|---------|
| Remember | [[dev-sk-mem-remember]] | Capture a new memory (active, or born-as-log); add it to the INDEX. Importance-gated — when in doubt, don't write. |
| Revise | [[dev-sk-mem-revise]] | Edit an active memory in place + append a changelog line. |
| Log | [[dev-sk-mem-log]] | Transition active → log (the "forget"); optionally `superseded-by`. |
| Recall | [[dev-sk-mem-recall]] | Load the INDEX and disclose matching notes; search logs only on explicit request. |
| Index | [[dev-sk-mem-index]] | Rebuild `(Memory) INDEX.md` from the active notes on disk. |

Periodic curation is the [[dev-wkfl-mem-consolidate]] workflow (log stale memories, merge duplicates, sharpen the index) — deliberate, human-reviewed, never automatic.

## Dashboards

| Dashboard | Purpose | Maintained By |
|-----------|---------|---------------|
| `(Dashboard) Memories.md` | Active vs log memories at a glance | DataviewJS |

## Rules

- Memory lives only in `Mesh/Memories/` — one flat folder; `state` (not location) distinguishes active from log.
- The note title IS the memory (a standalone claim), or `YYYY-MM-DD — <event>` for a born-as-log record.
- Keep memories atomic and short; keep the INDEX lean. A small, high-signal store beats a large one.
- Writes are deliberate and git-diffable. Inferred memories are surfaced for review, never silently authoritative; untrusted content must never write a memory.
- Forgetting is a feature — prefer logging stale memories over hoarding them.
