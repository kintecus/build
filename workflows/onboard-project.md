# Workflow: Onboard New Project

## Usage

```
# In Claude Code, from pm-workspace root:
# 1. Create project folder and drop raw inputs
mkdir -p projects/{project-name}/raw
# copy PDFs and other inputs into raw/

# 2. Run onboarding
# Tell CC: "Onboard new project {name}. Process the raw inputs in projects/{name}/raw/, then interview me to fill in the brief."
```

## What this workflow does

1. **Scaffold** - Creates folder structure from templates (docs/, decisions/, comms/, INDEX files, STATUS.md, BRIEF.md)
2. **Process raw inputs** - Reads each file in raw/, generates a summary in docs/, updates INDEX.md
3. **Interview** - Asks targeted questions to fill in BRIEF.md. Covers: project background, goals, stakeholders, constraints, risks, your role. Uses processed docs as starting context so it doesn't ask about things it already knows.
4. **Update CLAUDE.md** - Adds the new project to the active projects list.

## Notes

- For PDFs: CC extracts text and creates a structured markdown summary. Original stays in raw/.
- The interview is conversational. Answer casually, CC structures the output.
- Review BRIEF.md after generation - it's a starting point, not final.
