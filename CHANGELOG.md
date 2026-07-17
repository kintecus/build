# Changelog

Notable changes to the Build framework. Newest first.

## Latest - agent-agnostic

Build was written in and for Claude Code. The skills were always written to the [Agent Skills](https://agentskills.io) open standard, but the docs and file layout assumed one tool. This release makes the framework's portability real and explicit.

**What this means for you:** you can now run the whole workspace in Codex, opencode, Cursor, Gemini CLI, or any other agent that implements the Agent Skills and AGENTS.md standards - without editing a single skill. If you're on Claude Code, nothing changes; `CLAUDE.md` still works, it's just a symlink now.

1. **`AGENTS.md` is the canonical instruction file**, with `CLAUDE.md` symlinked to it. `AGENTS.md` is the cross-tool convention (Codex, opencode, Cursor, ~20 others); Claude Code reads only `CLAUDE.md`, and [Anthropic documents this symlink](https://code.claude.com/docs/en/memory#agents-md) as the supported bridge. Edit `AGENTS.md`, never the symlink.
2. **Skills are reachable at both discovery paths.** They still live at `.claude/skills/`, with `.agents/skills` symlinked to the same folder, because Codex scans `.agents/skills` and never `.claude/skills`. One copy, no drift.
3. **New `docs/portability.md`** - what runs where, per-tool setup, and an honest tested-vs-assumed table. Verified: Claude Code and opencode (all 8 skills load unmodified, per `opencode debug skill`). Assumed: everything else on the spec's client list.
4. **`templates/PROJECT-CLAUDE.md` renamed to `PROJECT-AGENTS.md`**, and `onboard-project`/`tune-persona` now generate a per-project `AGENTS.md` plus its `CLAUDE.md` symlink. Claude Code discovers nested `CLAUDE.md` but not nested `AGENTS.md`, so both are needed.
5. **Docs reworded tool-neutrally.** The Claude Code specific parts (auto-memory, plugins, SessionStart hooks) are now flagged as such, with guidance on what to swap. None of them are load-bearing.
6. **Windows note:** the repo uses symlinks, which need Administrator or Developer Mode. `SETUP.md` and `docs/portability.md` document the `@AGENTS.md` import fallback.

## Reconciled from live use

Reconciled ahead of the public release:

1. **New `review-prep` skill** ported from live use: dispatches a fresh-context agent to read a project's STATUS/docs/decisions and produce a pre-review punch list (pulse, decisions waiting, decisions closed, stale/at-risk flags, the 3 questions for the meeting). The fresh-context dispatch is the point - a cold reader catches STATUS drift and cross-artifact risks the operator's own working memory misses. Validated across two project shapes before porting.
2. **Optional templates** from real projects: `STRATEGY.md` (durable vision/roadmap), `EXPERIMENT.md` (thesis-under-test), `CONTEXT-BRIDGE.md` (PM <-> separate engineering repo), and `FREELANCE-ENGAGEMENT.md` (billable engagement log).
3. **`onboard-project` now generates a per-project persona file** from the persona interview as a first-class output, not an optional afterthought.
4. **`process-document` gained transcript handling** - detects call transcripts and runs an ASR-cleaning sub-agent (incl. non-English translate-to-English) before summarizing.
5. **Richer `DECISION.md`** - status vocabulary now aligns with the standard ADR lifecycle (proposed / accepted / deprecated / superseded / revisiting).

## v2 - structural refresh

1. **`CLAUDE.md` is now workspace-level**, not per-project. The per-project version moved to `templates/PROJECT-CLAUDE.md`.
2. **New `WORKSPACE.md`** explains why Claude should run from the workspace root, not each project folder.
3. **New `docs/`** section covers the auto-memory system, Obsidian integration, and the plugin ecosystem.
4. **Two new skills** ported from live use: `tune-persona` and `git-week`.
5. **`templates/WAYS-OF-WORKING.md`** added (the triad of BRIEF/STATUS/WAYS was referenced but the template was missing).
6. **`examples/adscale-case-study/`** - a fully anonymized real project, included inline so you can see the framework in action.
