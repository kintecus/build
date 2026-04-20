# Auto-memory

Claude Code ships a per-project memory directory that persists across sessions. This doc explains how the workspace uses it and what conventions keep memory useful over time.

## Where memory lives

Memory files live outside your repo, under:

```
~/.claude/projects/<workspace-path-slug>/memory/
```

The `<workspace-path-slug>` is an encoded version of your workspace directory - for example, `~/code/build` becomes `-Users-youruser-code-build`. You never edit this path manually; Claude Code manages it.

Inside the `memory/` directory:

- `MEMORY.md` - a hand-tuned index. One line per memory file.
- `<type>_<topic>.md` - individual memory files, one per concept.

## Four types of memory

Use the prefix on the filename to indicate type:

| Prefix | What it stores | Example |
|---|---|---|
| `user_` | Things about the operator: role, preferences, domain background | `user_ai_tools_advocacy.md` |
| `feedback_` | Guidance the operator has given about how to approach work (both corrections and confirmations of non-obvious choices) | `feedback_week_dates.md` |
| `project_` | Ongoing state about active work: who's doing what, why, by when | `project_adscale_phase_2_frozen.md` |
| `reference_` | Pointers to external systems where information lives | `reference_confluence_publishing.md` |

## What NOT to save

- Code patterns, conventions, architecture - these can be derived by reading the repo.
- Git history or "who changed what" - `git log` / `git blame` are authoritative.
- Fix recipes for specific bugs - the fix is in the code; the commit message has the context.
- Anything already documented in `CLAUDE.md`.
- Ephemeral task state.

Memory is for things that will *still be useful in a future conversation*. If it won't, it's clutter.

## Memory file structure

Every memory file has frontmatter:

```markdown
---
name: {memory name}
description: {one-line description - used to decide relevance}
type: {user | feedback | project | reference}
---

{Memory content.}
```

For `feedback` and `project` types, structure the body as:

```markdown
{The rule or fact itself.}

**Why:** {The reason - often a past incident or strong preference.}
**How to apply:** {When and where this guidance kicks in.}
```

The `Why` line matters most. It lets future-you (and future-Claude) judge edge cases instead of following the rule blindly.

## MEMORY.md index

`MEMORY.md` is loaded into every Claude session's context. Keep it terse - one line per memory, under ~150 characters:

```markdown
# Memory

## Topic group

- [Title](file.md) — one-line hook
```

The index is organized by topic, not chronology. Group related memories together.

## When Claude should save memory

Save when you learn something durable:

- Operator corrects your approach ("no, not that")
- Operator confirms a non-obvious approach worked ("yes, keep doing that")
- You discover a fact about an active engagement (stakeholder role, deadline, external tool mapping)

Save immediately - don't batch. A rule the operator stated ten minutes ago is already at risk of being misremembered.

## When Claude should access memory

- When the operator references prior-conversation work ("remember when we talked about...")
- When you're about to make a choice that memory might have guided before
- When the operator explicitly asks ("check memory", "what do you know about X")

Before acting on a recalled memory, verify it's still current. Memories decay. A memory that named a specific file or flag may be stale - grep the code before recommending from memory.
