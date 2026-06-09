---
date: 2026-03-12
date created: 2026-03-12 12:39
updated: 2026-03-12 13:15
---

# CLIENT-PRODUCT's QA gate before production releases

## Context

At the Mar 11 weekly sync, CLIENT-PRODUCT raised a hard requirement: no production releases without his QA sign-off on STAGING. This applies to all releases - CSV migration (v2/v3) and geotargeting (v4). This was not in the original release plan.

## What's in scope vs what's new

**What was agreed:** On the Mar 5 DoD alignment call, CLIENT-PRODUCT asked "how would you do the QA?" PM set expectations: "smoke testing and common sense validation... we cannot do a very rigorous metric-based QA." CLIENT-PRODUCT asked for API access to test himself. The team agreed. This is reflected in both the local DoD v4.2 ("CLIENT-PRODUCT's QA pass on milestone outputs") and PM's suggestion on the [shared Google Doc](https://docs.google.com/document/d/REDACTED/edit) ("API shared w/ CLIENT-PRODUCT"). The suggestion wasn't formally accepted in the Google Doc, but per DELIVERY-LEAD, AdScale was never expected to sign off on each edit - the call alignment is what counts.

So a light QA pass - CLIENT-PRODUCT gets API access, does a smoke test - was agreed. What was not:

- **Blocking gate before production.** CLIENT-PRODUCT's intent is likely to validate the combined 1.2A+1.2B result before it's live - a reasonable ask. But his language ("no production releases without QA sign-off") plus the versioned release plan (v2 → v3 → v4, each deployed independently) means the gate applies to intermediate steps too.
- **No acceptance criteria.** CLIENT-PRODUCT is validating against "no major discrepancy" without defining what's acceptable. Model outputs show ~2x differences vs historical, which are expected but could trigger extended back-and-forth.
- **No turnaround time.** An open-ended gate is a timeline risk with no accountability. This is the real concern.

QA/UAT was never discussed during Discovery. The Feb 5 tech design review (end of Discovery) covered phase approaches, batch processing, and CPM margins - no mention of client validation. The Northwind proposal v1.4 had no QA/UAT step in deliverables and explicitly deferred success criteria to Discovery. Discovery produced functional exit criteria, not QA gates.

## How it escalated

| Date | Source | What was said | Implication |
|---|---|---|---|
| Presale | Northwind proposal v1.4 | No QA/UAT in deliverables. Success criteria "deferred to Discovery." | No client validation pass contemplated |
| Feb 5 | Tech design review (end of Discovery) | Phase approaches confirmed, batch processing decided. No QA discussion. | Discovery closed without QA process |
| Feb 27 | DoD v4 (Northwind-drafted) | "CLIENT-PRODUCT's QA pass on milestone outputs" in local copy. Google Doc has "API shared w/ CLIENT-PRODUCT" as pending suggestion. | QA pass as checklist item, not a blocking gate |
| Mar 2 | Phase 1 sync | (CLIENT-VP-DS) Work completion = deployed to production | No intermediate sign-off step |
| Mar 5 | DoD alignment sync (transcript) | CLIENT-PRODUCT: "if you also share with me the API, so that I can call it and review it on my side." PM: "smoke testing and common sense validation... we cannot do a very rigorous metric-based QA." CLIENT-PRODUCT: "Okay." | Light smoke test agreed, not a blocking gate |
| Mar 11 | Weekly sync | CLIENT-PRODUCT: "No production releases without his QA sign-off." | Blocking gate. Intent is likely combined 1.2 validation, but applies to each versioned release in practice. |

## Validation infrastructure gap

CLIENT-PRODUCT's QA forces validation on STAGING - a single-instance 8 GB ClickHouse environment that can't pull from prod. There is no staging environment, no parallel production DAG, and no way to run a release candidate alongside the existing pipeline. Northwind raised the need for parallel production validation repeatedly:

| Date | What happened |
|---|---|
| Feb 26 | ENG-1: "a parallel DAG setup is required to replicate prod conditions before further refactoring is safe" |
| Feb 27 | PM: "once we have our parallel DAG running in prod, we can start simplifying the code because we'll be able to validate against real data" |
| Mar 2 | Internal DoD alignment: validation approach debated. Agreed on manual comparison. |
| Mar 4 | STAGING sandbox with prod data approved by CLIENT-DS-LEAD/CLIENT-PRODUCT-MGR. "The more autonomy you build, the better for everybody." |
| Mar 5 | Prod schema drift incident. Post-mortem: "If the sandbox had been in place with real prod partitions spanning multiple ETL runs, this would have surfaced during STAGING testing." |
| Mar 10 | ENG-1 (daily transcript): "We requested production-like data snapshots for testing [...] when we proposed a solution - let's do a parallel run in a production-like environment - the guys say: just test locally, if it works locally it'll work in production." |
| Mar 11 | STAGING limitations confirmed by CLIENT-DS-LEAD: DAG can't pull from prod, single instance, 8 GB RAM, resource limits on user queries. |

The workaround: STAGING with manually prepared data snapshots (3-4h per regeneration). This is what ENG-LEAD used to prepare v3 data for CLIENT-PRODUCT on Mar 12.

## Model predictions data quality risk

The model prediction CSVs from CLIENT-DS-LEAD are v1 - acknowledged by CLIENT-DS-LEAD as early-stage. ENG-LEAD flagged multiple data quality issues (Mar 6 Slack thread):

- Predictions are keyed on `geoip_code, environment_type, ad_unit_type, device_computed_type` only - no ZIP or app bundle dimensions
- Predicted values are constant across all ZIP codes and all app bundles for a given context
- Switching from historical win rates to model predictions may reduce granularity in some dimensions
- CLIENT-DS-LEAD acknowledged this is a v1 limitation - "use current files as-is, improve models later"

This matters for QA because CLIENT-PRODUCT is comparing model-based outputs against historical data. Expected discrepancies (~2x in some dimensions) are baked in by the current model granularity. No ETA from AdScale on when improved model data will be available. Risk: QA findings trigger a data quality discussion that blocks releases until AdScale ships better models - something entirely outside Northwind's control.

## Decision

Accept the gate, push for defined turnaround. ENG-LEAD already deployed v3 to STAGING with data for FR, US, DK, SE (Mar 12 morning). Ball is in CLIENT-PRODUCT's court.

**Rationale:**

- CLIENT-PRODUCT is the product owner and most qualified to spot business-logic issues. Catching problems before prod is in everyone's interest.
- Pushing back on a reasonable QA step looks bad, especially when model outputs show real discrepancies vs historical.
- The real risk is undefined turnaround and undefined acceptance criteria, not the gate itself.
- CLIENT-PRODUCT is leaving end of March - short-term constraint, not a permanent process change.

## Impact

- Mar 16 milestone target slips. Exact impact depends on CLIENT-PRODUCT's turnaround time.
- Timeline was already tight (1.5-2 months, now on 2-week extension). Undefined QA cycle compounds the risk.
- Model data quality issues may extend QA back-and-forth beyond what a simple sign-off requires.
- Documented as scope change for timeline tracking (DELIVERY-LEAD, ENG-LEAD flagged Mar 12 daily).
- Tracked as NEW-12 in backlog.

## Who was involved

- CLIENT-PRODUCT (raised the gate, Mar 11)
- DELIVERY-LEAD, ENG-LEAD (flagged as scope change, Mar 12)
- PM (documenting)
