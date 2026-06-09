# Build - Setup

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
# Open CLAUDE.md and replace the placeholder sections:
#   - About the operator
#   - How to work with me
#   - Active projects

# 6. (Optional) Add a shell alias so you always launch Claude from the workspace
# Add to ~/.zshrc or ~/.bashrc:
#   alias build='cd ~/code/build && claude'

# 7. Onboard your first project
# Drop raw docs into projects/{project-name}/raw/, then run:
#   /onboard-project {project-name}
```

## Optional integrations

**Obsidian vault integration** - if you work out of Obsidian, see `docs/obsidian-integration.md` for how to wire `obsidian-cli` into this workspace and surface your notes to Claude.

**Auto-memory** - Claude Code maintains typed memories (user/feedback/project/reference) across sessions. See `docs/memory.md` for the conventions this workspace follows.

**Plugin ecosystem** - opinionated defaults the author uses for semver, branch naming, diagram validation, and retrospectives. See `docs/plugin-ecosystem.md` for the list and how to swap or disable individual plugins.

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

**Do not use `disable-model-invocation: true` in skill frontmatter.** This flag blocks the `Skill` tool from invoking the skill programmatically - not just auto-triggering. The correct pattern is to write a precise `description` field that only matches explicit user intent. Adding "Only invoke when the user explicitly runs /skill-name" to the description is sufficient to prevent accidental auto-triggering.

## Structure

See `CLAUDE.md` for the workspace structure. See `WORKSPACE.md` for why it's set up this way.
