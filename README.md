# Build

A Claude Code-first operating system for product and program managers. Project templates, skills, workflows, and a worked case study - everything you need to run real consulting projects with Claude as a collaborator.

Not a personal vault. A shareable harness.

## The concept

Most PM tooling is either too heavyweight (Notion wikis, Confluence labyrinths) or too personal to share. Build is neither - it's a Claude Code workspace you clone once and shape to your style, then use across every project.

The harness is designed to be tailored, not just used as-is:

- **Setup** installs your tooling and markdown conventions.
- **`/pm-principles`** runs an interview to generate a personal PM principles skill, codifying how you think about product work.
- **`CLAUDE.md`** is where you fill in your operator context - role, background, how you like to work.
- **`WORKSPACE.md`** explains the workspace pattern: launch Claude once from the repo root, hold every project in a single context.

For each project, Claude operates inside a structured folder of markdown files (`BRIEF.md`, `STATUS.md`, `WAYS-OF-WORKING.md`, optional per-project `CLAUDE.md`) and raw inputs you drop in. The `/onboard-project` interview turns those inputs into a living context that Claude reads before acting.

Result: Claude behaves like a PM collaborator who knows your style and has read every relevant doc, not a generic assistant starting cold.

## What's in the box

**Context layer**

- `CLAUDE.md` - workspace-level system prompt. Fill in your personal context and working style.
- `WORKSPACE.md` - explains the layered CLAUDE.md model (personal → workspace → per-project).

**Templates** (`templates/`)

- `BRIEF.md`, `STATUS.md`, `WAYS-OF-WORKING.md`, `DECISION.md` - project file templates.
- `PROJECT-CLAUDE.md` - per-project agent persona template.

**Skills** (`.claude/skills/`)

- `onboard-project` - structured interview that creates a project context folder.
- `ideation` - turns brain dumps into structured specs or PRDs.
- `critique-idea` - steel-mans then pokes holes.
- `status-update` - generates stakeholder updates from project context.
- `process-document` - processes raw input materials into `.md` summaries.
- `tune-persona` - sets or updates the per-project agent persona.
- `git-week` - lightweight weekly git workflow helper (branches, commits, activity review).

**Reference docs** (`docs/`)

- `memory.md` - how to use Claude Code's auto-memory system across sessions.
- `obsidian-integration.md` - how to wire this workspace into an Obsidian vault.
- `plugin-ecosystem.md` - SessionStart hooks and plugins the author uses (swap freely).
- `skills-catalog.md` - full inventory of bundled and referenced skills.

**Worked example** (`examples/`)

- `adscale-case-study/` - a real 3-month ad tech consulting project, fully anonymized. Shows what the framework produces in practice: BRIEF, STATUS, decisions, specs, daily notes, stakeholder comms, retrospectives.

**Tooling**

- `.githooks/` - pre-commit hook for PlantUML diagram validation.
- `.markdownlint.json` + `.pre-commit-config.yaml` - markdown linting.

## Setup

See [SETUP.md](SETUP.md).

## Usage

Projects live in `projects/` (gitignored). Start a new project:

```
/onboard-project {project-name}
```

To see what a fully developed project looks like, browse `examples/adscale-case-study/`.

## What's new in v2

If you saw the earlier version of this repo, the structural changes are:

1. **`CLAUDE.md` is now workspace-level**, not per-project. The per-project version moved to `templates/PROJECT-CLAUDE.md`.
2. **New `WORKSPACE.md`** explains why Claude should run from the workspace root, not each project folder.
3. **New `docs/`** section covers the auto-memory system, Obsidian integration, and the plugin ecosystem.
4. **Two new skills** ported from live use: `tune-persona` and `git-week`.
5. **`templates/WAYS-OF-WORKING.md`** added (the triad of BRIEF/STATUS/WAYS was referenced but the template was missing).
6. **`examples/adscale-case-study/`** - a fully anonymized real project, included inline so you can see the framework in action.

## Philosophy

- Process as infrastructure, not ceremony.
- Match output weight to idea maturity.
- If something is repeatable, automate or template it.
- Quick capture, quick retrieval - your notes should help your future self, not impress anyone.

## License

See [LICENSE](LICENSE).
