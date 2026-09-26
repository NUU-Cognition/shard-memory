# Migrations

Migration notes for Memory. Latest version at top, separated by `---`.

# 0.1.3

- No migration step. The install replaces `Shards/(Shards) Obsidian Templates/otmp-mem-memory.md` when the lock records it and nobody changed it.
- An older Flint can hold a copy that the lock does not record (an install with `mode: once`). That copy can have a fixed `id` and a fixed `created` date. The install keeps it and prints `Kept note template … Next: flint shard reinstall <alias> --replace-note-templates`. Run that command: it saves a backup of the old copy and installs the new note template. The Flint migration of 0.7.0 does not change note templates (NUU Flint Task 1055, decision S33 of `(Spec) Flint Obsidian`). The rollout of 0.7.0 does it in each Flint: run `flint shard update`, then run `flint shard reinstall <alias> --replace-note-templates`.
- Memories made from an old copy can share one `id`. Do not change them in the migration. Find them with a search for the fixed id of the old copy, and give each one a new id only after a review.
