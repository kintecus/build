---
name: tune-persona
description: Set or update the agent persona for a specific project. Use when the user wants to tune how Claude behaves on a project - archetype, document standards, decision rigor, communication style. Works on existing projects with or without a CLAUDE.md.
---

# Tune persona

Adjust Claude's agent persona for a specific project by creating or updating the project-level `CLAUDE.md`.

## Usage

```
/tune-persona {project-name}
```

If no project name is given, ask which active project to tune (check the workspace `CLAUDE.md` for the list).

## Workflow

### Step 1: Load current state

Read `projects/{project-name}/CLAUDE.md` if it exists. If not, note that we're creating one from scratch.

Also read `projects/{project-name}/BRIEF.md` and `projects/{project-name}/WAYS-OF-WORKING.md` for context about the project.

### Step 2: Interview

Ask the PM about desired agent behavior. Use what you know from the project docs to suggest defaults. Ask 2-3 questions at a time using `AskUserQuestion`.

**Areas to cover:**

1. **Archetype**: What kind of PM partner do you need? (senior TPM, product strategist, program manager, builder-operator, or describe your own)
2. **Document standards**: What bar should artifacts meet? (Amazon 6-pager precision, lean one-pagers, working notes, mixed depending on audience)
3. **Decision rigor**: How thoroughly should decisions be tracked? (every decision logged, key decisions only, lightweight capture in STATUS.md)
4. **Communication style**: Tone and audience? (formal/precise, casual/direct, depends on artifact type)

If the project already has a `CLAUDE.md` with a persona, show the current settings and ask what to change rather than re-interviewing from scratch.

### Step 3: Generate or update

- If no `CLAUDE.md` exists: create one using the `templates/PROJECT-CLAUDE.md` template, populated from interview answers.
- If `CLAUDE.md` exists: update the relevant sections, preserving any project-specific rules the PM added manually.

### Step 4: Review

Present the full `CLAUDE.md` for approval. Iterate based on feedback.

## Notes

- Keep persona descriptions concrete and actionable, not abstract. "Log every decision with context and alternatives" is better than "be rigorous."
- The persona should complement, not contradict, the workspace-level `CLAUDE.md`. Project rules override workspace defaults where they conflict.
- If the PM can name a specific person or role model ("like the best Amazon TPM I worked with"), capture what made that person effective in concrete behavioral terms.
