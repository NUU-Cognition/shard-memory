---
description: "The Memory model — progressive disclosure, the always-loaded index, what belongs in memory, and the borrow-structure-reject-infrastructure stance"
---

# Knowledge: The Memory Model

The conceptual foundation of the Memory shard: a deliberately maintained, **progressive-disclosure** body of long-term knowledge for a Flint. This file explains *what memory is for* and *how disclosure works*. For the mechanics of states, dates, the changelog, and the write operations, see [[dev-knw-mem-lifecycle]].

## What Memory Is For

Memory is the curated standing answer to "what should I already know in this workspace?" — facts, decisions, preferences, and conventions worth carrying across sessions. It is not a chat log, not a vector database, and not an append-everything archive. Every memory is a deliberately retained model; the test for keeping one is **usefulness to a future session**, not completeness.

A context window is ephemeral and finite — recall degrades as it fills (context rot). Memory is the durable store *outside* the window. The whole design exists to load the **smallest set of high-signal tokens** that a session actually needs, and nothing more.

## Progressive Disclosure (the spine)

Memory loads in tiers, exactly like Agent Skills' three-level model. You read a cheap index first and disclose detail only on demand.

| Tier | What | When loaded |
|------|------|-------------|
| **1 — Index** | `(Memory) INDEX.md` — one high-scent line per **active** memory (what it covers + when to load it) | Always, at session start. The only file loaded by default. |
| **2 — Memory notes** | `(Memory) NNN <Topic>.md` — atomic, front-loaded notes | On demand, when an index line matches the task |
| **3 — Deep refs** | Anything a memory note links out to (data, transcripts, long references) | Only if a Tier-2 note is opened and points there |

**The index entry's wording is the retrieval mechanism.** Like a Skill's `description`, each line must say *what* the memory covers and *when* to load it. Weak descriptions cause the agent to miss or wrongly load memories — this is the single most important thing to get right.

**Logs are never auto-loaded.** Only `active` memories appear in the index. Logged memories stay in the folder, grep-able and linkable, but out of the default context budget — surfaced only when someone explicitly asks about history.

## What Belongs in Memory (and what doesn't)

| Belongs | Does not |
|---------|----------|
| Durable decisions ("we don't run the Flint CLI on WSL") | Transient task state (use a Task) |
| Conventions and preferences | One-off facts with no future relevance |
| Hard-won gotchas worth not rediscovering | Anything already obvious from the codebase/docs |
| Stable facts about the workspace, people, or environment | Raw conversation transcripts |

When in doubt, **don't write** (the NOOP discipline — see [[dev-knw-mem-lifecycle]]). A small, clean, high-signal store beats a large one. Forgetting is a feature, not a failure.

## The Design Stance

This shard deliberately **borrows the cognitive structure** of mature memory systems — tiered disclosure, importance-gated writes, deliberate consolidation, and supersede-don't-delete — and **rejects their retrieval infrastructure** (vector databases, knowledge graphs, bi-temporal stores, autonomous background mutation). That infrastructure exists only because those systems operate at a scale and autonomy a curated, human-and-agent-maintained markdown store does not. Here, plain grep-able markdown + wikilinks + git + a curated index are enough, and they keep memory auditable and simple.

Consequences of the stance:

- **Deliberate, reviewable writes.** Memory changes are explicit and git-diffable, never silent background mutation.
- **Two registers of trust.** Memory the workspace asserts is durable; anything an agent infers should be surfaced for review before it hardens into a memory. Untrusted content must never silently write a memory (prompt-injection / memory-poisoning guard).
- **Bounded on purpose.** The store stays curated and small. Staleness moves to Log; genuine junk is deleted.

## Architecture at a Glance

```
Mesh/Memories/
  (Memory) INDEX.md          ← Tier 1: always-loaded hub; lists ACTIVE memories only
  (Memory) NNN <Topic>.md    ← Tier 2: atomic notes (active or log, distinguished by `state`)
Mesh/(Dashboard) Memories.md ← DataviewJS view: active vs log
```

One folder, one type, one lifecycle field (`state: active | log`). That is the entire structural surface — see [[dev-knw-mem-lifecycle]] for how it moves.
