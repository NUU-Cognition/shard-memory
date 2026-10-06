# 0.1.4

- The dependency on the Flint shard accepts any version (`"@nuucognition/flint": ""`), so the shard works with Flint 0.3.x and with Flint 0.4.0 (NUU Flint Task 1129).

# 0.1.3

- `otmp-mem-memory.md` (the note template) uses Templater syntax: `<% crypto.randomUUID() %>` for the id and `<% tp.date.now("YYYY-MM-DD") %>` for the dates. Before, it held `{{uuid}}` and `{{date}}`. Templater does not resolve them, and an older CLI stamped one fixed id and one fixed date into the installed copy. So every memory made from that copy had the same id.
- The note template points to the current agent template `tmp-mem-memory-v0.1` and holds no fixed id or date.
- It installs with `mode: force`. A CLI of NUU Flint Task 1055 replaces an unchanged copy, and keeps a changed copy with the next command `flint shard reinstall <alias> --replace-note-templates`.

---

# 0.1.0

- Initial shard scaffold
