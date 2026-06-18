---
description: "Recall memory — load the INDEX and disclose matching active notes; search logs only on explicit request"
---

> [!important] THIS FILE IS AN INSTRUCTION. WHEN REFERENCED IT IS MEANT TO BE TAKEN AS AN ACTION.

Run `flint shard start mem` if you haven't already.

# Skill: Recall

Surface the right memory for the task at hand using progressive disclosure — cheap index first, detail on demand.

# Input

- The current task or question
- (Optional) `--history` — also search logged memories (off by default)

# Actions

1. **Read the index.** Open `Mesh/Memories/(Memory) INDEX.md`. This is the cheap, always-available Tier-1 hub listing active memories with high-scent descriptions.
2. **Match.** Pick the index lines whose description matches the task. If nothing matches, stop — there is no relevant memory (do not load notes speculatively; that defeats progressive disclosure).
3. **Disclose on demand.** Open only the matching `(Memory) NNN <Topic>.md` notes (Tier 2). Follow a note's outbound links (Tier 3) only if it points at detail you actually need.
4. **History (only if asked).** If `--history` was passed or the task is explicitly about the past, also `grep` logged notes (`state: log`) in `Mesh/Memories/`. Otherwise leave logs out of context.
5. **Use, don't dump.** Apply the recalled facts to the task. Surface to the user only what's relevant.

# Output

- The relevant active memories loaded into context (and logs only when history was requested)
