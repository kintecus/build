---
date created: 2026-02-18 20:59
updated: 2026-03-16 09:30
---

# AdScale - Ways of working

> How I actually operate on this project. Practical, not aspirational.

## Operating mode

Fractional TPM (0.5 FTE, ~20 hrs/week) splitting time with a second engagement. Hands-on delivery manager + scrum master, not strategic PM. Builder-operator mode: close to technical implementation, using Claude as force multiplier for documentation, synthesis, and deliverable formatting. Kanban.

Weight of process: lightweight. Weekly syncs, Kanban board, status reports. No heavy ceremony - the team is small and communicates well async.

## Weekly rhythm

- **Weekly sync with AdScale** - standing call (Wednesdays). Ownership is blurred - CLIENT-DS-LEAD is the one who set it up in the calendar. ENG-LEAD collects and shares open questions pre-meeting. Participants: CLIENT-PRODUCT, CLIENT-VP-DS, CLIENT-DS-LEAD, CLIENT-PRODUCT-MGR (AdScale); PM, ENG-LEAD/ENG-1/ENG-2 as needed (Northwind). PM writes and publishes post-meeting notes on AdScale Confluence. Purpose: progress review, blocker escalation, decision requests, deliverable alignment.
- **Daily syncs** - weekdays. Internal Northwind team only (PM, ENG-LEAD, ENG-1, ENG-2). Quick standup: status, blockers, coordination.
- Weekly status reports - sent by DELIVERY-LEAD to external channel. Team fills in a Google Document
- **Internal status reports** - weekly, structured as "Activities" and "Blockers" sections
- **Async coordination** - Slack for day-to-day. Team coordinates internally before engaging AdScale.
- **Deep work** - variable. Documentation, analysis, presentation prep. Fits around meetings.

## Capture habits

This workspace is my second brain for the project. Goal: capture important stuff as it happens during the day into a daily markdown file so the workspace always has latest context.

- Quick ideas/questions: Markdown file (Obsidian or nvim), then into this workspace
- Decisions made in Slack/meetings: Extract and capture here, log significant ones in `decisions/`
- Status updates: Generated from STATUS.md using the `/status-update` skill
- Blockers: Surfaced in STATUS.md, escalated in weekly syncs

### Weekly sync artifacts

- **Prep notes / agenda** -> `comms/` (e.g., `comms/archive/2026-02-25-weekly-sync.md`)
- **Processed transcripts** -> `docs/calls/` (e.g., `docs/calls/call-2026-03-04-weekly-sync.md`)
- **Confluence-ready notes** -> `comms/` (e.g., `comms/2026-03-04-weekly-sync-confluence.md`)

## Communication

- **Primary channel**: Slack (both internal Northwind and external AdScale channels)
- **Stakeholder updates**: Weekly to AdScale via sync call. Internal status reports for DELIVERY-LEAD/Northwind.
- **Escalation path**: Technical blockers -> CLIENT-DS-LEAD or CLIENT-PRODUCT-MGR. Product/scope -> CLIENT-PRODUCT. Account/commercial -> DELIVERY-LEAD. Audience questions -> CLIENT-PERSONA-LEAD directly (confirmed Feb 19).
- **Claude Chrome extension**: Used for extracting Slack conversations and synthesizing high-reply-count threads. Valuable for multi-day channel reviews and stakeholder responsiveness tracking.

## Tools

- **Project management**: Jira (Kanban), Confluence (project docs and knowledge management), Miro
- **Communication**: Slack
- **Code**: GitHub, AWS, ClickHouse, FastAPI, Python
- **Personal**: This PM workspace (Claude Code), Obsidian/nvim for quick capture, Claude Chrome extension for Slack extraction
- **Presentation**: Miro for visual artifacts

## Definition of done

Current live DoD: **v4.2** - see [[2026-02-27-phase-1-dod-v4|Phase 1 DoD v4]]. "Done" = shipped to production, verified against real data. Each milestone has a Northwind delivery date and an AdScale review buffer tracked separately.

**Two-part tracking** (agreed with CLIENT-VP-DS, Mar 2): For each milestone, track (1) date Northwind opens final PR, and (2) buffer time for AdScale review + production deployment. Review after first completed milestone to calibrate whether buffer was optimistic or pessimistic. Purpose: separate Northwind delivery velocity from AdScale review cycle time without reopening the "done = PR submitted" debate.

## Anti-patterns to watch for

- **Over-scoping when anxious about proving value** - This is a pilot. Phase 1 just needs to work. Don't gold-plate.
- **Over-engineering process when unsure of role** - DELIVERY-LEAD limits ownership scope while expecting ceremony. Keep process minimal, focus on delivery outcomes.
- **Absorbing others' responsibilities** - DELIVERY-LEAD promised Jira Structure view but delegated Miro artifact to PM with compressed timeline. Watch for this pattern.
- **Not formalizing timeline slips early enough** - Gut says 1-2 weeks. Better to flag it now with data than apologize later.
- **Workaround accumulation** - AdScale provides workarounds rather than direct solutions (e.g., "use PROD data" vs. populating STAGING). Track these; they create tech debt.
- **Meeting requests** - CLIENT-DS-LEAD explicitly prefers async over meetings (Feb 19). Default to Slack. Only request a meeting when async clearly won't work.
- **PR size limit** - 400 lines is a guideline not a rule. Notebooks don't count toward the limit. Larger refactor PRs are acceptable when justified.
