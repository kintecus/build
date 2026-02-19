# Build - Setup

## Quick start

```bash
# 1. Clone the repo
git clone https://github.com/ostaps/build ~/code/build
cd ~/code/build

# 2. Install pre-commit (if not already installed)
pip install pre-commit
# or: brew install pre-commit

# 3. Install the markdown lint hook
pre-commit install

# 4. (Optional) Copy the personal PM principles skill to your home directory
#    First run the pm-principles interview to generate your profile:
#    /pm-principles
#    Then copy the output:
cp -r personal-skills/pm-principles ~/.claude/skills/pm-principles

# 5. Open CLAUDE.md and fill in your personal context (About the operator, How to work with me)

# 6. Onboard your first project
# Drop raw docs into projects/{project-name}/raw/
# /onboard-project {project-name}
```

## Markdownlint rules

`.markdownlint.json` and `.pre-commit-config.yaml` are included in this repo directly.

Config overview:

- **MD004** - enabled, dash style (`-`) for unordered lists
- **MD013** (line length) - off, templates have long lines
- **MD024** (duplicate headings) - off, templates reuse heading names
- **MD033** (inline HTML) - off, not relevant
- **MD036** (emphasis as heading) - off, we use bold for labels
- **MD040** (fenced code language) - off, some code blocks are templates
- **MD041** (first line h1) - off, YAML frontmatter comes first in skills
- **MD046** (code block style) - off, mixed styles in templates
- **MD060** (table column style) - off

Everything else stays on: heading structure, no trailing spaces, etc.

## Git hooks

`.githooks/pre-commit` checks that PlantUML diagram URLs in markdown files stay in sync with their source blocks. Installed automatically by `pre-commit install` if you configure git to use `.githooks/`:

```bash
git config core.hooksPath .githooks
```

## Skill authoring notes

**Do not use `disable-model-invocation: true` in skill frontmatter.** This flag blocks the `Skill` tool from invoking the skill programmatically - not just auto-triggering. The correct pattern is to write a precise `description` field that only matches explicit user intent. Adding "Only invoke when the user explicitly runs /skill-name" to the description is sufficient to prevent accidental auto-triggering.

## Structure

See `CLAUDE.md` for the full repo structure and how Claude Code interacts with it.
