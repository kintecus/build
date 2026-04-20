---
date: 2026-03-16
type: escalation email draft
variant: "Option A - Close Phase 1 cleanly"
audience: CLIENT-DECISION-1, CLIENT-VP-DS, CLIENT-DECISION-2 (AdScale); cc DELIVERY-LEAD (Northwind)
date created: 2026-03-16 13:17
sent: false
updated: 2026-03-16 13:22
---

# Option A - Close Phase 1 cleanly

> Posture: defensive. "We delivered what was agreed. The remaining gap is on your side. Here's how we unblock."
> Use this if DELIVERY-LEAD's priority is protecting Northwind's delivery record and exiting cleanly.

---

**Subject:** Forecast Service - Mar 16 milestone missed; parallel deployment proposal

Hi CLIENT-DECISION-1, CLIENT-VP-DS,

The Mar 16 production release target for model predictions integration (1.2B) is missed. The integration code is complete and reviewed. The blocker is a data quality gap between the model predictions and historical win rates that produces forecasts unacceptable for production use. We propose a parallel deployment (v1/v2) to unblock the release sequence while predictions quality is addressed separately.

**Integration status**

Four PRs cover the predictions integration. Two are merge-ready:

- **#121 - v2 win rates (service)**: approved by 4 reviewers including CLIENT-DS-LEAD and CLIENT-PRODUCT-MGR. Merge-ready.
- **#122 - v3 CTR/VTR/viewability (service)**: approved by CLIENT-DS-LEAD. Merge-ready.
- **#125 - v2 win rates (ETL)**: CLIENT-DS-LEAD requested changes Mar 12. All addressed Mar 16. Awaiting re-review.
- **#127 - v3 CTR/VTR/viewability (ETL)**: approved internally. Awaiting AdScale review.

**QA findings**

CLIENT-PRODUCT tested 20,000 forecast scenarios across US, FR, DE, and SE, plus 192 targeted isolation tests comparing STAGING (model predictions) against production (historical win rates).

The integration code works correctly. Five metrics confirm this: response latency is identical between environments, country ordering is preserved, budget-to-CPM scaling follows the same curve, feasibility has zero mismatches across all 10,000 scenarios, and web segment forecasts align within 0.83-1.00x of production.

The divergence is isolated to in-app segments:

- **banner_320x50 inventory** is entirely absent from the model predictions. Production has $0.27-$0.61 CPM inventory for this ad unit; STAGING has none. 9.8% of scenarios show >2x CPM increase as a result.
- **In-app CPM accuracy** after excluding banner_320x50: only 24.5% of scenarios fall within +/-10% of production CPM. Target: >70%.
- **Directional inconsistency across markets**: FR/DE are 16-25% more expensive on STAGING (planners would overpay). US is 50% cheaper (revenue leakage). SE is 26% cheaper with 3.5x more impressions (underpricing).

**Root cause analysis**

Two factors compound to produce the gap. Both relate to how model prediction data interacts with existing pipeline mechanics - not to the integration code itself.

First, CPM bucketing granularity. The model predictions use a 0.5 CPM step. All inventory priced below $0.50 collapses into a single bin, erasing the difference between $0.10 and $0.40 inventory. Historical data does not have this limitation because it operates on raw bid prices. Reducing the step to 0.1 in the sub-$0.50 range would preserve the distribution.

Second, truncation compounding. The Forecast Service removes the bottom portion of the CPM curve to filter low-price inventory bias. With historical data, the low-CPM range has enough volume that truncation removes noise as intended. With model predictions, the low-CPM range is already sparse - truncation removes meaningful inventory on top of an already-thin tail. This explains the CPM floor gap: STAGING effective floor is $1.23-$1.93 vs production at $0.25-$0.87.

Both factors are addressable. Neither requires changes to the integration code.

**Proposal: parallel API deployment (v1/vX)**

Deploy current logic (v1/forecast, historical win rates) and new logic (v2/forecast, model predictions) side by side in production. v1/forecast continues serving live traffic. v2/forecast is available for evaluation and tuning against production-scale data.

This completes the integration deliverable per the agreed Definition of Done - model predictions are integrated, deployed, and available in production. It gives CLIENT-PRODUCT a production-grade comparison tool for ongoing model evaluation. And it unblocks the release sequence with zero risk to live forecasting.

Without parallel deployment, the release sequence stalls until model prediction accuracy meets production standards. No timeline exists for that work today.

**ZIP code support (1.2A) requires the same approach**

ZIP-level data is 10x the volume of country-level data. Three constraints make STAGING validation insufficient:

1. STAGING environment resources cannot support end-to-end testing at ZIP-level data volume.
2. There is no staging environment with production-representative data - standard practice for projects of this scale. To compensate, Northwind built a data replication pipeline that syncs production data into STAGING, but it runs 3-4 hours per cycle as a nightly job and is not fully stable (scripts crash without auto-restart). This infrastructure sits outside the MPT deliverable scope.
3. [X]% of personas are missing ZIP values in STAGING. The full picture is only available in production.

For ZIPs, we propose a dedicated v3/forecast endpoint on production - separate from the predictions v2/forecast - allowing ZIP-specific QA and tuning independently. v1/forecast remains untouched throughout.

For both predictions and ZIPs, production is the only viable end-to-end environment. Parallel deployment is not a preference - it is the only path to proper validation.

**Scope boundary**

In Phase 1, Northwind committed to integrating model predictions and geo-ZIP codes into the forecasting pipeline. The accuracy of the resulting forecasts depends on the input data - the model prediction files and persona ZIP coverage - which sits outside the integration scope. This was understood from the outset.

The ongoing QA and tuning cycle - iterating on model predictions, adjusting bucketing granularity, improving ZIP data coverage - is Phase 2 scope. Our team can support this work, but it requires a separate agreement.

**Decisions needed**

Three items require your input before the next release can proceed:

1. **Parallel deployment approval** - v2/forecast for predictions, v3/forecast for ZIPs, v1/forecast untouched. Northwind can deploy within days of approval. Proposed decision date: Mar 17 call.
2. **QA and tuning ownership** - the two root causes of the accuracy gap (bucketing granularity, truncation interaction) are identified and addressable. Who owns the iteration cycle, and what's the timeline?
3. **Phase 2 scope and staffing** - current Northwind team allocation ends March 31. Continuing into Phase 2 requires scope agreement and staffing confirmation this week. Owner: CLIENT-DECISION-1.

We propose a call tomorrow (Tuesday, March 17) to align on all three items.

DELIVERY-LEAD
