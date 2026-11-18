# Build - Setup

Works with any agent that supports the [Agent Skills](https://agentskills.io) and [AGENTS.md](https://agents.md) standards. See [docs/portability.md](docs/portability.md) for what's tested versus assumed, and for per-tool details.

## Quick start

```bash
# 1. Clone the repo
git clone https://github.com/kintecus/build ~/code/build
cd ~/code/build

# 2. Install pre-commit (if not already installed)
pip install pre-commit
# or: brew install pre-commit

# 3. Install the markdown lint hook
pre-commit install

# 4. (Optional) Wire the diagram validators (PlantUML + Mermaid)
git config core.hooksPath .githooks

# 5. Fill in your context
# Open AGENTS.md and replace the placeholder sections:
#   - About the operator
#   - How to work with me
#   - Active projects
# (CLAUDE.md is a symlink to AGENTS.md - don't edit it directly.)

# 6. (Optional) Add a shell alias so you always launch from the workspace
# Add to ~/.zshrc or ~/.bashrc - point it at whichever agent you use:
#   alias build='cd ~/code/build && claude'
#   alias build='cd ~/code/build && codex'
#   alias build='cd ~/code/build && opencode'

# 7. Onboard your first project
# Drop raw docs into projects/{project-name}/raw/, then run:
#   /onboard-project {project-name}
```

## Agent setup

Nothing to configure for most tools - the repo ships both skill-discovery paths and both instruction-file names.

| Your agent | Instruction file | Skills found at | Action needed |
|---|---|---|---|
| Claude Code | `CLAUDE.md` (symlink) | `.claude/skills/` | None |
| Codex | `AGENTS.md` | `.agents/skills/` (symlink) | None |
| opencode | `AGENTS.md` | reads both, dedupes | None |
| Cursor, Gemini CLI, Goose, Copilot, others | `AGENTS.md` | varies | Check your tool's skill path; add a symlink if it's neither |
| Claude Cowork | n/a | upload skills manually | Document skills only - no terminal, so `git-week` won't work |

Verify skills loaded before your first real session. In opencode:

```bash
opencode debug skill
```

In other tools, ask the agent to list its available skills. You should see the eight bundled ones.

**Windows:** the repo uses symlinks for `CLAUDE.md` and `.agents/skills`, which need Administrator privileges or Developer Mode. If you'd rather not enable either, delete `CLAUDE.md` and create a regular file containing `@AGENTS.md`, and copy `.claude/skills` to `.agents/skills` instead of linking. See [docs/portability.md](docs/portability.md#windows).

## Optional integrations

**Obsidian vault integration** - if you work out of Obsidian, see `docs/obsidian-integration.md` for how to wire the official Obsidian CLI and agent skills into this workspace and surface your notes to Claude.

**Auto-memory** - Claude Code maintains typed memories (user/feedback/project/reference) across sessions. See `docs/memory.md` for the conventions this workspace follows. Other agents have their own memory features; the conventions port even where the wiring doesn't.

**Plugin ecosystem** - opinionated defaults the author uses for semver, branch naming, diagram validation, and retrospectives. Claude Code specific. See `docs/plugin-ecosystem.md` for the list and how to swap or disable individual plugins.

## Markdownlint rules

`.markdownlint.json` and `.pre-commit-config.yaml` are included.

Config overview:

- **MD004** - enabled, dash style (`-`) for unordered lists.
- **MD013** (line length) - off, templates have long lines.
- **MD024** (duplicate headings) - off, templates reuse heading names.
- **MD033** (inline HTML) - off, not relevant.
- **MD036** (emphasis as heading) - off, we use bold for labels.
- **MD040** (fenced code language) - off, some code blocks are templates.
- **MD041** (first line h1) - off, YAML frontmatter comes first in skills.
- **MD046** (code block style) - off, mixed styles in templates.
- **MD060** (table column style) - off.

Everything else stays on: heading structure, no trailing spaces.

## Git hooks

`.githooks/pre-commit` keeps PlantUML diagram URLs in sync with their source blocks and validates Mermaid diagram syntax in markdown files. Enable with:

```bash
git config core.hooksPath .githooks
```

## Skill authoring notes

Skills follow the [Agent Skills specification](https://agentskills.io/specification): a folder with a `SKILL.md` whose frontmatter needs only `name` (matching the folder) and `description`. Stick to those two fields and your skill stays portable across every compatible tool. Validate with `skills-ref validate ./my-skill`.

**Do not use `disable-model-invocation: true` in skill frontmatter.** This Claude Code flag blocks the `Skill` tool from invoking the skill programmatically - not just auto-triggering. It's also not part of the open spec, so other agents ignore it. The correct pattern is a precise `description` that only matches explicit user intent. Adding "Only invoke when the user explicitly runs /skill-name" to the description is sufficient to prevent accidental auto-triggering.

## Structure

See `AGENTS.md` for the workspace structure. See `WORKSPACE.md` for why it's set up this way. See `docs/portability.md` for what runs where.
