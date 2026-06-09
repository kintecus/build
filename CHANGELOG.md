# Changelog

Notable changes to the Build framework. Newest first.

## Latest

Reconciled from live use ahead of the public release:

1. **Optional templates** from real projects: `STRATEGY.md` (durable vision/roadmap), `EXPERIMENT.md` (thesis-under-test), `CONTEXT-BRIDGE.md` (PM <-> separate engineering repo), and `FREELANCE-ENGAGEMENT.md` (billable engagement log).
2. **`onboard-project` now generates a per-project `CLAUDE.md`** from the persona interview as a first-class output, not an optional afterthought.
3. **`process-document` gained transcript handling** - detects call transcripts and runs an ASR-cleaning sub-agent (incl. non-English translate-to-English) before summarizing.
4. **Richer `DECISION.md`** - status vocabulary now aligns with the standard ADR lifecycle (proposed / accepted / deprecated / superseded / revisiting).

## v2 - structural refresh

1. **`CLAUDE.md` is now workspace-level**, not per-project. The per-project version moved to `templates/PROJECT-CLAUDE.md`.
2. **New `WORKSPACE.md`** explains why Claude should run from the workspace root, not each project folder.
3. **New `docs/`** section covers the auto-memory system, Obsidian integration, and the plugin ecosystem.
4. **Two new skills** ported from live use: `tune-persona` and `git-week`.
5. **`templates/WAYS-OF-WORKING.md`** added (the triad of BRIEF/STATUS/WAYS was referenced but the template was missing).
6. **`examples/adscale-case-study/`** - a fully anonymized real project, included inline so you can see the framework in action.
