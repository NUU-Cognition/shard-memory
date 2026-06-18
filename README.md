# Memory

Deliberately maintained, **progressive-disclosure** long-term memory for a Flint: a curated `Mesh/Memories/` folder with a cheap always-loaded index and atomic notes loaded on demand. Memory is the curated standing answer to "what should I already know here?" — durable facts, decisions, preferences, and conventions worth carrying across sessions.

The design **borrows the cognitive structure** of mature memory systems (tiered disclosure, importance-gated writes, deliberate consolidation, supersede-don't-delete) and **rejects the retrieval infrastructure** (vector DBs, knowledge graphs). Plain markdown + wikilinks + git + a curated index — auditable and simple.

## The model

- **One folder, one type, one lifecycle field.** `Mesh/Memories/` holds `(Memory) NNN <Topic>.md` notes; `state: active | log` is the only thing that moves them.
- **Progressive disclosure.** `(Memory) INDEX.md` is the always-loaded Tier-1 hub listing active memories with high-scent descriptions; notes are Tier 2, loaded on demand; deep refs are Tier 3. Logs are never auto-loaded.
- **Active → Log is the "forget" operation** — *move, don't delete*. Frontmatter carries `created` (always) and `logged` (only when logged). Revision history lives in an in-note `## Changelog`, not an `updated` field.

## Capabilities

| Kind | File | Purpose |
|------|------|---------|
| Skill | `dev-sk-mem-remember.md` | Capture a new memory (active or born-as-log); add it to the index |
| Skill | `dev-sk-mem-revise.md` | Edit an active memory in place + append a changelog line |
| Skill | `dev-sk-mem-log.md` | Transition active → log (the forget); optional `superseded-by` |
| Skill | `dev-sk-mem-recall.md` | Load the index and disclose matching notes; search logs only on request |
| Skill | `dev-sk-mem-index.md` | Rebuild `(Memory) INDEX.md` from the active notes on disk |
| Workflow | `dev-wkfl-mem-consolidate.md` | Periodic curation pass with human review (+ headless `dev-hwkfl-*`) |
| Template | `dev-tmp-mem-memory-v0.1.md` | The memory note (Active/Log frontmatter + changelog) |
| Knowledge | `dev-knw-mem-model.md` | The memory model — progressive disclosure, what belongs in memory |
| Knowledge | `dev-knw-mem-lifecycle.md` | States, dates, changelog, and the operations |

## Installs

- `Mesh/Memories/` and `Mesh/Archive/Memories/` (folders)
- `Mesh/Memories/(Memory) INDEX.md` (seed index, `inst-mem-index.md`)
- `Mesh/(Dashboard) Memories.md` (DataviewJS: active vs log, `inst-mem-memories_dashboard.md`)
- `(Type) Memory (Memory Shard).md` (type definition)
- Obsidian template `otmp-mem-memory.md`

## Structure

```
Shards/(Dev Local) Memory/
  shard.yaml                         # Manifest (not prefixed)
  dev-init-mem.md                    # Init — shard context + loading contract
  dev-hinit-mem.md                   # Headless init
  skills/        dev-sk-mem-{remember,revise,log,recall,index}.md
  workflows/     dev-wkfl-mem-consolidate.md, dev-hwkfl-mem-consolidate.md
  templates/     dev-tmp-mem-memory-v0.1.md
  knowledge/     dev-knw-mem-{model,lifecycle}.md
  install/       type-mem-memory.md, inst-mem-index.md,
                 inst-mem-memories_dashboard.md, otmp-mem-memory.md   (NOT prefixed)
```
