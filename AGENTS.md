# Build - Workspace instructions

This file is loaded by your coding agent when you launch it from the workspace root. It tells the agent who you are, how you work, and how the workspace is laid out.

`AGENTS.md` is the canonical file - it's the open instruction-file convention read by Codex, opencode, Cursor, and ~20 other tools. `CLAUDE.md` is a symlink to this file, so Claude Code picks up the same content. Edit `AGENTS.md`; never edit `CLAUDE.md` directly.

Treat this as a template. Replace the placeholder blocks with your own content. Anything between `<!-- -->` is guidance, not output.

## About the operator

<!-- Fill in your context here. Example:
Your name, role, and domain background. Years of experience. Current engagements.
What you're optimizing for right now (shipping, learning, hiring, exiting, etc.).

The richer this section, the better the agent can tailor outputs to your voice and situation. -->

## How to work with me

<!-- Customize these to match your actual working style. Remove what doesn't apply.
The defaults below reflect a builder-operator mindset. -->

- Be a thinking partner, not a yes-man. Challenge my ideas before refining them.
- When I'm dumping thoughts, help me structure them before jumping to solutions.
- Default to concise. Go deep only when I ask.
- Push back when warranted. I'm not always right.
- No filler, no excessive enthusiasm. Friendly but professional.
- Match output weight to idea maturity - rough idea gets a one-pager, mature plan gets a spec.

## Core PM principles

<!-- Optional: describe your PM philosophy here, or link to a pm-principles skill
in your personal global skills directory (~/.agents/skills/pm-principles/ or
~/.claude/skills/pm-principles/, depending on your agent). -->

- Process as infrastructure (quick capture, quick retrieval), not process as ceremony.
- Prefer short, visual specs over long prose documents.
- If something is repeatable, automate or template it.

## Workspace layout

This workspace is multi-project. Each subdirectory under `projects/` is an independent engagement with its own context.

```
build/
  AGENTS.md                    # You are here. Workspace-level instructions.
  CLAUDE.md                    # Symlink -> AGENTS.md, for Claude Code.
  WORKSPACE.md                 # Explains the layered instruction-file model and why the agent runs from here.
  .claude/skills/              # Workspace-level skills (Agent Skills standard).
  .agents/skills               # Symlink -> ../.claude/skills, for Codex and other .agents/ readers.
  templates/                   # File templates. Core: BRIEF, STATUS, WAYS-OF-WORKING, DECISION, PROJECT-AGENTS.
                               #   Optional: STRATEGY, EXPERIMENT, CONTEXT-BRIDGE, FREELANCE-ENGAGEMENT.
  docs/                        # Reference docs (portability, memory, obsidian, plugins, skills catalog).
  examples/                    # Worked examples - real engagements anonymized for reference.
  projects/                    # Active engagements (gitignored).
    {project}/
      AGENTS.md                # Optional. Per-project agent persona.
      CLAUDE.md                # Symlink -> AGENTS.md (Claude Code only reads CLAUDE.md, incl. nested).
      BRIEF.md                 # From onboarding interview. Background, goals, stakeholders.
      STATUS.md                # Living doc. Current state, priorities, blockers.
      WAYS-OF-WORKING.md       # Project rhythms, capture habits.
      STRATEGY.md              # Optional. Durable vision, moat, roadmap (when STATUS isn't enough).
      EXPERIMENT.md            # Optional. Thesis-under-test + verdict (when the project tests a method).
      CONTEXT-BRIDGE.md        # Optional. Map to a separate engineering repo (when code lives elsewhere).
      raw/                     # Original input docs - drop zone, never edited.
      docs/                    # Processed summaries + working docs.
        INDEX.md
      decisions/               # YYYY-MM-DD-title.md
      comms/                   # Stakeholder updates, meeting notes, daily notes.
```

See `WORKSPACE.md` for the full reasoning behind this structure.

## Working with project context

When the agent enters any `projects/{name}/` folder, follow this order:

1. Read `projects/{name}/AGENTS.md` if present - it sets the agent persona for this project. (Claude Code reaches the same content through the `CLAUDE.md` symlink beside it.)
2. Check `STATUS.md` for current state.
3. Read `BRIEF.md` for background when needed.
4. Read `WAYS-OF-WORKING.md` to understand project-specific operating mode.
5. Browse `docs/INDEX.md` before opening individual docs.
6. Never read `raw/` files directly for context - use processed `docs/` summaries instead. If no processed version exists, run `/process-document` on the raw file first.
7. Check `decisions/` for prior rationale before suggesting changes.
8. When creating or modifying files in `docs/` or `decisions/`, update `docs/INDEX.md` in the same turn.

## Active projects

<!-- Add your active projects as you onboard them, e.g.:
- **AdScale forecasting** - `projects/adscale/` - Programmatic ad tech forecasting service. Jan-Mar 2026.
- **Personal site rewrite** - `projects/site/` - Hugo -> Astro migration. Q2 2026.

For a worked example of a fully populated project, see `examples/adscale-case-study/`.
-->
