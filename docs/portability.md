# Portability

Build is not a Claude Code product. It's a set of markdown conventions plus skills written to the [Agent Skills](https://agentskills.io) open standard, arranged so that most of it survives a change of tool.

This doc is the honest version: what's portable, what isn't, and what you have to do per tool.

## The four layers

Portability decreases as you go down this list. The good news is that the top layer carries most of the value.

| Layer | Portable to | Why |
|---|---|---|
| Conventions - `BRIEF`/`STATUS`/`WAYS-OF-WORKING`/`DECISION`, `projects/` structure | Anything that reads markdown | It's just markdown in folders. No tool feature involved. |
| Skills - the 8 in `.claude/skills/` | ~40 agents | [Agent Skills](https://agentskills.io) standard. Required frontmatter is `name` + `description`; nothing here uses vendor extensions. |
| Instruction file - `AGENTS.md` | The [AGENTS.md](https://agents.md) readers + Claude Code | Canonical file is `AGENTS.md`; `CLAUDE.md` symlinks to it. |
| Extras - auto-memory, plugins, `.githooks/` | Claude Code, mostly | These lean on Claude Code specifics. Swap or drop them. |

## Tested vs. assumed

Be skeptical of unqualified "works everywhere" claims, including mine.

| Tool | Status | Notes |
|---|---|---|
| Claude Code | **Tested** | Daily driver. Everything works, including the extras. |
| opencode | **Tested** | All 8 skills load unmodified. Verified with `opencode debug skill` (v1.17.18). opencode reads `.claude/skills` *and* `.agents/skills` natively and dedupes the overlap. |
| Codex | Assumed | Follows the same published spec. Scans `.agents/skills` only - which is why the symlink exists. Not personally run end-to-end. |
| Cursor, Gemini CLI, Goose, Copilot, pi, others | Assumed | On the [Agent Skills client list](https://agentskills.io/clients). Same spec, untested here. |
| Claude Cowork | Partial | Runs SKILL.md, but has no terminal. Document skills work; `git-week` does not. |
| ChatGPT (web) | Conventions only | Skills are a Codex feature, available in the ChatGPT desktop app - not the ChatGPT web product. The markdown conventions still work by hand. |

Found a discrepancy? Open an issue. Real reports beat my assumptions.

## Why skills live in two places

Discovery conventions differ, and no spec mandates one:

- Claude Code reads `.claude/skills/`.
- Codex reads `.agents/skills/` and never `.claude/skills/`.
- opencode reads both, plus `.opencode/skills/`.

So the files live once at `.claude/skills/`, and `.agents/skills` is a symlink pointing at them. One copy, two discovery paths, no drift.

```bash
# what's checked in
.claude/skills/          # real directory
.agents/skills -> ../.claude/skills
```

If your tool wants a third location, add another symlink rather than copying the folder.

## Why the instruction file is AGENTS.md

`AGENTS.md` is the cross-tool convention, read by Codex, opencode, Cursor, and ~20 others. Claude Code is the holdout: per [Anthropic's own docs](https://code.claude.com/docs/en/memory#agents-md), *"Claude Code reads `CLAUDE.md`, not `AGENTS.md`."* The same page documents the fix, which is what this repo does:

```bash
ln -s AGENTS.md CLAUDE.md
```

Edit `AGENTS.md`. Never edit `CLAUDE.md` - it's a symlink, and editing through it just writes to `AGENTS.md` anyway.

### Windows

Symlinks need Administrator privileges or Developer Mode. If that's a problem, delete the symlink and use a real `CLAUDE.md` that imports the canonical file:

```markdown
@AGENTS.md
```

Anthropic documents this import form as the Windows-friendly alternative. It also gives you somewhere to put Claude-specific instructions that shouldn't reach other tools - anything below the import line is Claude Code only.

### Per-project persona files

The same rule applies inside `projects/{name}/`. Claude Code discovers nested `CLAUDE.md` files on demand when it reads files in a subdirectory, but it won't pick up a nested `AGENTS.md`. So each project that needs a persona gets both:

```
projects/my-project/
  AGENTS.md              # the real file
  CLAUDE.md -> AGENTS.md # symlink
```

`/onboard-project` creates both for you.

## The Claude Code specific parts

None of these are load-bearing. The framework works without them; you lose convenience, not structure.

| Thing | What it depends on | What to do elsewhere |
|---|---|---|
| Auto-memory (`docs/memory.md`) | Claude Code's `~/.claude/projects/<slug>/memory/` | Most agents have some persistent-memory feature. The *conventions* in that doc (four types, why-lines, terse index) port to any of them. If yours has none, keep the same notes in a checked-in file. |
| Plugins / `SessionStart` hooks (`docs/plugin-ecosystem.md`) | Claude Code's plugin + hook system | opencode has plugins, Codex has its own hook surface. The pattern - a small script that prints standing rules at session start - is general even where the wiring isn't. |
| `.githooks/` diagram validation | Nothing agent-specific | Plain git hooks. Works anywhere git does. |
| `/skill-name` invocation | Per-tool slash command syntax | Most tools expose skills by slash command or let the model auto-trigger from `description`. Worst case, say "use the onboard-project skill". |

## Porting checklist

Moving the workspace to a different agent:

1. Clone the repo. `AGENTS.md` and `.agents/skills` are already in place.
2. Check your tool's skill discovery path. If it's neither `.claude/skills` nor `.agents/skills`, add a symlink.
3. Confirm the skills loaded. opencode: `opencode debug skill`. Others: ask the agent to list its available skills.
4. Drop or replace the extras above that your tool doesn't have.
5. Fill in `AGENTS.md` and run `/onboard-project`.
