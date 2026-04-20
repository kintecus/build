# Workflow: Onboard New Project

## Usage

```
# In Claude Code, from the workspace root:
# 1. Create project folder and drop raw inputs
mkdir -p projects/{project-name}/raw
# copy PDFs, transcripts, and other inputs into raw/

# 2. Run onboarding
/onboard-project {project-name}
```

## What this workflow does

1. **Scaffold** - Creates folder structure (`docs/`, `decisions/`, `comms/`, `INDEX.md`, `STATUS.md`, `BRIEF.md`, `WAYS-OF-WORKING.md`) from `templates/`.
2. **Process raw inputs** - Reads each file in `raw/`, generates a summary in `docs/`, updates `INDEX.md`.
3. **Interview** - Asks targeted questions to fill in BRIEF, STATUS, and WAYS-OF-WORKING. Covers: project background, goals, stakeholders, constraints, risks, your role, operating rhythm. Uses processed docs as starting context so it doesn't ask about things it already knows.
4. **Update CLAUDE.md** - Adds the new project to the "Active projects" section in the workspace `CLAUDE.md`.
5. **(Optional) Tune persona** - Offers to create a per-project `CLAUDE.md` via the `tune-persona` skill. Do this when the project needs Claude to behave noticeably differently from the workspace default.

## Notes

- For PDFs: CC extracts text and creates a structured markdown summary. Original stays in `raw/`.
- The interview is conversational. Answer casually, CC structures the output.
- Review `BRIEF.md` after generation - it's a starting point, not final.
- For a worked example of what a fully onboarded project looks like, see `examples/adscale-case-study/`.
