---
date created: 2026-03-31
updated: 2026-03-31
visibility: public (blog/LinkedIn) - anonymized
status: draft - needs PM review for voice and deanonymization check
---

# Running a real project on an AI-native PM operating system

> What happened when I used Claude Code as my daily PM tool for an 11-week enterprise engagement. Not a product review - a field report.

## The setup

I'm a fractional Technical Program Manager. For the past 11 weeks, I ran a backend engineering engagement between two companies - a 4-person vendor team building a Forecast Service for a large enterprise client. My allocation was 0.5 FTE. The project had all the hallmarks of enterprise complexity: multiple stakeholders across two orgs, cross-team dependencies, shifting requirements, and a compressed timeline that kept getting more compressed.

For the past year, I've been building a personal PM operating system on top of Claude Code - a set of structured artifacts, skills, and workflows that I call "build." This engagement was the first real-world test.

The question wasn't "can AI do PM work?" It was more specific: can a structured set of AI-assisted workflows replace the spreadsheets, Notion boards, and manual note-taking that most PMs rely on? And if so, where does it actually save time versus where does it just shift the effort?

## What I built (the framework)

The system is straightforward. Each project gets a directory with three living documents:

- **BRIEF.md** - captured through an onboarding interview. Background, goals, stakeholders, constraints. Written once, referenced constantly.
- **STATUS.md** - the living state of the project. What's done, what's in progress, what's blocked. Updated as things change.
- **WAYS-OF-WORKING.md** - how I operate on this specific project. Meeting rhythms, capture habits, communication channels.

Beyond those, the directory accumulates:

- **Daily notes** - one per working day. Context from yesterday, overnight developments, today's priorities.
- **Decision log** - every meaningful decision with context, options, and rationale. Date-stamped.
- **Raw inputs** - transcripts, exports, Slack dumps. Never edited, only processed.
- **Processed docs** - summaries and specs derived from raw inputs. The stuff you actually read.

Claude Code runs as my working partner across all of these. Not as an automation tool - as a thinking partner with project context.

## The daily ritual

Here's what a typical morning looked like:

1. Open terminal, launch Claude Code in the project directory
2. Say "prep today's daily"
3. Claude reads yesterday's notes, checks the git repo for overnight PR activity, pulls the current status, and drafts today's daily note
4. I review, add context from Slack or calls, and we're caught up

This sounds simple but the compound effect is significant. By week 3, Claude had enough context in the project artifacts to draft daily notes that were 80-90% accurate without me providing any new information. The overnight PR checks caught things I would have missed. The carry-over tracking surfaced forgotten action items.

The key insight: **context retrieval is the actual time savings, not text generation.** I don't need AI to write my sentences. I need it to remember that three weeks ago we decided X because of Y, and that decision is now relevant because Z just happened.

## Where it worked well

### Processing raw inputs

The highest-ROI workflow was processing call transcripts and Slack dumps. A 45-minute closure call produces a rough transcript. Manually reviewing that transcript, extracting action items, updating the status doc, and logging decisions would take 30-45 minutes. With Claude, it takes about 5 minutes of interactive review.

The workflow: drop the transcript in `/raw/`, run the processing skill, review the structured output, approve or correct, done. The processing skill knows the project's stakeholders, understands the technical context, and can distinguish between "CLIENT-PRODUCT said he'd look at it" (action item) and "CLIENT-PRODUCT mentioned he'd looked at it" (context).

### Keeping specs in sync with reality

We maintained a detailed technical specification - about 50 pages of architecture, data contracts, API examples, and decision history. When the client's data scientist asked for a specific addition (mapping tables, API examples), I could:

1. Paste his Slack request
2. Have Claude read the current spec and the actual codebase
3. Generate the additions verified against real code, not just meeting notes
4. Update the doc

This is where the "verify claims against source code, not project management artifacts" principle paid off. Specs written from meeting notes drift. Specs verified against actual code don't.

### Decision logging

Every meaningful decision got logged with context, options, and rationale. By the end of the engagement, we had 9 formal decision documents and a running log of 25+ tactical decisions in the tech spec's appendix. When questions came up about "why did we do it this way?", the answer was always one search away.

The underrated part: Claude would proactively flag when a new decision contradicted or affected an old one. "This changes the assumption in the Feb 24 decision about X" - the kind of cross-referencing that's tedious to do manually but trivial when the full decision history is in context.

## Where it didn't work

### Real-time communication

The framework is excellent at capturing and processing information. It's useless for the political navigation that happens in live calls, Slack threads, and hallway conversations. When a stakeholder is being pushed out of the company and the project's future depends on reading organizational signals correctly, no amount of structured artifacts helps.

I still caught political shifts from meeting transcripts after the fact. But by then, the decisions were already made. The framework is a rearview mirror for organizational dynamics - useful for documentation, not for navigation.

### Stakeholder management

Related but distinct: the framework doesn't help you build relationships, read the room, or know when to escalate. These are human judgment calls that happen in real-time. Claude can help you draft the escalation email, but it can't tell you whether today is the right day to send it.

### The overhead is real

Maintaining the framework isn't free. Processing each transcript takes review time. Updating STATUS.md requires judgment about what changed. Daily notes need correction. The system pays for itself on engagements longer than 4-5 weeks, where the context accumulation compounds. On a 2-week sprint, the overhead would probably exceed the benefit.

## What surprised me

### The compound context effect

By week 8, Claude's daily prep was catching connections I was missing. "Yesterday's PR discussion about latency connects to the scaling decision from three weeks ago" - that kind of synthesis. Not because Claude is smarter than me, but because it has perfect recall of 8 weeks of daily notes, decision logs, and status updates. I don't.

### Spec quality went up, not just speed

I expected AI-assisted specs to be faster but sloppier. The opposite happened. Because Claude could verify claims against the actual codebase (we had a read-only code clone in the project directory), the specs were more accurate than what I'd produce manually. I'd write "the API returns X" based on a meeting discussion. Claude would check the code and say "actually it returns Y, and here's the function that does it."

### The framework shaped how I think about projects

After 11 weeks, I found myself structuring new problems the same way even outside this project. Decision logs, raw-then-processed input flows, daily context building. The framework became a thinking habit, not just a tool.

## The numbers

Over 11 weeks:

- ~55 daily notes generated and reviewed
- 9 formal decision documents
- 1 technical specification (~50 pages, continuously updated, verified against code)
- 25+ tactical decisions logged in running appendix
- 15 raw documents processed (transcripts, exports, Slack dumps)
- ~15 minutes daily on framework maintenance (daily prep + review)

Estimated time savings: 5-8 hours per week on context retrieval, spec updates, and status tracking. That's significant on a 0.5 FTE allocation - it's the difference between having headroom for strategic thinking versus drowning in administrative catch-up.

## What I'd do differently

1. **Start the decision log from day 1, not week 4.** I lost 3 weeks of decision context that I had to reconstruct later.
2. **Process raw inputs same-day, not next-day.** The processing quality drops when you're reconstructing context from a day-old transcript.
3. **Maintain the status doc more aggressively.** I let STATUS.md get stale during busy weeks. A stale status doc is worse than no status doc because it creates false confidence.
4. **Build the project archival workflow.** The framework has no "close project" skill. When the engagement ended, I improvised. That should be templated.

## Would I recommend this?

For fractional PMs or technical leads juggling multiple projects: yes, without hesitation. The context retrieval alone is worth the setup cost. Being able to say "what did we decide about X three weeks ago?" and getting an answer with full context in 5 seconds instead of 15 minutes of searching Slack and Google Docs - that changes how you operate.

For full-time PMs on a single project: probably still yes, but the ROI is lower because you have more headroom for manual context management. The sweet spot is when you're stretched thin across multiple workstreams and can't afford to hold everything in your head.

The framework isn't magic. It's structured context management with an AI partner that has perfect recall. The magic, if there is any, is in the compound effect of consistent daily use over weeks and months.

---

*I'm open-sourcing the framework. If you're interested in the templates, skills, and workflows, check out [the build repo] - still early, but usable.*
