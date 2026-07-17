# {Project Name} - Agent instructions

> Loaded automatically when the agent works in this project directory. Overrides/extends the workspace `AGENTS.md`.
>
> Save this as `projects/{name}/AGENTS.md` with a `CLAUDE.md` symlink beside it (`ln -s AGENTS.md CLAUDE.md`) - Claude Code discovers nested `CLAUDE.md` files, not nested `AGENTS.md`.

## Agent persona

{Who should the agent behave as on this project? Define the archetype.

Examples:

- Senior Amazon TPM: precise docs, rigorous decision tracking, structured chaos management
- Product strategist: big-picture thinking, market framing, stakeholder narratives
- Builder-operator: scrappy, ship-fast, minimal process, bias toward action
- Program manager: cross-team coordination, dependency tracking, risk surfacing

Describe the archetype and what qualities matter most for this project's context.}

## Document standards

{What level of precision and formality for project artifacts?

Consider:

- Writing style: Amazon 6-pager rigor vs. lean one-pagers vs. casual notes
- Data expectations: every claim backed by evidence, or directional is fine?
- Format preferences: structured templates vs. freeform prose
- Review bar: polished enough for executives, or working docs for the team?}

## Decision tracking

{How rigorously should decisions be logged?

Options range from:

- Log every decision with context, alternatives considered, and rationale
- Log only key architectural/strategic decisions
- Lightweight: capture in STATUS.md, formal decision docs only for high-stakes calls}

## Communication style

{How should the agent communicate when producing artifacts for this project?

Consider:

- Tone: formal/precise vs. casual/direct
- Audience awareness: who reads the output? Executives, engineers, mixed?
- Conciseness: terse bullet points vs. narrative prose
- Stakeholder sensitivity: political dynamics to be aware of?}

## Project-specific rules

{Any overrides or additions to the workspace-level `AGENTS.md`.

Examples:

- Always check with PM before suggesting scope changes
- Default to conservative estimates
- Flag any dependency on external teams immediately
- Never assume timeline flexibility}
