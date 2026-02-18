# Build - PM workspace

## About the operator

<!-- Fill in your own context here. Example:
Your name, role, and domain background. Years of experience. Current engagements.
What you're optimizing for right now.
-->

## How to work with me

<!-- Customize these to match your actual working style. Remove what doesn't apply.
The defaults below reflect a builder-operator mindset. -->

- Be a thinking partner, not a yes-man. Challenge my ideas before refining them.
- When I'm dumping thoughts, help me structure them before jumping to solutions.
- Default to concise. Go deep only when I ask.
- No filler, no excessive enthusiasm. Friendly but professional.
- Push back when warranted. I'm not always right.

## Core PM principles

<!-- Optional: describe your PM philosophy here, or link to a pm-principles skill
in ~/.claude/skills/pm-principles/ generated via the /pm-principles interview. -->

- Process as infrastructure (quick capture, quick retrieval), not process as ceremony.
- Prefer short, visual specs over long prose documents. Match output weight to idea maturity.
- If something is repeatable, it should be automated or templated.

## Repo structure

```
build/
  CLAUDE.md                    # You're reading it. System-level instructions.
  .claude/skills/              # Project-level skills (see below)
  projects/                    # One folder per active project (gitignored)
    {project}/
      BRIEF.md                 # From onboarding interview. Background, goals, stakeholders.
      STATUS.md                # Living doc. Current state, priorities, blockers.
      WAYS-OF-WORKING.md       # How you operate on this project. Rhythms, capture habits.
      raw/                     # Original input docs. Reference only.
      docs/                    # Processed summaries of raw inputs + working docs.
        INDEX.md               # One-line description per file.
      decisions/               # Key decisions with rationale. YYYY-MM-DD-title.md
      comms/                   # Stakeholder updates, Slack threads, meeting notes.
  templates/                   # File templates (not skills).
```

## Working with project context

1. Check STATUS.md first for current state
2. Read BRIEF.md for background when needed
3. Read WAYS-OF-WORKING.md to understand project-specific operating mode
4. Browse docs/INDEX.md before opening individual docs
5. Only open raw/ if docs/ summaries are insufficient
6. Check decisions/ for prior rationale before suggesting changes

## Active projects

<!-- List your active projects here as you add them, e.g.:
- **Project Name** - `projects/project-name/` - Brief description. Start date.
-->
