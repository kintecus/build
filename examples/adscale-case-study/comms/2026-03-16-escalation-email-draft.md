---
date: 2026-03-16
type: escalation email draft
audience: CLIENT-DECISION-1, CLIENT-VP-DS, CLIENT-DECISION-2 (AdScale); cc DELIVERY-LEAD (Northwind)
date created: 2026-03-13
updated: 2026-03-16
sent: false
---

# Escalation email - Mar 16 milestone slip

> Draft v3. Updated with truncation compounding analysis and ENG-LEAD's technical input.
> Flow: PM + ENG-LEAD polish -> DELIVERY-LEAD reviews and adjusts -> DELIVERY-LEAD sends + requests meeting Tue Mar 17.

---

**Subject:** Forecast Service - milestone update and proposed path forward

Hi CLIENT-DECISION-1, CLIENT-VP-DS,

Sharing an update on the model predictions integration (1.2B) and a proposed path forward on the items that need alignment.

**Where we are**

The integration PRs are submitted and nearly all reviewed:

- v2 win rates (service #121) - approved by four reviewers including CLIENT-DS-LEAD and CLIENT-PRODUCT-MGR. Merge-ready.
- v3 CTR/VTR/viewability (service #122) - approved by CLIENT-DS-LEAD. Merge-ready.
- v2 win rates (ETL #125) - CLIENT-DS-LEAD's review comments addressed, awaiting re-review.
- v3 CTR/VTR/viewability (ETL #127) - approved internally, awaiting AdScale review.

CLIENT-PRODUCT ran thorough QA on STAGING - 20,000 forecast scenarios across US, FR, DE, and SE, plus 192 targeted isolation tests. His findings confirm the code works correctly: latency is identical, country ordering preserved, budget scaling matches, feasibility has zero mismatches, and web segments align within 0.83-1.00x of production.

The divergence is isolated to in-app segments and traces to the model prediction data:

- banner_320x50 inventory ($0.27-$0.61 in production) is entirely absent in the model predictions, causing 9.8% of scenarios to see >2x CPM increases
- Even excluding that ad unit, only 24.5% of in-app scenarios land within +/-10% CPM of production (target: >70%)
- The low-CPM tail of the bid guidance data is systematically thinner in the model predictions vs historical data

We've identified two factors that contribute to the gap:

1. **CPM bucketing granularity.** The model predictions use a 0.5 CPM step. For the sub-$0.50 range, this collapses all cheap inventory into a single bin, losing the granularity that distinguishes $0.10 from $0.40 inventory. A finer step (e.g. 0.1) in that range would preserve the distribution.

2. **Truncation compounding.** The Forecast Service applies a truncation to the lower portion of the CPM curve to filter out low-price inventory bias. With historical data, there's enough volume in the low-CPM range that this works as intended. With model predictions, the low-CPM range is already sparse - truncation on top of that removes meaningful inventory, not just noise. This is why STAGING's effective CPM floor ($1.23-$1.93) is significantly higher than production ($0.25-$0.87).

The integration works as built. The accuracy gap is a function of the model prediction data interacting with existing pipeline mechanics - CLIENT-PRODUCT's own analysis confirms this. Both contributing factors are addressable, but require alignment on approach.

**Two items that need alignment**

**1. Parallel API deployment (v1/vX)**

We propose deploying both the current logic (v1, historical win rates) and the new logic (v2+, model predictions) side by side in production. v1 keeps serving live traffic, v2 is available for evaluation and comparison against real production data.

This approach:

- Completes the integration deliverable per the agreed DoD
- Gives CLIENT-PRODUCT a production-grade comparison tool for model evaluation
- Unblocks the release sequence with zero risk to live forecasting
- Allows quality tuning to continue in parallel

Without parallel deployment, the entire release sequence stalls on model prediction improvements with no clear timeline.

**2. ZIP code support (1.2A) - production validation required**

ZIP-level data is approximately 10x the volume of country-level. Three issues:

- STAGING environment resources are insufficient for end-to-end testing at this scale
- There is no staging environment with production-representative data - standard practice for projects of this scale. To compensate, Northwind built a data replication pipeline that syncs production data into STAGING, but it runs 3-4 hours per cycle (nightly jobs) and is not fully stable - scripts crash and don't auto-restart. This infrastructure is outside the MPT deliverable scope.
- A percentage of audiences are currently missing ZIP values, with no way to assess the full picture without production data

For ZIPs specifically, parallel deployment to production (v1/vX) is not just preferred - it's the only viable path. STAGING cannot provide the data completeness or resource capacity needed for proper validation.

**What we need from you**

- Decision on the parallel deployment approach - both for predictions and ZIPs
- If model prediction accuracy needs to improve before v2 goes live: who owns that work and what's the timeline?
- Alignment on Phase 2 scope and staffing - our team allocation ends at the end of March

Happy to discuss on a call - would tomorrow work?

DELIVERY-LEAD

---

> **Internal notes (do not send)**
>
> Key framing choices:
>
> - Lead with facts, not blame. CLIENT-PRODUCT's QA is described as "thorough" (positive), his findings are cited as evidence that supports our position.
> - Root cause stated clearly but neutrally: model predictions produce less accurate results than historical approach. We don't say "AdScale's data is bad."
> - V1/V2 parallel deploy is framed as the recommended option, not a workaround. It "completes the deliverable per DoD."
> - ZIP parallel deploy is framed as a technical necessity, not a preference - STAGING literally can't handle it.
> - 0.5 CPM bucketing hypothesis included as a constructive suggestion, not an accusation.
> - Phase 2 mentioned as alignment item, not pressure tactic.
> - No mention of the QA gate being a scope change - save that card for if they push back on timeline.
> - Signed by DELIVERY-LEAD per his plan. He sends, then immediately requests a meeting for Tue Mar 17.
>
> Changes from v1:
>
> - Updated with CLIENT-PRODUCT's 20k-scenario findings (much stronger evidence than 901-scenario round)
> - Added specific numbers from CLIENT-PRODUCT's analysis to back every claim
> - Added ZIP section as separate item per ENG-LEAD's input
> - Added 0.5 CPM bucketing hypothesis per ENG-LEAD
> - Shifted "ask for a call" to specific proposal for tomorrow
> - Signed by DELIVERY-LEAD instead of PM per DELIVERY-LEAD's plan
>
> Changes from v2:
>
> - Added truncation compounding as second contributing factor alongside bucketing
> - Framed both as "addressable, but require alignment" - constructive, not accusatory
> - Margin application considered but excluded: it's a flat % applied identically on both sides, so it amplifies the absolute gap but doesn't cause the divergence. Mentioning it would muddy the message.
>
> **Strategic question for DELIVERY-LEAD before sending:**
>
> This email tries to do two things: close Phase 1 cleanly AND open Phase 2. Those pull in opposite directions.
>
> The current tone leans defensive - "we did our job, the gap is on your side." That's the right posture for a clean exit. But if the real goal is continuing the engagement into Phase 2, the email needs to land differently: "we understand your system deeply, we diagnosed root causes you didn't see, we have a practical plan - you want us in the room for what comes next."
>
> Specific things to consider:
>
> - The root cause analysis (bucketing + truncation) is the best sales pitch in the email. It proves Northwind understands AdScale's pipeline at a level a new vendor would need months to reach. Don't bury it.
> - "Our team allocation ends March 31" currently reads as a deadline/threat. If continuity is the goal, reframe as: "we're ready to continue, need to align on scope to plan staffing."
> - The parallel deploy proposal is inherently a relationship move - Northwind solving AdScale's problem, not just delivering scope. That's Phase 2 energy. Lean into it.
>
> The question: is this email meant to close Phase 1, or open the door to Phase 2? The tone, the Phase 2 paragraph, and even the subject line should differ depending on the answer. Worth aligning with DELIVERY-LEAD on intent before finalizing.
