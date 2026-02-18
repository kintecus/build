# Build

A Claude Code-first PM operating system. Project templates, skills, prompts, and workflows that give a TPM a standing start on any engagement.

Not a personal vault - a shareable harness.

## The concept

Most PM tooling is either too heavyweight (Notion wikis, Confluence labyrinths) or too personal to share. Build is neither - it's a Claude Code workspace you clone once and shape to your style, then use across every engagement.

The harness is designed to be tailored, not just used as-is:

- **Setup** installs your tooling and markdown conventions
- **`/pm-principles`** runs an interview to generate a personal PM principles skill, codifying how you think about product work
- **`CLAUDE.md`** is where you fill in your operator context - role, background, how you like to work

For each project, Claude operates within a structured folder of markdown files (`BRIEF.md`, `STATUS.md`, `WAYS-OF-WORKING.md`) and raw inputs you drop in. The `/onboard-project` interview turns those inputs into a living context that Claude reads before acting.

The result: Claude behaves like a PM collaborator who knows your style and has read every relevant doc, not a generic assistant starting cold.

## What's in the box

**Context layer**

- **`CLAUDE.md`** - system prompt for Claude Code; fill in your personal context and working style

**Skills** (`.claude/skills/`)

- `onboard-project` - structured interview that creates a project context folder
- `ideation` - turns brain dumps into structured specs or PRDs
- `critique-idea` - steel-mans then pokes holes
- `status-update` - generates stakeholder updates from project context
- `process-document` - processes raw input materials (can be added as projects evolve) in .md docs

**Templates** (`templates/`)

- `BRIEF.md`, `STATUS.md`, `DECISION.md` - project file templates

**Prompts and workflows**

- `prompts/` - prompt templates for common TPM tasks
- `workflows/` - workflow docs for repeatable processes

**Tooling**

- `.githooks/` - pre-commit hook for PlantUML diagram validation
- `.markdownlint.json` + `.pre-commit-config.yaml` - markdown linting

## Setup

See [SETUP.md](SETUP.md).

## Usage

Projects live in `projects/` (gitignored). Start a new engagement:

```
/onboard-project {project-name}
```

## Philosophy

- Process as infrastructure, not ceremony
- Match output weight to idea maturity
- If something is repeatable, automate or template it
