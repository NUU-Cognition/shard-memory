---
description: "Memory note — an atomic Active/Log memory with an in-note changelog"
---

# Filename: Mesh/Memories/(Memory) NNN [Topic].md

/* A single memory note. Lives flat in Mesh/Memories/ — Active and Log notes share the
   folder and are distinguished by the `state` field, not by location.
   Get NNN with: flint helper type newnumber Memory
   The note TITLE is the memory, stated as a standalone claim ("Flint CLI is not run on WSL"),
   or for a born-as-log event, lead with the date ("2026-06-18 — \<what happened\>"). */

```markdown
---
id: [generate-uuid4]
tags:
  - "#mem/memory"
state: [active (current, true now, listed in the INDEX)|log (aged-out or historical, never auto-loaded)]
created: [ISO 8601 date the memory first came into existence]
logged: /* ISO 8601 — ONLY present when state is log; the date it stopped being current */
superseded-by: /* optional wikilink — only if a newer ACTIVE memory replaced this one: "[[(Memory) NNN Title]]" */
orbh-sessions:
  - "[[agent-session-uuid]]"
template: "[[tmp-mem-memory-v0.1]]"
authors: /* from .flint/identity.json; omit the field entirely if no identity is set */
  - "[[@Person Name]]"
---

# [The memory as a standalone claim, or "YYYY-MM-DD — <event>" for a born-as-log record]

[The memory content. Front-load the essential claim in the first sentence (BLUF), then any
 rationale or pointers. Keep it atomic — one concept per note. Keep it short.]

## Changelog

/* Append-only, oldest → newest. One dated line per substantive change. The top of the note
   always reflects current truth; the changelog records how it got there. This replaces an
   `updated` field — the last changelog date IS the effective last-updated date.
   The Active → Log transition is itself just a final changelog line plus the `state`/`logged` edit. */

- [YYYY-MM-DD] — Created. /* for a born-as-log note: "Recorded." */
- (continue)
```

/* Notes:
   - state: active → log is the only lifecycle transition. Logging is the "forget" operation —
     move, don't delete. Use flint helper delete only for genuinely erroneous notes.
   - superseded-by: set when a fresher ACTIVE memory replaces this one (new note carries the new
     fact; this one becomes a log pointing forward). Plain staleness with no replacement just
     flips state to log with no superseded-by.
   - Active notes are listed in (Memory) INDEX.md with a one-line description; Log notes are not.
*/
