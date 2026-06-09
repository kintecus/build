---
date created: 2026-02-18 20:59
updated: 2026-03-16 09:22
---

# AdScale - Brief

> Generated from onboarding interview on 2026-02-18. Update as understanding evolves.

## One-liner

Add ZIP-level forecasting and audience-based recommendation to AdScale's Media Planning Tool (MPT) - delivered as a 2-month backend API pilot by Northwind to enhance AdScale's MPT.

## Background

AdScale is a privacy-first ad exchange operating across 25+ countries. Their differentiator is "Audience-Driven Advertising" - anonymized behavioral audiences built from contextual signals (domains, apps, ZIP codes), not individual user data.

Their Media Planning Tool (MPT) is used by Customer Success Managers (CSMs) and Sales to estimate impression volume at a given CPM under targeting constraints. The MPT was built as a 3-month PoC in early 2025 and pushed directly into production. Its core failure mode: when it cannot find sufficient impression volume for a selected audience/targeting combination, it blocks the campaign instead of suggesting alternatives. This is the #1 reported pain point - it forces CSMs to down-sell or abandon campaigns, directly reducing AdScale's revenue.

Northwind won a competitive RFP in Aug-Sep 2025. Six alignment calls reduced the original 9-feature, 32-week scope by ~65%. The engagement is structured as a Phase 1 pilot proving forecasting and recommendation capability, with Phases 2-4 targeted for Q2-Q3 2026.

## Goals and success criteria

**Phase 1 deliverables:**

1. **Forecast Service** - New API that consumes ZIP-level geo data integrated into audience scores, model predictions datafeed integration (impression rate, CTR, VCR, viewability), and CPM margin guidance. Returns CPM-to-impression curves with KPI predictions per placement. Must support multi-audience combination in a single request.
2. **Recommender** - When forecast volume falls below the configured threshold (50% of budget at up to 1.5× CPM), recommends audience expansion via semantic similarity + reach expansion (weighted) to unblock the campaign. Calls the Forecast Service 3-5 times per request to evaluate alternatives.

**Success criteria:**

- Forecast Service returns results in < 5s end-to-end with ZIP-level geo data integrated
- CSMs receive ranked alternative audience suggestions instead of "not enough volume" blocks
- Forecast Service and Recommender are documented and handed off to the AdScale One frontend team (AdTools lead: CLIENT-FRONTEND-LEAD) for integration
- Phase 1 accepted by CLIENT-PRODUCT, unlocking Phase 2 contract

## Stakeholders

**AdScale:**

- **CLIENT-PRODUCT** - Director, Product / Product Owner. Primary decision-maker. Technically strong, engages same-day, directly supportive of the engagement. The single most critical relationship on this project.
- **CLIENT-DS-LEAD** - Data Scientist. Main technical counterpart for model integration. Responds same-day ~80% of the time but is a single point of failure (SPOF) - nearly every technical thread routes through him. His unavailability blocks work.
- **CLIENT-VP-DS** - VP Data Science. Oversees the audience team. Drove the decision to keep all ML models as AdScale IP (not transferred to Northwind).
- **CLIENT-PRODUCT-MGR** - Data Science Team (fka Delivery Optimization). Handles PR reviews and code merges.
- **CLIENT-FRONTEND-LEAD** - AdTools Lead (software engineering), AdScale One frontend. Owns the integration surface.
- **CLIENT-PRODUCT-BIZ** - Role TBC. Referenced for API V2 contract product view clarification.
- **CLIENT-PERSONA-LEAD** - Audience team. Confirmed by CLIENT-PRODUCT as point of contact.
- **CLIENT-VP-DS** - Jira/project admin.
- **CLIENT-DEVOPS-1** ==???== - DevOps/Infra.
- **CLIENT-DEVOPS-2** ==???== - DevOps/Infra. Also involved in S3/IAM access provisioning.
- **CLIENT-DATA-PLATFORM** - Data Platform.

**Northwind:**

- **PM** - TPM / Scrum Master (0.5 FTE, that's me)
- **ENG-LEAD** - Sr Backend Engineer, Python (1.0 FTE)
- **ENG-1** - Sr Data Engineer, Python (1.0 FTE)
- **ENG-2** - Data Scientist (1.0 FTE)
- **DELIVERY-LEAD** - Director of Solution Engineering / Account Manager (oversight)

**Political dynamics:** CLIENT-PRODUCT is an ally - technically strong PO who resolves blockers directly. CLIENT-DS-LEAD is collaborative but a bottleneck by volume, not intent. Inside Northwind, there is tension between a customer-focused delivery approach and a minimal-scope / upsell orientation from some colleagues. DELIVERY-LEAD expects ceremony management but limits PM's ownership footprint.

## Constraints

- **Budget**: ~$80k fixed-scope pilot, 2 months
- **Timeline**: Kickoff Jan 15. Target completion mid-March. PM has flagged a 1-2 week slip as probable due to onboarding friction and upstream data dependencies (see Risks). Phase 1 extended to end of March (soft-aligned Mar 2). 1.2A+1.2B batched for W11 delivery. 1.3 deadline: Mar 31.
- **Allocation**: PM at 0.5 FTE (20 hrs/week) split across AdScale and a second engagement.
- **Technical**: < 5s end-to-end latency. 9.1B audience score rows create a cardinality explosion when ZIP-level geo data is integrated. ClickHouse migration in progress (from Redshift).
- **Organizational**: Access and tooling on the AdScale side have multiple owners (DevOps, Data Platform, Jira admin), each resolving issues independently and slowly. AdScale provides workarounds rather than root-cause fixes.

## Risks and concerns

- **Delivery timeline slip** (HIGH) - Two compounding causes: (1) onboarding friction - tools, access, and environments have separate AdScale owners, each slow to resolve; (2) dependency on CLIENT-DS-LEAD delivering batch prediction datasets, with no committed ETA. PM's current estimate: 1-2 week slip from mid-March target.
- **CLIENT-DS-LEAD as SPOF** (MEDIUM) - Nearly all technical decisions route through one person. If CLIENT-DS-LEAD is unavailable for more than 2-3 days, active development blocks. Mitigation: build direct relationship with CLIENT-PRODUCT-MGR; request named backup from CLIENT-PRODUCT.
- **"Sufficient volume" threshold** - ~~HIGH~~ RESOLVED (Feb 18). Confirmed: 50% budget threshold + 1.5× CPM multiplier, both configurable.
- **Audience cardinality explosion** (MEDIUM) - 9.1B rows × ZIP codes exceeds the 5s latency target under naive query patterns. Mitigation: pre-aggregation, sampling, ClickHouse query optimization.
- ~~**Batch prediction data not yet delivered**~~ **RESOLVED Feb 19** - All 4 model prediction CSVs received and archived to staging S3.

## My role

DELIVERY-LEAD's official framing is Scrum Master: facilitate ceremonies, don't own delivery or product decisions. In practice, a 2-month fixed-scope pilot doesn't need robust Agile process - it needs three things: clear phase goals with target dates, frequent and precise stakeholder communication, and fast blocker resolution. That's closer to TPM work than SM work, but it's what the project requires to hit mid-March.

My actual focus: define and lock phase outcomes with dates (jointly with CLIENT-PRODUCT), participate in weekly syncs that produce written decisions rather than just conversation, and surface blockers to DELIVERY-LEAD or CLIENT-PRODUCT before they compound. I won't wait for DELIVERY-LEAD to authorize delivery ownership - I'll operate within the SM remit while doing what the project needs, and document the gap if it becomes a problem.

## Open questions

- ~~What is the specific volume threshold that triggers the Recommender?~~ **Resolved Feb 18**: 50% budget threshold + 1.5× CPM benchmark, both configurable. Confirmed by CLIENT-PRODUCT.
- ~~When will batch prediction datasets be delivered?~~ **Resolved Feb 19**: all 4 CSVs received from CLIENT-DS-LEAD, archived to staging S3.
- Does "AdScale One UI team" mean AdTools engineers (CLIENT-FRONTEND-LEAD) or UX designers? CLIENT-PRODUCT asked for clarification - most likely both, at different phases.
- What is the formal timeline impact of onboarding delays? PM to quantify and communicate to DELIVERY-LEAD and CLIENT-PRODUCT.
