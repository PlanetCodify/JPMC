# ICB Trust Platform — Product Requirements Document

**Date:** August 17, 2026
**Author:** Product Team (PoC — Platform Product Manager, VP application, JPMC International Consumer Bank Digital)
**Status:** Draft — illustrative PoC, not a real JPMC initiative
**Version:** 3.0 — supersedes [04_final_strategy_doc_v2.md](04_final_strategy_doc_v2.md); synthesizes [market](market-research-icb-trust-platform-2026-08-17.md), [user](user-research-icb-trust-platform-2026-08-17.md), and [competitive](competitor-analysis-icb-trust-platform-2026-08-17.md) research completed since v2.

**Author's note**: Grounded entirely in public sources (see [01_research_notes.md](01_research_notes.md) for the full citation trail). Makes no claim to insider knowledge of JPMC's actual systems, vendors, spend, or roadmap. Where this document states something as fact, it is independently verifiable; where it makes an assumption, it says so.

**What changed since v2** (driven by new research, not just restated): (1) roadmap resequenced so day-one engineering experience ships before/alongside governance — adoption research shows that's the bigger risk; (2) added a customer-trust metric, not just a personalization-adoption metric; (3) governance is now pitched as something Risk & Compliance would *want*, not just tolerate; (4) vendor recommendation is now concrete (composable CDP) instead of abstract; (5) UK-first vs. Germany-parallel sequencing is surfaced as a genuinely open question instead of silently keeping the old RICE-only call.

---

## 1. Executive Summary

### Problem Statement
International Consumer Bank (ICB) is scaling a branchless, fully digital retail bank across markets — UK live since 2021, Germany publicly targeted for 2026. Each new market likely rebuilds the same customer data, communications, and content capability from scratch (inference from ICB's public roadmap). Compounding this, the Fed's April 2026 SR 26-2 guidance explicitly excluded generative and agentic AI from formal model-risk scope for banks over $30B in assets — so any AI-adjacent feature a market adds has no standard governance path. New research adds a sharper edge: **the platform's biggest real risk isn't regulatory or executive — it's that engineers route around a platform that's slower than the workaround** (a documented pattern in 45.3% of internal-platform initiatives generally), which would starve the governance layer of anything to actually govern.

### Proposed Solution
A shared platform — a versioned API contract, a composable data/comms/CMS integration layer, and a callable AI-governance service — that any ICB market consumes instead of rebuilding. Sequenced so the first thing an engineer experiences is speed, not a compliance gate.

### Business Impact
- **Cost**: reuse across markets should drive cost-per-market-onboarded down over time (the platform's core leverage thesis; status quo per-market builds are the actual baseline being beaten, per competitive analysis §6).
- **Competitive**: UK-market peers (NatWest's Cora, Lloyds' Envoy) are already live with AI-driven customer experiences — this platform is table-stakes infrastructure to compete, not a differentiator on its own.
- **Regulatory**: closes a real, dated governance gap (SR 26-2) with an internal standard before an incident forces a reactive one.

### Key Milestones
| Milestone | Target |
|---|---|
| Platform API contract + composable CDP integration live (engineering-first) | Q1 |
| Governance Tier 1 (automated) live, in parallel | Q2 |
| First market fully onboarded (sequencing TBD — see §9 Open Questions) | Q3 |
| Tier 2/3 governance + full metrics suite live | Q4 |

### Success Metrics
| Metric | Current (assumed baseline) | Target |
|---|---|---|
| Time-to-first-live-integration for a new market team | Unmeasured (assumed multi-week custom build) | <1 business day for a first data sync |
| Reuse rate | 0% (no shared platform exists) | 65%+ by Q4 |
| API SLA adherence | N/A | 99.9%+ |
| Governance review cycle time (Tier 1) | Assumed 3+ days (ad hoc, per user research Finding 2) | <8 hours |
| **Customer trust proxy (new in v3)** | Unmeasured | Opt-out rate on AI-driven comms stays flat or improves post-launch, not just personalization-adoption rate rising |

---

## 2. Problem Definition

### 2.1 Customer Problem

**Internal platform user (market launch team)**
- **Who**: product/engineering team inside a new ICB market launch.
- **What**: needs data/comms/CMS infrastructure fast, under a launch deadline, without inventing governance for every AI-adjacent feature.
- **When**: acute at every new market launch; chronic for any team wanting to ship an AI-adjacent feature.
- **Where**: internal engineering workflow, not a customer-facing surface.
- **Why**: no shared platform exists today (inferred); SR 26-2 leaves no federal AI-governance template to copy.
- **Impact**: per user research, **64% of engineers bypass internal platforms slower than the workaround** — this is a real, cited failure mode, not a hypothetical.

**Risk & Compliance stakeholder**
- **Who**: the function approving AI-adjacent customer-facing features.
- **What**: needs a standardized, defensible review path.
- **Why**: SR 26-2 explicitly excludes gen-AI/agentic AI from formal MRM scope, leaving reviews ad hoc today.
- **Impact**: per user research Finding 4, this is plausibly a pain Risk wants solved, not a constraint being imposed on them — reframes the internal sales pitch materially.

**End customer**
- **Who**: ICB digital banking customer, UK (live) or Germany (prospective).
- **What**: wants relevant communications but doesn't trust the trade-off yet — 64% want personalization, only 39% think it's worth the privacy cost; 52% fear AI could mistakenly freeze their account; 62% would lose confidence in the bank after a breach.
- **Impact**: the platform's consent-store and Tier-3 human-in-the-loop design are proportionate responses to these cited fears, not over-engineering.

### 2.2 Market Opportunity
- **Market size**: CDP market $3.5B–$12B (2026, range across sources), 20–34% CAGR; narrow AI-governance software $418M–$839M, growing ~31–36% CAGR — roughly **2x the growth rate of the broader model-risk-management market**, independent evidence the governance gap is real and industry-wide, not a JPMC-specific framing device. [See market research for full sourcing.]
- **Growth rate**: see above; martech-in-banking growing alongside — 47% of banks attribute up to 30% of revenue growth to martech investment.
- **Current solutions**: fragmented CDP/CMS vendor market (Adobe strongest banking track record; Segment strongest architectural fit); no vendor has "won" gen-AI bank governance yet — genuine whitespace.
- **Why now**: SR 26-2 (April 2026) is dated and verifiable; Germany launch is publicly targeted for 2026; UK peers are already shipping AI-driven customer experiences, raising the competitive floor.

### 2.3 Business Case
- **Cost savings**: cost-per-market-onboarded should trend down with reuse — the platform's real economic case, measured against the do-nothing baseline (competitive analysis §6).
- **Strategic alignment**: extends JPMC's own LLM Suite precedent (230K+ employees, 450+ use cases) into the specific internal platforms this JD's team owns, rather than duplicating firm-wide AI infrastructure.
- **Risk of inaction**: each market invents its own AI governance judgment calls, which doesn't scale and risks inconsistent customer protection — the exact inconsistency SR 26-2's gap makes costly.

---

## 3. Solution Overview

### 3.1 What We're Building
A platform exposing three things any ICB market team consumes: (1) a versioned API contract over a composable customer-data/comms/CMS integration layer, (2) a consent store as the one deliberate synchronous exception in an otherwise event-driven design, and (3) a callable AI-Governance & Policy Engine returning a risk tier (Tier 1/2/3) and required review path.

### 3.2 In Scope
| Feature | Priority | Description |
|---|---|---|
| Platform API contract (versioned) | P0 | Market teams integrate once; vendor swaps happen behind the contract, invisible to consumers. |
| Composable CDP integration (Segment-class vendor) | P0 | Buy the data-unification layer; don't build what the market already sells well. |
| Consent store (sync write) | P0 | Single source of truth for opt-out state before any comms send — the platform's actual hard compliance problem. |
| AI Governance & Policy Engine, Tier 1 (automated) | P0 | Ships in parallel with the foundation, not after — highest RICE score, addresses Risk's real pain (Finding 4). |
| Event bus / async fan-out to CDP, comms, CMS | P1 | Standard integration pattern for non-consent data. |
| AI Governance Tier 2/3 (spot-check, human-in-the-loop) | P1 | Full tiering live once Tier 1 proves the pattern. |
| ValidMind-class integration for Tier 3 model validation | P2 | Call into existing specialized tooling rather than rebuilding model-validation from scratch (competitor analysis §4). |
| Customer-trust dashboard (opt-out/complaint tracking) | P1 | New in v3 — tracks the metric user research flagged as missing. |

### 3.3 Out of Scope
- Specific vendor contract negotiation/selection — this PRD recommends a *category* (composable CDP), not a named procurement decision.
- Replacing any existing JPMC CDP/CMS if one already exists internally (unconfirmed) — platform should integrate with it via the versioned-API pattern, not assume greenfield.
- A committed Germany launch date — public reporting only confirms "targeted 2026."

### 3.4 MVP Definition
- **Core features**: Platform API contract + composable CDP integration + consent store + Tier 1 governance, all shipping together in Phase 1 (resequenced from v2, where governance followed the full foundation build).
- **Success criteria**: a market engineering team achieves a live data sync in under 1 business day — the metric most directly targeting the adoption-bypass risk from user research.
- **Learning goal**: whether engineers actually choose the platform over a workaround when it's fast, before governance maturity is tested at all.

---

## 4. User Stories & Requirements

### 4.1 User Stories

> As a market launch engineer, I want to integrate customer data/comms/CMS through one versioned API, so that I don't rebuild integration logic every market and don't lose time to a vendor swap happening behind me.
Acceptance Criteria:
- [ ] First live data sync achievable in <1 business day from platform onboarding start
- [ ] No market-team code changes required when the platform swaps an underlying vendor
- [ ] Consent state read is available synchronously with <200ms latency

> As a Risk & Compliance reviewer, I want a standardized, tiered review pathway for AI-adjacent features, so that my reviews are faster and more defensible than today's ad hoc process.
Acceptance Criteria:
- [ ] Every AI-adjacent feature request receives an automatic Tier 1/2/3 classification
- [ ] Tier 1 requires no human review; Tier 2 logs a spot-check sample; Tier 3 routes into existing model-risk process
- [ ] Full audit trail retrievable per feature, per market

> As an ICB customer, I want relevant communications without worrying my data is mishandled, so that I trust the bank enough to keep engaging.
Acceptance Criteria:
- [ ] Opt-out honored across all channels within the same session (consent store propagation)
- [ ] No Tier 3 (credit/account-decision) feature ships without human sign-off
- [ ] Opt-out rate tracked and reported as a platform-level metric, not buried in a single market's dashboard

### 4.2 Functional Requirements
| ID | Requirement | Priority | Notes |
|---|---|---|---|
| FR1 | Versioned platform API contract, independent of underlying vendor | P0 | Core to the "buy CDP, build governance" strategy |
| FR2 | Consent store, synchronous write, single source of truth | P0 | The one deliberate exception to async-by-default |
| FR3 | Event-bus fan-out for non-consent data writes | P1 | Standard async pattern |
| FR4 | AI Governance & Policy Engine, callable service, Tier 1 automated | P0 | Ships alongside FR1/FR2, not after |
| FR5 | Tier 2 (policy enforcement + logged spot-check) | P1 | Follows Tier 1 validation |
| FR6 | Tier 3 (human-in-the-loop, routes to model-risk process) | P1 | May call into a ValidMind-class tool rather than rebuild |
| FR7 | Customer-trust dashboard (opt-out/complaint tracking) | P1 | New in v3, closes the metrics gap user research identified |

### 4.3 Non-Functional Requirements
- **Performance**: consent-state reads <200ms; async propagation to CDP/comms/CMS within a defined SLA (not yet specified — flagged as open question).
- **Scalability**: platform must support at least 2 markets (UK, Germany) without re-architecture; API versioning is the mechanism that makes this true.
- **Security**: vendor selection requires SOC 2 Type II / ISO 27001 minimum (per competitive analysis, matching Segment's certification bar).
- **Reliability**: 99.9%+ API SLA adherence, measured on the platform's own contract, independent of underlying vendor performance.
- **Compliance**: GLBA, applicable US state privacy law, UK data protection law, Germany/EU GDPR — no single compliance model assumed to work everywhere; Legal is a hard per-market gate, not a formality.

---

## 5. Go-to-Market Strategy

### Launch Plan
- **Beta**: Phase 1 (Q1) ships to one internal team first — the market with the most launch pressure, sequencing TBD (see Open Questions).
- **Full rollout**: Phase 3 (Q3) target, contingent on Phase 1's adoption metric (time-to-first-integration) actually hitting target — a hard internal gate, not a calendar-only milestone.
- **Enablement**: platform docs and a fast onboarding path are treated as P0 product surface, not an afterthought, directly responding to the 64%-bypass adoption research.

### Pricing
Not applicable — internal platform, cost allocated via CCB/ICB technology budget, not priced to consumers.

---

## 6. Risks & Mitigations

| Risk | Probability | Impact | Mitigation |
|---|---|---|---|
| Engineers bypass the platform because it's slower than a workaround | High (64% base rate in comparable research) | High — governance layer has nothing to govern if unused | Resequenced roadmap ships a fast integration experience in Phase 1, before governance maturity; time-to-first-integration is a hard MVP gate |
| SR 26-2's AI exclusion is revised, changing the governance model's regulatory footing | Medium | Medium | Governance model reviewed on a proposed semi-annual cadence; platform doesn't lean on SR 26-2 as sole justification (multi-market reuse case stands independently) |
| CFPB Section 1033 status changes (currently enjoined) | Medium | Medium | Data layer built flexible to open-banking API requirements without assuming the rule's final form |
| UK customers already expect AI-driven banking (NatWest/Lloyds live) and this platform's first customer-visible output lags that bar | Medium-High | Medium | Explicit open question (§9) — may require UK-first resequencing, not assumed away |
| Vendor selection (composable CDP) turns out to duplicate an existing internal JPMC system | Unknown (no public confirmation either way) | High if true | Versioned-API design specifically absorbs this risk — a vendor swap or integration-not-replacement pivot happens behind the contract |
| Risk & Compliance doesn't actually want a standardized tiering model, prefers case-by-case discretion | Low-Medium (inference, unvalidated) | High if true — would undercut the core governance pitch | Flagged as the single highest-priority validation question before Phase 2 governance work is built out |

---

## 7. Timeline & Milestones

| Milestone | Target | Deliverables | Success Criteria |
|---|---|---|---|
| Foundation + Governance Tier 1 (parallel) | Q1–Q2 | API contract, composable CDP integration, consent store, Tier 1 automated checks | Time-to-first-integration <1 day; Tier 1 review cycle <8hrs |
| First market onboarding | Q3 | Full platform onboarding for one market | Market-specific compliance sign-off (Legal hard gate) |
| Tier 2/3 governance + full metrics | Q4 | Spot-check + human-in-the-loop tiers live; trust dashboard live | Reuse rate 65%+; opt-out rate flat or improved |

---

## 8. Team & Resources

*Illustrative allocation — not a claim about actual JPMC staffing.*

| Role | Allocation |
|---|---|
| Product Manager (Platform PM, this role) | 1 FTE |
| Engineering Lead / Tech Lead partner | 1 FTE, co-owns architecture decisions per JD |
| Backend/platform engineers | 3–4 FTEs |
| Risk & Compliance liaison | Partial allocation, not embedded FTE |
| Vendor management / procurement | Partial allocation |

**Budget**: not estimated — would require actual vendor quotes (composable CDP licensing) and internal engineering cost data neither of which is public.

---

## 9. Open Questions

1. **UK-first vs. Germany-parallel sequencing** — the original RICE-only roadmap ran Germany onboarding in parallel with the foundation build. New competitive research (NatWest's Cora, Lloyds' Envoy already live in the UK) suggests UK-first might close a competitive gap faster than a Germany-launch-driven sequence. This is now a genuinely open strategic call, not a settled one — needs real stakeholder input to resolve.
2. **Does JPMC already run a CDP/CMS/comms vendor internally?** Unconfirmed. If yes, this platform's design (integrate via versioned API) should absorb that reality with minimal rework; if no, the composable-CDP recommendation stands as proposed.
3. **Would actual Risk & Compliance stakeholders want a standardized tiering model, or prefer discretion precisely because SR 26-2 doesn't mandate otherwise?** Highest-leverage assumption to validate before Phase 2 (Tier 2/3 governance) is built — user research flags this as an inference, not a confirmed preference.
4. **Is there an existing ValidMind-class (or comparable) model-validation tool already in JPMC's stack** that Tier 3 should call into, rather than this platform building overlapping model-validation logic?
5. **What's the real internal appetite/budget for a platform-layer investment of this scope** — JPMC CCB's ~$7B/year tech budget is public, but this platform's specific fundability within that isn't.

## 10. Assumptions Made

- Assumed ICB markets currently rebuild data/comms/CMS capability per launch — inferred from ICB's public multi-market roadmap, not confirmed internally.
- Assumed no existing internal JPMC CDP/CMS was named publicly — treated as a genuine unknown, not evidence of absence.
- Assumed Risk & Compliance's current process is ad hoc post-SR-26-2 — a reasonable inference from the regulation's own text, not confirmed against JPMC's specific internal practice.
- Cost, budget, and staffing figures in §8 are illustrative placeholders, not derived from any real JPMC financial data.
- Assumed a composable, Segment-class CDP is the right buy-side recommendation over a full Adobe-suite commitment — based on architectural fit with the platform's own build strategy, not a formal vendor bake-off.
