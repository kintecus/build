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

Ask 2-3 questions at a time. Don't overwhelm. Let the conversation flow naturally. Use processed docs to pre-fill what you can and validate with the PM rather than asking from scratch.

### Step 4: Generate docs

From interview outputs, create:

1. `BRIEF.md` using [brief-template.md](references/brief-template.md)
2. `STATUS.md` using [status-template.md](references/status-template.md)
3. `WAYS-OF-WORKING.md` using [ways-of-working-template.md](references/ways-of-working-template.md)

Present each for review. Iterate based on feedback.

### Step 5: Update CLAUDE.md

Add the new project to the "Active projects" section in the root CLAUDE.md.

## Notes

- The interview is conversational. Accept casual answers, structure the output.
- If the PM says "I don't know yet" to something, note it as an open question rather than pushing.
- BRIEF.md is a snapshot. It doesn't need to be perfect - it gets refined as the project evolves.
- WAYS-OF-WORKING.md should reflect reality, not aspiration. What will actually happen day-to-day.
