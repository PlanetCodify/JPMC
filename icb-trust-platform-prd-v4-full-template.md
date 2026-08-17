# ICB Trust Platform — Product Requirements Document

**Date:** August 17, 2026
**Author:** Product Team (PoC, Platform Product Manager VP application, JPMC International Consumer Bank Digital)
**Status:** Draft
**Version:** 4.0

**A note on how this was built:** this is a job-application proof of concept, grounded entirely in public sources cited throughout, never in claimed knowledge of JPMorgan Chase's actual internal systems, vendors, or roadmap. It went through real market and competitive research, a synthesized user study (explicitly not primary interviews, flagged as such), and a 7-perspective review panel before landing here. Full source trail in the Appendix note at the end.

---

## 1. Executive Summary

### Problem Statement

International Consumer Bank (ICB) is scaling a branchless, digital-only retail bank across markets, UK live since 2021, Germany publicly targeted for 2026, right as a real regulatory gap opened up. The Federal Reserve's SR 26-2 guidance (April 17, 2026, superseding SR 11-7) explicitly excludes generative and agentic AI from formal model-risk scope for banks over $30 billion in assets. JPMC already runs a dedicated Model Risk Governance & Review (MRGR) function with coverage scoped to ICB, but public evidence (a live MRGR job posting for ICB) suggests that coverage is limited to traditional statistical models, the same boundary SR 26-2 itself draws. Every gen-AI-adjacent feature ICB ships today likely has nowhere clear to go for review.

### Proposed Solution

An automated tiering layer that classifies gen-AI and agentic features by risk (Tier 1 automated, Tier 2 spot-checked, Tier 3 full review) and routes the highest-risk cases into MRGR's existing review workflow. This isn't a new governance function competing with MRGR. It's the technical layer that extends their existing authority into a category their current scope doesn't appear to cover.

### Business Impact

- **Risk reduction:** closes the gap between "SR 26-2 doesn't require formal gen-AI review" and "gen-AI features ship with no review at all," which is the current default state.
- **Competitive positioning:** UK peers (NatWest's Cora, Lloyds' Envoy) are already live with AI-driven customer experiences. A credible, defensible governance story lets ICB ship gen-AI features at a competitive pace without operating in the open gap SR 26-2 left behind.
- **Efficiency for an existing team:** if MRGR is already stretched covering traditional models, an automated filter that only escalates genuinely high-risk cases is a real efficiency gain for a team that already exists, not a new cost center.

### Key Milestones

| Milestone | Target Date |
|---|---|
| Tiering layer + Tier 1 automated checks live, validated with MRGR | End of Q2 |
| MRGR accepts and reviews first Tier 3 escalation through the new routing | Q3 |
| Tier 2/3 governance fully live, CDP/CMS integration decision made | Q4 |
| Full data/comms platform build (if justified by Phase 1-2 data) | Beyond Q4, conditional |

### Success Metrics

| Metric | Current | Target |
|---|---|---|
| Time from first integration attempt to a working classification call | Unmeasured | Under 1 business day |
| 90-day sustained use rate | Unmeasured | Tracked from day one, required check-in before Phase 2 spend |
| Percentage of Tier 3 escalations MRGR accepts and reviews through this routing | 0% (doesn't exist yet) | High enough to prove the "extends MRGR" thesis is real, not just a pitch |

---

## 2. Problem Definition

### 2.1 Customer Problem

**Market launch engineer**
- **Who:** product/engineering teams inside ICB market launches, most immediately the Germany team
- **What:** needs to know what review path a gen-AI feature requires, without inventing the answer themselves
- **When:** every time a feature touches customer data in an AI-adjacent way, which is becoming routine, not rare
- **Where:** internal engineering workflow, not a customer-facing surface
- **Why:** no standard exists because SR 26-2 explicitly declined to set one for gen-AI
- **Impact:** research on comparable internal-platform populations shows 64% of engineers bypass a platform that's slower than just building the workaround themselves. If this tiering layer isn't fast and clear, it won't get used, no matter how sound the governance model is.

**MRGR reviewer**
- **Who:** JPMC's Model Risk Governance & Review function, with a role explicitly scoped to ICB
- **What:** needs a defensible way to review gen-AI features without inventing new controls case by case
- **Why:** their current public scope appears limited to traditional statistical models. Gen-AI features don't have an obvious entry point into their process today.
- **Impact:** every ad hoc review is slower to make and harder to defend later if a regulator or examiner asks why something was approved.

**ICB customer**
- **Who:** digital banking customers in the UK today, prospectively in Germany
- **What:** wants relevant, personalized service, but doesn't fully trust the data trade-off behind it. Published research shows 64% want personalization while only 39% think it's worth the privacy cost.
- **Impact:** 62% of banking customers say they'd lose confidence in their bank after a data breach, and 52% worry AI could mistakenly freeze their account. Getting AI governance visibly right isn't just a compliance box, it's a trust question customers are actively weighing.

### 2.2 Market Opportunity

- **Market Size:** the narrow AI-governance software category (the segment this platform's tiering logic sits in) was an estimated $418 million to $839 million in 2025/2026, growing at roughly 31 to 36 percent CAGR, close to twice the growth rate of the broader, more mature model-risk-management market (about 16 percent CAGR). The customer data platform market this could eventually connect to is separately estimated at $3.5 billion to $12 billion, depending on scope definition.
- **Growth Rate:** the AI-governance category's outsized CAGR relative to the broader MRM market is independent evidence that the SR 26-2 gap is being felt and addressed industry-wide, not just inside this one PRD's framing.
- **Current Solutions:** MRGR handles traditional models for ICB today. No vendor has fully solved gen-AI governance for a bank's internal platform layer yet. ValidMind positions closest to this problem but is scoped to model validation, not gen-AI-specific tiering. Databricks is building an "agentic CDP" concept that validates the same thesis from the vendor side, but has no named bank deployment yet.
- **Why Now:** SR 26-2 is dated and verifiable (April 17, 2026). ICB's Germany launch is publicly targeted for the same year. UK peers are already shipping AI-driven customer experiences, raising the competitive floor for what "good enough" governance looks like while still moving fast.

### 2.3 Business Case

- **Revenue Potential:** not directly quantifiable without real usage data. The more honest framing is risk avoidance and speed: a bank that can ship gen-AI features at a competitive pace, with a defensible governance story, protects both the ability to compete and the downside of getting it wrong.
- **Cost Savings:** likely lower build cost than a new governance function, since this integrates into MRGR's existing expertise and regulatory relationships rather than duplicating them.
- **Strategic Alignment:** JPMC already runs a firmwide Chief Data & Analytics Office (CDAO) that sets data and AI governance strategy, including LLM Suite as a platform product. This platform is designed to extend that existing structure into ICB's gen-AI review gap, not sit outside it.
- **Risk of Inaction:** each market invents its own ad hoc gen-AI review judgment calls, which doesn't scale and creates the exact inconsistency SR 26-2's gap makes expensive if something goes wrong.

---

## 3. Solution Overview

### 3.1 What We're Building

A tiering layer that classifies a gen-AI or agentic feature request into one of three risk tiers and returns the required review path. Tier 1 (low risk, like content copy variants) clears an automated policy check. Tier 2 (medium risk, like personalized messaging using customer data) gets an automated check plus a logged human spot-check. Tier 3 (high risk, anything touching a credit or account decision) routes into MRGR's existing review workflow, with the classifier's full reasoning and an audit trail attached.

### 3.2 In Scope

| Feature | Priority | Description |
|---|---|---|
| Tier 1 automated classification | P0 | Rules-based or lightweight model classification for low-risk features, no human review needed |
| Integrator experience (docs, sandbox, error handling) | P0 | Real quickstart docs, a non-production sandbox, clear errors, and self-serve status so engineers aren't guessing |
| MRGR escalation queue for Tier 3 | P0 | A queue where flagged features land in MRGR's existing process, with reasoning and audit trail attached |
| Audit trail data model | P0 | Defines what makes a Tier 3 record complete: classifier reasoning, reviewer identity and decision, timestamp, data inputs used |
| Fail-safe for classifier unavailability | P0 | Defaults to Tier 2 if the classifier itself is down, logged, never a silent failure |
| Tier 2 policy enforcement plus spot-check | P1 | Automated policy check plus a logged human sample review on a fixed cadence |
| 90-day sustained use tracking | P0 | Tracked from day one so the platform can't pass a speed metric while going unused |
| Fair-lending review of the classifier's self-routing logic | P0 | Required before any build, given the classifier decides its own review tier for credit-adjacent features |
| Customer data, communications, CMS integration | P2 | Deferred to Phase 2 or later, decided only after the governance layer proves itself |

### 3.3 Out of Scope

- **Naming a specific CDP, CMS, or comms vendor.** No public source confirms what JPMC's consumer digital org already runs, so this PRD doesn't commit to a vendor.
- **Replacing or competing with MRGR.** The tiering layer feeds their existing authority. It doesn't stand up a parallel governance function.
- **Committing to a Germany launch date.** Public reporting only confirms "targeted 2026."
- **Building Tier 3 model-validation logic from scratch.** The plan is to route into MRGR's process, and potentially a specialized tool like ValidMind if one already exists inside JPMC's stack, not duplicate that work.
- **Deciding UK-first versus Germany-first sequencing.** Left as an explicit open question. NatWest and Lloyds are already live in the UK market, which makes this a real strategic call, not a detail to bury.

### 3.4 MVP Definition

- **Core Features:** the tiering layer, Tier 1 automated checks, integrator docs and sandbox, and a direct validation conversation with MRGR on whether they'll accept escalations through this routing.
- **Success Criteria:** a market engineer gets a working classification call in under one business day, and MRGR gives a real answer, not an assumption, on whether they want this.
- **MVP Target Date:** end of Q2.
- **Learning Goals:** whether MRGR's actual scope already covers gen-AI (contrary to what one job posting suggests), and whether a fast integration path is enough to prevent the workaround behavior the adoption research warns about.

---

## 4. User Stories & Requirements

### 4.1 User Stories

**As a** market launch engineer,
**I want to** call the tiering layer once and get back a risk tier and review path,
**so that** I don't have to guess what review a gen-AI feature needs or wait on an ad hoc judgment call.

Acceptance Criteria:
- [ ] Classification response returns within the P50/P95 latency budget
- [ ] A stuck-in-review feature has a self-serve way to check status without filing a ticket
- [ ] Ambiguous features default to Tier 2, never silently ship as Tier 1

**As an** MRGR reviewer,
**I want to** receive Tier 3 escalations through a queue with full reasoning and audit trail attached,
**so that** I can review gen-AI features with the same rigor I'd apply to a traditional model, without inventing a new process from scratch.

Acceptance Criteria:
- [ ] Every Tier 3 escalation includes the classifier's reasoning, the data inputs used, and a complete audit trail
- [ ] No Tier 3 feature reaches a customer without an MRGR sign-off recorded
- [ ] The review SLA is stated and tracked, not open-ended

**As an** ICB customer,
**I want to** trust that any AI-driven decision about my account or credit went through a real review,
**so that** I don't have to wonder whether a bank feature was checked by anyone before it reached me.

Acceptance Criteria:
- [ ] No feature that could affect a credit or account decision ships without human review
- [ ] Opt-out choices are honored consistently, tracked as a platform-level metric
- [ ] A customer-facing incident involving an ungoverned AI feature never happens, by design, not by luck

### 4.2 Functional Requirements

| ID | Requirement | Priority | Notes |
|---|---|---|---|
| FR1 | Classify a feature request into Tier 1, 2, or 3 within the latency budget | P0 | Core function of the tiering layer |
| FR2 | Route Tier 3 classifications into MRGR's existing review workflow | P0 | Not a new, parallel review process |
| FR3 | Default to Tier 2 on ambiguous input or classifier unavailability | P0 | Never silently defaults to Tier 1 |
| FR4 | Maintain a complete, machine-checkable audit trail for every Tier 3 decision | P0 | Rollback plan depends on this being verifiable, not just described |
| FR5 | Provide integrator docs, sandbox, and self-serve status | P0 | Directly addresses the adoption-bypass risk from user research |
| FR6 | Log Tier 2 decisions for periodic human spot-check | P1 | Not full review, but not unreviewed either |
| FR7 | Track 90-day sustained use per integrating team | P0 | Prevents the platform from passing a speed metric while going unused |

### 4.3 Non-Functional Requirements

- **Performance:** classification latency P50 under 2 seconds, P95 under 10 seconds.
- **Scalability:** must support at least two markets (UK, Germany) without re-architecture, achieved through a versioned API contract independent of any underlying vendor.
- **Security:** any future data-layer vendor should meet SOC 2 Type II or ISO 27001 minimum, matching the bar set by comparable composable CDP vendors like Segment.
- **Reliability:** the tiering layer's own contract should hit 99.9%+ uptime, tracked independently of any underlying vendor's performance.
- **Usability:** integrator docs, sandbox, and error messages need to be clear enough that a market engineer doesn't need to file a ticket to understand what happened.
- **Compliance:** GLBA data-safeguarding and opt-out requirements, applicable UK and, pending Germany's launch, EU/German data protection law, and a fair-lending (ECOA/Reg B) review of the classifier's self-routing logic before any feature touching credit decisions ships. CFPB Section 1033 status is currently unsettled (enjoined, under reconsideration), so the data layer shouldn't assume its final form.

---

## 5. Go-to-Market Strategy

### Launch Plan

- **Beta:** Phase 1 ships to one pilot team, likely the Germany launch team given the active timeline, with MRGR directly involved in validating the escalation routing before anything scales further.
- **Full Launch:** Phase 2 and 3 expand Tier 2/3 governance and open the CDP/CMS integration question, contingent on Phase 1 proving both integration speed and MRGR's actual acceptance of the routing.
- **Enablement:** since this is an internal platform, "marketing" means enablement, real docs, a working sandbox, and a direct pitch to MRGR framed as extending their reach, not adding to their workload.
- **Support:** self-serve status checking and an error taxonomy clear enough that most integration issues don't require a support ticket.

### Pricing

Not applicable. This is an internal platform, funded through CCB/ICB's existing technology budget rather than priced to any external customer.

---

## 6. Risks & Mitigations

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| MRGR doesn't want an automated layer feeding into their process | Medium | High, this is the core bet of the whole platform | Validate directly with MRGR in Phase 1 before any further build. If they decline, fall back to a lightweight, standalone decision-logging tool instead. |
| Engineers bypass the platform because it's slower than a workaround | Medium-High, based on comparable research | High, the governance layer has nothing to govern if unused | Ship a fast, real integration experience in Phase 1 as a hard gate, not an afterthought |
| The classifier's self-routing logic fails a fair-lending review | Medium | High, real legal exposure | Fair-lending review required before any build touching credit-adjacent classification |
| MRGR's scope already covers gen-AI, contrary to the one job posting this thesis relies on | Low-Medium | Low if caught early, since the build simply shrinks to a smaller integration | Confirm directly with MRGR or CDAO before committing further resources |
| Time-to-first-integration looks good but sustained use doesn't follow | Medium | Medium | Track 90-day reuse from day one, require a real check-in before Phase 2 spend |
| SR 26-2's gen-AI exclusion gets revised, changing the regulatory footing | Low-Medium | Medium | Review the governance model on a proposed semi-annual cadence, don't treat it as permanent |

---

## 7. Timeline & Milestones

| Milestone | Date | Deliverables | Success Criteria |
|---|---|---|---|
| Foundation | End of Q1 | Tiering layer architecture, API contract, audit trail data model defined | Architecture reviewed with Engineering |
| Phase 1 complete | End of Q2 | Tier 1 live, integrator docs and sandbox live, MRGR validation conversation held | Time-to-first-integration under 1 day, real answer from MRGR |
| Phase 2 complete | End of Q3 | Tier 2/3 live and routed into MRGR's workflow, CDP/CMS decision made, UK-vs-Germany sequencing resolved | 90-day reuse data reviewed, Legal sign-off on cross-border data handling if platform work proceeds |
| Phase 3 complete | End of Q4 | Full platform build if justified, customer-trust dashboard, dedicated consent UX | Reuse rate and governance-acceptance metrics both trending in the right direction |

---

## 8. Team & Resources

*Illustrative allocation, not a claim about actual JPMC staffing.*

| Role | Allocation |
|---|---|
| Product Manager (Platform PM, this role) | 1 FTE |
| Engineering Lead / Tech Lead partner | 1 FTE, co-owns architecture per the JD's stated partnership model |
| Backend/platform engineers | 2-3 FTEs for Phase 1's narrower governance-only scope |
| MRGR liaison | Partial allocation, not embedded, but a named point of contact from Phase 1 onward |
| Legal (fair lending, cross-border data) | Partial allocation, gated review before build, not continuous |

**Budget:** not estimated. A real number would require internal engineering cost data and confirmation of whether any existing vendor relationship (CDP, model-validation tooling) already exists, neither of which is public.

---

## 9. Open Questions

1. Does MRGR's actual scope for ICB already cover gen-AI and agentic AI, contrary to what one job posting suggests? This is checkable, not a guess, for anyone with access to ask.
2. Would MRGR want an automated triage layer feeding into their existing process, or see it as unnecessary or intrusive?
3. Does the model-risk process Tier 3 escalations route into actually have scope to cover gen-AI outputs feeding credit decisions?
4. Does JPMC's engineering culture, given its MACH Alliance membership and existing LLM Suite platform, mean general platform-adoption research (like the 64% bypass figure) doesn't fully transfer here?
5. Does German consumer attitude toward AI and data privacy differ from the UK/US research this PRD leans on, given GDPR-native expectations?
6. Should this sequence UK-first, given NatWest's Cora and Lloyds' Envoy are already live, or proceed Germany-parallel as originally planned?
7. Should this plug into CDAO's existing firmwide data-product discipline, rather than standing alone as a separate initiative?

## 10. Assumptions Made

- Assumed MRGR's public job posting accurately reflects their current scope. It's one source, not a policy document, and could lag reality.
- Assumed no existing internal JPMC CDP, CMS, or comms vendor was found publicly, and treated this as a genuine unknown rather than evidence of absence.
- Assumed Germany is the likely first pilot market given the active public launch timeline, though UK-first is left as an explicitly open, unresolved question.
- Cost, budget, and staffing figures in Section 8 are illustrative placeholders, not derived from real JPMC financial or headcount data.
- Assumed the 8-12 gen-AI-adjacent features per year estimate, sourced from peer-bank public roadmap language, is directionally useful despite not being an ICB-specific signal.
- Assumed the general 64% platform-bypass adoption statistic is a reasonable prior for JPMC's engineering culture, while flagging in Open Questions that this may not fully transfer given JPMC's existing platform maturity.

---

**Full research and process trail:** [01_research_notes.md](01_research_notes.md), [market-research-icb-trust-platform-2026-08-17.md](market-research-icb-trust-platform-2026-08-17.md), [user-research-synthesis-icb-trust-platform-v2.md](user-research-synthesis-icb-trust-platform-v2.md), [competitor-analysis-icb-trust-platform-v2.md](competitor-analysis-icb-trust-platform-v2.md), [icb-trust-platform-review-synthesis.md](icb-trust-platform-review-synthesis.md), [icb-trust-platform-prd-final.md](icb-trust-platform-prd-final.md).
