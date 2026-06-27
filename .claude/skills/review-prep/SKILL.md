---
name: review-prep
description: Prepare a busy operator for a project review by deriving meaning from the project's artifacts - a punch list of pulse, decisions waiting on you, decisions recently closed, stale/at-risk flags, and the 3 questions worth the meeting. Use when the user runs /review-prep, asks to prep for a review/sync, or wants to know "what needs my attention" on a project.
---

# Review Prep

Produce a 2-minute punch list that an operator opens before a project review. The job is not to summarize the docs - it is to **derive meaning across them**: surface what needs a human decision, what quietly stalled, and what no single doc says on its own.

## Which project

If the user named a project, use it. If they ran this from inside a project context, use that. If ambiguous, ask which project (one short question, do not guess).

## How to run it - use a fresh-context agent

**Dispatch a fresh general-purpose agent to read the artifacts and produce the doc.** Do NOT do the reading-and-synthesis yourself in the main conversation - if you have been working on the project this session, your context is polluted by what you already know, and you will miss exactly the cross-artifact findings that make this valuable (the whole point is catching what the operator's own working memory misses). Hand the agent only the project root and these instructions.

The agent reads, in order:

1. `STATUS.md` - current state, active threads, open questions, next actions
2. `docs/INDEX.md` - the one-line index of every doc; read this to know what exists before opening anything, then open only the live threads (judge from dates - what is active *now*)
3. The most recent / most active `docs/` files - not all of them, just the ones the index shows are live
4. `decisions/*.md` - the decision log

Pass the agent today's date explicitly.

## Output format - exactly these five sections

**1. Project pulse** - one line per *active* thread (skip dormant ones). Format: `thread - RED/AMBER/GREEN - the single most important fact about its current state (source: filename)`. Judge the color: green = moving, amber = waiting/at-risk, red = blocked or overdue.

**2. Decisions waiting on the operator** - the 3-7 things that genuinely need the operator's judgment now. Each phrased as a crisp question, with a source. Do NOT pad - if it's already decided, it doesn't belong here. Quality over quantity.

**3. Decisions recently made (closed loop)** - decisions from roughly the last 2 weeks, one line each, dated, sourced. The "what got closed" view.

**4. Stale / at-risk flags** - anything overdue, abandoned, or contradictory: a next-action open too long, a passed deadline, two docs that disagree, a cross-thread blocker, a recurring un-owned open question, a STATUS.md whose tail is out of sync with its newest entries. If nothing, say "none spotted." Cite sources.

**5. The 3 questions for the review** - if the operator had 25 minutes, the 3 questions actually worth the room's time. Genuine decisions or risks, not status updates.

## Hard rules (encode these in the agent's prompt, they are what make it work)

- **Every factual claim links to its source file** in `(source: filename)` form. Never assert project state without a citation. If you can't source it, don't claim it.
- **Derive meaning, don't summarize.** "X is amber because the session happened but its outcome is unconfirmed" beats "X had a meeting." The cross-artifact findings (a blocker two threads share, a STATUS freshness lie, a recurring un-owned question) are the highest-value output - hunt for them.
- **Be terse.** A punch list, not a report. Read in 2 minutes.
- **Do not invent.** If a thread's state is genuinely unclear from the docs, say so - that itself is a finding.

## After the agent returns

Present the punch list to the operator. Then:

- If it surfaced a concrete correctness issue (a stale STATUS date, a contradiction), offer to fix it.
- Do not auto-save by default - this is a pre-meeting artifact, often ephemeral. Offer to save it to `comms/YYYY-MM-DD-review-prep.md` only if the operator wants a record.

## Why the fresh-context dispatch matters

This skill's one load-bearing design choice is that the synthesizer must NOT run in the operator's working context. The operator (and any agent that has been editing the project this session) shares the same blind spot: things you just touched feel current, so you don't notice STATUS drift, a blocker two threads quietly share, or a decision that's been "waiting on me" for two weeks. A cold reader of only the artifacts catches these. Validated in live use across two different project shapes (a BD-heavy engagement and a discovery/PoC side project) - in both, the cold agent flagged STATUS staleness the human curator could not see, plus cross-artifact risks the working sessions had missed.
