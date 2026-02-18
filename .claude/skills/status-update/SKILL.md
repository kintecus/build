---
name: status-update
description: Generate a stakeholder status update from project context. Use when the user needs to write a status update, weekly report, or stakeholder communication about project progress.
---

# Status Update

Draft a stakeholder update from the project's current context.

## Context gathering

Before drafting, read:

1. The project's `STATUS.md` for current state
2. Recent files in `decisions/` for key decisions
3. Recent files in `comms/` for prior updates and context

## Output format

Draft a concise status update covering:

- **Progress**: What moved forward since the last update
- **Decisions**: Key decisions made and their rationale (brief)
- **Blockers/risks**: What's stuck or at risk, and what's being done about it
- **Next steps**: What's coming, with rough timeline

## Tone and style

- Professional but not corporate. Clear, specific, no fluff.
- Assume the audience is busy and technical.
- Lead with the most important information.
- Under 300 words unless the user says otherwise.
- No process theater - skip anything that doesn't inform or require action.

## After drafting

Present the draft for review. Ask if the tone, level of detail, and emphasis are right for the specific audience. Adjust based on feedback.

Save the final version to `comms/` with date prefix: `YYYY-MM-DD-status-update.md`.
