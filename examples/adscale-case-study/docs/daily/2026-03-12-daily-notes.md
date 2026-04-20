---
date: 2026-03-12
date created: 2026-03-12 10:30
updated: 2026-03-12 12:09
---

# Daily notes - Mar 12 (Thu)

## Context from yesterday

Weekly sync (Wed) aligned the versioned PR release plan with AdScale. CLIENT-PRODUCT dropped a new gate: no prod releases without his QA sign-off on STAGING. This wasn't in the plan and will push the Mar 16 milestone. CLIENT-DS-LEAD approved PR #121 (win rates service) and agreed to fast-track reviews.

Daily sync confirmed ENG-LEAD worked through the weekend + until 3 AM on ZIPs. ENG-1 porting ZIPs to current prod version (no limits). ENG-2 parked his PR to not overload CLIENT-DS-LEAD's review queue - starting second recommender task instead.

## Updates

**ENG-LEAD (10:03 AM)** - deployed v3 (win rates + CTR/VTR/viewability from CSVs) to STAGING. Prepared prod data snapshot (2025-03-10, -1d) for FR, US, DK, SE. Shared with CLIENT-PRODUCT for QA. Also added `cpmGuidanceTruncated` field to the response - shows the truncated curve that `recommendedImpressions`/`recommendedCpm`/`estimatedReach` are calculated from. Truncation controlled by `truncateMinImpressionProportion` param (default 0.5).

Sample FR result (display, in-app, mobile, interstitial, $1k budget): recommended CPM 10.1, 99k impressions, ~66k reach, CTR 3.9%, viewability 84%. Full curve goes up to 2.4M impressions at CPM 30.

This unblocks CLIENT-PRODUCT's QA on v3. STAGING data prep question is answered - ENG-LEAD handled it.

**GitHub PR status (as of morning):**

| PR | Title | State | AdScale review |
| ---- | -------------------------------- | --------------------------------------------------------------------------- | ---------------------- |
| #121 | Win rates v2 (service) | **4 approvals** (eng2-gh/ENG-2, eng1-gh, client-product-mgr-gh, client-ds-lead-gh). Ready to merge. | CLIENT-DS-LEAD approved Mar 11 |
| #125 | Win rates v2 ETL (ENG-1) | Approved internally (eng2-gh/ENG-2, eng-lead-gh). SonarQube passed. | No AdScale approval yet |
| #122 | CTR/VTR/viewability v3 (service) | Only cursor bot comments. No human reviews. | None |
| #127 | v3 ETL DAG changes | Opened Mar 11. Cursor bot flagged 2 high-sev items. No human reviews. | None |
| #126 | v4 ZIP codes (service) | Opened Mar 11. Zero review activity. | None |
| #119 | Eval deprecation | No activity since Mar 6. CLIENT-DS-LEAD approved earlier but never merged. | Stale |
| #120 | STAGING e2e | No activity. Blocked on AdScale Terraform PR #176. | Blocked |

Bottom line: #121 is fully approved and should be merging right now. Everything else in the pipeline is waiting on AdScale review. #125 is next in line. Ask at daily: has CLIENT-DS-LEAD actually hit merge on #121?

## Today's priorities

1. **PR #121 merge** - fully approved, should be merging now. Check if CLIENT-DS-LEAD merged it.
2. **ZIP PRs from ENG-1** - promised by this morning. Check if delivered
3. **CPM margins** - CLIENT-PRODUCT promised Q2 margin CSV yesterday. Did it arrive? ENG-LEAD needs it for integration (ADV-30346)
4. **Jira cleanup** - continue updating descriptions, prep for Friday session with ENG-LEAD

## CLIENT-PRODUCT's QA gate - scope change

DELIVERY-LEAD and ENG-LEAD flagged on today's daily: CLIENT-PRODUCT's QA gate is a change of scope.

After cross-checking: on the Mar 5 DoD call, CLIENT-PRODUCT asked for API access to test, PM set expectations as "smoke testing and common sense validation, not rigorous metric-based QA." CLIENT-PRODUCT agreed. That's what was aligned - a light smoke test, not a blocking gate. The Google Doc has "API shared w/ CLIENT-PRODUCT" as a pending suggestion (never formally accepted, but per DELIVERY-LEAD we weren't expecting AdScale to sign off on each edit - the call alignment counts).

What's new:
- **Blocking gate before production** - the agreed level was a smoke test. CLIENT-PRODUCT escalated to "no prod releases without sign-off." His intent is probably to validate the combined 1.2A+1.2B result, which is reasonable. But his language plus the versioned release plan (v2 → v3 → v4) means it applies to intermediate steps too.
- **No acceptance criteria** - "no major discrepancy" without defining what's acceptable. Model outputs show ~2x differences vs historical, which are expected.
- **No turnaround time** - open-ended. This is the real timeline risk.

Additional context: the QA gate is harder to execute than it needs to be because there's no parallel prod environment. Northwind raised the need for a parallel production DAG as early as Feb 26. ENG-1's quote from the Mar 10 daily: "when we proposed a solution - let's do a parallel run in a production-like environment [...] the guys say: just test locally, if it works locally it'll work in production." All we have is STAGING (8 GB, can't pull from prod). Captured in the decision log.

See [[2026-03-12-product-qa-gate-scope-change|decision log entry]].

## Things to watch

- **CLIENT-PRODUCT's QA gate turnaround** - no one explicitly agreed how much buffer this adds. Need to get CLIENT-PRODUCT to commit to a turnaround time or we're open-ended
- **Model data quality vs QA expectations** - CLIENT-DS-LEAD's model predictions are v1, acknowledged as early-stage. ENG-LEAD flagged data quality issues (Mar 6) - predictions lack ZIP/app bundle granularity, may reduce output quality vs historical in some dimensions. No ETA from AdScale on improved models. Risk: CLIENT-PRODUCT's QA finds discrepancies caused by model data quality, blocks releases until AdScale ships better data - outside our control.
- **ZIP resource risk** - large countries may OOM. Can only test once ZIPs + predictions are combined on real data
- **Milestone slip comms** - DELIVERY-LEAD planned to flag slip risk at yesterday's sync. Did he? If PRs aren't on prod by Monday, he writes to CLIENT-DECISION-1/CLIENT-DECISION-2

## Questions for daily sync

- ENG-1: ZIP PRs ready?
- ENG-LEAD: has CLIENT-DS-LEAD merged #121? What's blocking #125 AdScale review?
- ENG-LEAD: did CLIENT-PRODUCT respond on Slack about QA? (STAGING data sent this morning - ball in CLIENT-PRODUCT's court)
- ENG-LEAD: cursor bot flagged 2 high-sev items on #127 (missing test columns, missing rate CSV args) - real issues or noise?
- ENG-2: what's the second recommender task, and is it tracked in Jira?

## Carry-overs

- [x] PM - flag UAT buffer impact to DELIVERY-LEAD (from STATUS.md)
- [ ] PM - schedule 1.3 alignment sync (recommendation UX + frontend path)
- [ ] PM - follow up with CLIENT-PRODUCT on CPM benchmarks for narrow audience detection
- [ ] PM - Jira session with ENG-LEAD Friday (30 min)
- [ ] ENG-LEAD - formalize short release plan for AdScale (from yesterday's daily)
- [ ] ENG-LEAD + CLIENT-PRODUCT - Slack sync on QA validation (from weekly sync)
