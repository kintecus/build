# Build

A Claude Code-first PM operating system. Project templates, skills, prompts, and workflows that give a TPM a standing start on any engagement.

Not a personal vault - a shareable harness.

## What's in the box

- **CLAUDE.md** - system prompt for Claude Code; fill in your personal context
- **templates/** - BRIEF, STATUS, DECISION file templates
- **.claude/skills/** - project-level skills: onboard-project, ideation, critique-idea, status-update, process-document
- **prompts/** - prompt templates for common TPM tasks
- **workflows/** - workflow docs for repeatable processes
- **.githooks/** - pre-commit hook for PlantUML diagram validation
- **.markdownlint.json** - markdown linting config
- **.pre-commit-config.yaml** - pre-commit hook config

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
