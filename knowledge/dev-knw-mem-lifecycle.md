---
description: "Memory lifecycle mechanics — the Active/Log states, the two dates, the in-note changelog, and the remember/revise/log/recall/index operations"
---

# Knowledge: Memory Lifecycle

How a memory moves over time. The model and disclosure philosophy live in [[dev-knw-mem-model]]; this file is the mechanical reference for states, dates, the changelog, and the operations.

## The Two States

```
        remember                 log (stale / superseded)
  ∅ ─────────────► active ──────────────────────────────► log
                     │ ▲                                     ▲
              revise │ └───────── (logs are terminal) ───────┘
                     ▼                              remember (born-as-log)
                  active                                  ∅ ──► log
```

| State | Meaning | In the index? | Loaded by default? |
|-------|---------|---------------|--------------------|
| `active` | Current, true now | Yes | On demand (Tier 2) |
| `log` | Aged-out, superseded, or a past event recorded as history | No | Never (history-only) |

There is exactly one transition: **active → log**. It is the "forget" operation — *move, don't delete*. Logs are terminal; if a logged fact becomes true again, write a fresh active memory (optionally linking back). Genuinely erroneous notes are the only case for `flint helper delete`.

## The Two Dates

Frontmatter carries at most two dates. Per-edit history lives in the changelog, not in frontmatter.

| Field | Meaning | Active | Log |
|-------|---------|--------|-----|
| `created` | When the memory first came into existence | ✅ always | ✅ always |
| `logged` | The date it stopped being current (became a Log) | absent | ✅ |

There is intentionally **no `updated` field** — the last `## Changelog` date is the effective last-updated date.

`superseded-by` (optional wikilink) is added only when a *newer active memory replaces* this one.

## The In-Note Changelog

Every memory note ends with a `## Changelog` section: append-only, oldest → newest, one dated line per substantive change.

```markdown
## Changelog
- 2026-06-18 — Created.
- 2026-09-02 — Added rationale (path translation + file-watching).
- 2027-01-15 — Logged: WSL support landed in CLI v2; superseded by [[(Memory) 042 Flint CLI runs on WSL]].
```

The top of the note always reflects current truth; the changelog records how it got there. The active → log transition is itself just a final changelog line plus the `state`/`logged` edit.

## Worked Example (the canonical case)

A live decision, born active:

```yaml
state: active
created: 2026-06-18
```
> `# Flint CLI is not run on WSL` … `## Changelog` → `- 2026-06-18 — Created.`

Later it goes stale because WSL support lands. It transitions to log — nothing is deleted:

```yaml
state: log
created: 2026-06-18
logged: 2027-01-15
superseded-by: "[[(Memory) 042 Flint CLI runs on WSL]]"
```
> `## Changelog` gains `- 2027-01-15 — Logged: superseded by [[(Memory) 042 …]].`

A past event recorded straight as history (born-as-log): `created == logged`, no `superseded-by`, changelog has a single `Recorded.` line.

## Operations

Each maps to a skill. The verbs mirror the ADD / UPDATE / DELETE / NOOP discipline, kept deliberate and reviewable.

| Operation | Skill | What it does |
|-----------|-------|--------------|
| **Remember** (ADD) | [[dev-sk-mem-remember]] | Create a new memory (active by default, or born-as-log) and add its line to the INDEX. Importance-gated — resist writing low-signal facts (NOOP). |
| **Revise** (UPDATE) | [[dev-sk-mem-revise]] | Edit an active memory's content in place and append a changelog line. Latest truth wins. Refresh the INDEX line if the gist changed. |
| **Log** (the "forget") | [[dev-sk-mem-log]] | Transition active → log: set `state: log`, add `logged`, append a changelog line, optionally set `superseded-by`, and remove the line from the INDEX. |
| **Recall** | [[dev-sk-mem-recall]] | Load the INDEX, disclose matching active notes; search logs only when history is explicitly requested. |
| **Index** | [[dev-sk-mem-index]] | Rebuild `(Memory) INDEX.md` from the active notes on disk (drops logged entries, repairs drift). |

## The INDEX File

`(Memory) INDEX.md` is the always-loaded Tier-1 hub. It lists **active** memories only, one line each, as a wikilink plus a high-scent description (*what* it covers + *when* to load it). Keep it lean — a giant index defeats the purpose. `remember`/`revise`/`log` keep it in sync inline; `index` rebuilds it from scratch when it drifts.

## Consolidation

Periodically (on demand), run [[dev-wkfl-mem-consolidate]]: scan active memories, log the stale ones, merge duplicates, sharpen index descriptions, and rebuild the INDEX. This is the deliberate "reflection" pass that keeps the store small and high-signal. It is never automatic — a human reviews before logging/merging.
