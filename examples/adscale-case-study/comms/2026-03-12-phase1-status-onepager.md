---
date: 2026-03-12
type: status onepager
audience: DELIVERY-LEAD, CONSULTING-CEO, CLIENT-DECISION-2 (decision makers)
scope: private (Northwind internal only)
---

# AdScale Phase 1 - status as of Mar 12

## Where we are

Phase 1 upgrades AdScale's forecasting tool with better prediction models and geographic targeting. Deadline: end of March (2-week extension approved, discount negotiated). Three deliverables, shipped in sequence.

| Deliverable | What it does | Status | Target |
|---|---|---|---|
| Model integration | Replaces historical guesswork with ML-backed predictions (win rates, CTR, viewability) | QA with CLIENT-PRODUCT | Was Mar 16, slipping ~1 week |
| Geo targeting | Forecasts at postal code level instead of country-only | Code complete, waiting on model integration | After model integration |
| Recommender | Detects narrow audiences, suggests alternative personas | In development (embedding pipeline PR submitted) | Mar 31 |

## What's working

- **7 PRs submitted in 4 days** (Mar 8-12). The team has ramped up significantly. Pipeline is full.
- Model integration deployed to test environment with data for France, US, Denmark, Sweden. CLIENT-PRODUCT is reviewing.
- AdScale engineering (CLIENT-DS-LEAD) approved win rates PR with no issues. Review process is moving.
- Recommender work started ahead of schedule - ENG-2 submitted the embedding pipeline PR today.

## What's at risk

**CLIENT-PRODUCT's QA gate is the critical path.** CLIENT-PRODUCT (AdScale PM, leaving end of March) added a new requirement: no production releases without his personal sign-off. This was not in the agreed scope. We accepted it to keep the relationship intact, but it introduces timeline risk because:

- No defined turnaround time - we don't know how long his review takes
- No acceptance criteria - "no major discrepancy" without defining what's acceptable
- The ML models feeding our system are early-stage (acknowledged by AdScale's own team). Output quality issues are upstream, not in our code.

**Net effect:** Mar 16 target for first production release slips by approximately one week. If CLIENT-PRODUCT's QA triggers rework requests tied to model quality, we flag it as new scope.

## What we need from AdScale

| Item | Owner | Why it matters |
|---|---|---|
| QA sign-off on STAGING data | CLIENT-PRODUCT | Blocks all production releases |
| Merge PR #121 (already approved by 4 reviewers) | CLIENT-DS-LEAD | First domino - everything else queues behind it |
| Review 6 more PRs in the pipeline | CLIENT-DS-LEAD | We've submitted the work, review speed = release speed |
| Phase 2 session | CLIENT-PRODUCT | Staffing decisions due this week |

## Phase 2 readiness

No update from AdScale on Phase 2 discussions. CLIENT-PRODUCT has not scheduled the session he promised on Mar 5. PM is pushing daily in the shared channel.

**Staffing implication:** End-of-week system notification goes out (2 weeks to end of allocation). If Phase 2 doesn't materialize, the team moves to the new project. We need AdScale's intent this week.

## Defense posture

If AdScale challenges deliverable quality or claims Phase 1 isn't complete:

1. **Our functional work is done** (or will be by end of March). Code works, PRs submitted, tests pass.
2. **Output quality depends on their models.** We consume AdScale's ML predictions. If the predictions are rough, the output is rough. We didn't build or maintain those models.
3. **The DoD protects us.** No quantitative acceptance criteria in the agreed Definition of Done. "Production deployment cycle completed" is the bar.
4. **QA gate is new scope.** Not in the original agreement, not in the shared DoD document. We accepted it as a courtesy. Any rework requests from QA findings = additional effort, tracked and communicated.
