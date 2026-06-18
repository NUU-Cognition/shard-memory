---
id: {{uuid}}
tags:
  - "#dashboard"
  - "#mem/dashboard"
  - "#managed/shard/mem"
  - "#read-only"
---

```dataviewjs
function formatName(p) {
  return p.file.name.replace(/^\(Memory\)\s*/, '');
}

const memories = dv.pages('#mem/memory').where(p => p.file.name !== '(Memory) INDEX');

// Active — current memories, surfaced through the index and loaded on demand
dv.header(1, "Active");
const active = memories.where(p => p.state === 'active')
  .array().sort((a, b) => b.file.name.localeCompare(a.file.name));
if (active.length === 0) {
  dv.paragraph("*None*");
} else {
  dv.table(["Memory", "Created"],
    active.map(p => [
      dv.fileLink(p.file.path, false, formatName(p)),
      p.created ?? "—"
    ])
  );
}

// Log — aged-out or historical memories, kept for the record, never auto-loaded
dv.header(1, "Log");
const log = memories.where(p => p.state === 'log')
  .array().sort((a, b) => String(b.logged ?? "").localeCompare(String(a.logged ?? "")));
if (log.length === 0) {
  dv.paragraph("*None*");
} else {
  dv.table(["Memory", "Created", "Logged", "Superseded By"],
    log.map(p => [
      dv.fileLink(p.file.path, false, formatName(p)),
      p.created ?? "—",
      p.logged ?? "—",
      p["superseded-by"] ?? "—"
    ])
  );
}
```
