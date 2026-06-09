---
name: ideation
description: Transform brain dumps, scattered thoughts, and half-formed ideas into structured documents. Automatically determines the right output type based on idea maturity - from quick one-pagers for early exploration to full PRDs for well-defined work. Use when the user is thinking out loud, dumping ideas, or needs help structuring messy thoughts.
---

# Ideation

Transform unstructured thinking into the right document for its maturity level.

## Phase 1: Intake

Accept whatever the user provides without judgment:

- Scattered thoughts and half-formed ideas
- Voice dictation transcripts
- Bullet points mixed with rambling
- Topic jumping, tangents, contradictions
- "I'm not sure but maybe..." statements

**The mess is the input. Don't ask for organization.**

Acknowledge receipt and begin analysis. Do not ask for clarification yet.

## Phase 2: Route

After intake, classify the input into one of five tracks. Consider:

- How mature is the thinking? (vague hunch vs clear problem vs detailed solution)
- What does the user need next? (validation vs alignment vs execution)
- Who is the audience? (themselves vs team vs stakeholders)

### Routing table

| Signal | Track | Output |
|--------|-------|--------|
| Vague idea, lots of "maybe", exploring whether something is worth pursuing | **Exploring** | One-pager |
| Clear problem, unclear solution, needs to figure out the approach | **Shaping** | Pitch (Shape Up style) |
| Approach is clear, needs detailed requirements for team execution | **Specifying** | PRD |
| Stuck between options, needs structured comparison | **Deciding** | Decision doc |
| Needs to explain or sell an idea to stakeholders | **Communicating** | Memo/proposal |

### Propose the route

Tell the user which track you've identified and why. Suggest the output type. Let them confirm or redirect.

Example: "This sounds like you're still exploring whether this is worth building. I'd suggest we shape this into a one-pager - problem, hypothesis, key assumptions - rather than jumping to a PRD. Want to go that direction, or do you need something more detailed?"

**Do not proceed until the user confirms the track.**

## Phase 3: Interview

Conduct a targeted interview based on the selected track. Load the appropriate rubric from [routing-rubrics.md](references/routing-rubrics.md).

### Interview principles

- Ask 2-3 questions at a time, max; target three rounds or less

- Target the weakest dimension first
- Be specific, not open-ended. Offer options where possible.
- Use the `AskUserQuestion` tool when interviewing the user.
- Reference what the user already said: "You mentioned X, did you mean...?"
- If the user says "I don't know" - that's useful signal, not a gap to force closed
- Match question sophistication to idea maturity. Don't ask about acceptance criteria for a hunch.

### Confidence loop

After each round of answers:

1. Recalculate confidence per the track's rubric
2. If below threshold, identify weakest dimension, ask targeted questions
3. If at threshold, proceed to generation
4. If stuck after 3 rounds, consider: the idea might not be ready yet. Say so.

## Phase 4: Generate

Create the output document using the appropriate template from `references/`.

| Track | Template | Output path |
|-------|----------|-------------|
| Exploring | [one-pager-template.md](references/one-pager-template.md) | `docs/ideation/{name}-one-pager.md` |
| Shaping | [pitch-template.md](references/pitch-template.md) | `docs/ideation/{name}-pitch.md` |
| Specifying | [prd-template.md](references/prd-template.md) | `docs/ideation/{name}-prd.md` |
| Deciding | [decision-doc-template.md](references/decision-doc-template.md) | `docs/ideation/{name}-decision.md` |
| Communicating | [memo-template.md](references/memo-template.md) | `docs/ideation/{name}-memo.md` |

Create `docs/ideation/` directory if it doesn't exist.

Present the document for review. Iterate based on feedback.

## Notes

- Match document weight to idea maturity. A one-pager should be one page. A PRD can be longer.
- Don't inflate early-stage thinking into heavy documents. That kills ideas.
- If during the interview the idea matures beyond its original track (e.g., exploring becomes shaping), propose upgrading the track.
- If the idea isn't ready for any structured output, say so. "This needs more thinking time" is a valid output.
- Reference the PM's principles (CLAUDE.md) when making judgment calls about document weight.
