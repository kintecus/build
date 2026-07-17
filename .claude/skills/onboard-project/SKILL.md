---
name: onboard-project
description: Scaffold a new project, process raw input documents, interview the PM to capture context, and generate BRIEF.md, STATUS.md, and WAYS-OF-WORKING.md. Only invoke when the user explicitly runs /onboard-project or asks to onboard a new project by name.
---

# Onboard Project

Set up a new project in the PM workspace through a structured onboarding flow.

## Usage

```
/onboard-project {project-name}
```

## Workflow

### Step 1: Scaffold

Create the project folder structure under `projects/{project-name}/`:

```
{project-name}/
  raw/
  docs/
    INDEX.md
  decisions/
  comms/
```

If `raw/` already contains files, note them for processing in Step 2.

### Step 2: Process raw inputs

For each file in `raw/`:

1. Read the file (PDF, text, export, etc.)
2. Generate a structured markdown summary following the process-document skill
3. Save to `docs/` with a matching name
4. Update `docs/INDEX.md`

If `raw/` is empty, skip to Step 3.

### Step 3: Interview

Conduct a structured interview to populate the project brief. Use processed docs as starting context so you don't ask about things you already know.

**Interview areas** (adapt based on what you already learned from docs):

1. **Project basics**: What is this project? Why does it exist?
2. **Goals and success**: What does good look like? How will we know?
3. **Stakeholders**: Key people, roles, what they care about, political dynamics.
4. **Constraints**: Time, budget, tech, organizational limits.
5. **Risks**: What worries you most?
6. **Your role**: What are you responsible for? What authority do you have?
7. **Ways of working**: How will you operate day-to-day on this project? What's your rhythm? What tools? How do you want to capture and track things?
8. **Agent persona**: What kind of PM partner do you need on this project? Think about: archetypes (senior TPM, product strategist, program manager, builder-operator), document standards (Amazon 6-pager precision vs. lean one-pagers), decision rigor (log everything vs. only key calls), communication style (formal/precise vs. casual/fast). If the PM isn't sure, suggest an archetype based on what you've learned about the project so far.

Ask 2-3 questions at a time. Don't overwhelm. Let the conversation flow naturally. Use processed docs to pre-fill what you can and validate with the PM rather than asking from scratch.

### Step 4: Generate docs

From interview outputs, create:

1. `BRIEF.md` using [brief-template.md](references/brief-template.md)
2. `STATUS.md` using [status-template.md](references/status-template.md)
3. `WAYS-OF-WORKING.md` using [ways-of-working-template.md](references/ways-of-working-template.md)
4. `AGENTS.md` using the project persona template (`templates/PROJECT-AGENTS.md`), populated from the agent persona interview answers. This file tunes the agent's behavior when working in this project directory.

   Then add a `CLAUDE.md` symlink beside it, because Claude Code discovers nested `CLAUDE.md` files but not nested `AGENTS.md`:

   ```bash
   ln -s AGENTS.md projects/{name}/CLAUDE.md
   ```

   On Windows, where symlinks need elevated privileges, write a `CLAUDE.md` containing `@AGENTS.md` instead.

Present each for review. Iterate based on feedback.

### Step 5: Update workspace

1. Add the new project to the "Active projects" section in the workspace `AGENTS.md` (at the repo root).
2. Confirm that the project-level `AGENTS.md` was generated with the persona configuration, and that its `CLAUDE.md` symlink resolves. (To adjust the persona later, invoke the `tune-persona` skill.)

## Notes

- The interview is conversational. Accept casual answers, structure the output.
- If the PM says "I don't know yet" to something, note it as an open question rather than pushing.
- BRIEF.md is a snapshot. It doesn't need to be perfect - it gets refined as the project evolves.
- WAYS-OF-WORKING.md should reflect reality, not aspiration. What will actually happen day-to-day.
