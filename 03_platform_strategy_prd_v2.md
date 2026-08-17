# ICB Trust Platform — Platform Strategy Document (v2)
### A Shared Customer Data, Communications & Content Platform for International Consumer Bank Markets

**Author's note**: This is an illustrative product strategy document prepared as part of a job application for the Platform Product Manager (VP) role, International Consumer Bank Digital team. It is grounded entirely in publicly available information (see [01_research_notes.md](01_research_notes.md)) and does not claim knowledge of JPMorgan Chase's actual internal systems, vendors, or roadmap. Confidence language in this document intentionally matches the confidence level of the underlying research — where something is inferred rather than confirmed, it is marked as such throughout, not just in a disclaimer.

---

## 1. Problem Statement

International Consumer Bank (ICB) is scaling a fully digital, branchless retail banking model across multiple countries — Chase UK live since 2021, Germany publicly targeted for 2026. Each new market launch plausibly requires the same underlying capability: customer data integration, communications infrastructure, and content management, built under that market's specific privacy and data-residency law, on a launch-driven timeline. Without a shared platform, this is likely rebuilt market-by-market — a reasonable inference from ICB's public multi-market roadmap, not a confirmed internal fact.

A second, compounding condition raises the cost of *not* having a shared platform now: in April 2026, the Federal Reserve's SR 26-2 guidance (superseding SR 11-7) explicitly excluded generative and agentic AI from formal model-risk scope for banks over $30B in assets. Any AI-adjacent capability a market team wants to add to its digital channels — personalized messaging, content recommendations — currently has no standardized federal governance model to plug into. That's a real, dated, verifiable regulatory fact; it is not, by itself, the reason this platform should exist.

**The core problem, stated once**: ICB's multi-market expansion needs a shared platform for customer data, communications, and content — one durable enough to survive without any single regulatory trigger, but designed so that adding a governed AI capability to it is a configuration, not a re-architecture.

## 2. Stakeholder Map (new in v2)

| Stakeholder | Role | RACI on platform decisions |
|---|---|---|
| Platform PM (this role) | Owns vision, roadmap, cross-market prioritization | Accountable |
| Market launch team (e.g. Germany) | Consumes the platform, surfaces local requirements | Responsible (for their integration), Consulted (on roadmap) |
| Risk & Compliance | Reviews AI-adjacent features, sets data-handling policy | Consulted on roadmap, Accountable on governance sign-off |
| Legal | Interprets local privacy law (GLBA, UK/Germany data protection) | Consulted |
| Vendor Management / Procurement | Owns commercial terms with CDP/comms/CMS vendors | Responsible for contracts, Consulted on vendor selection |
| CCB/ICB Technology leadership | Funds platform, sets firm-wide architecture standards (e.g. MACH) | Informed, Accountable on funding |
| Engineering (Tech Lead partner) | Designs integration patterns, APIs, system architecture with PM | Responsible, Accountable jointly with PM on technical design |

*Note: this is a plausible RACI given the JD's stated stakeholder set ("Product, Engineering, and other business teams") — not a claim about Chase's actual governance structure.*

## 3. User Personas

| Persona | Need |
|---|---|
| Internal platform user (market launch team) | Reusable integration pattern for data/comms/CMS — avoid rebuilding per market |
| Risk & Compliance | Standardized, auditable review pathway for AI-adjacent features |
| End customer | Consistent, relevant communications with data handled uniformly across markets |

## 4. Jobs to Be Done

1. When a new market launches, give the team a reusable, versioned integration pattern for data/comms/CMS.
2. When a product team wants to add an AI-adjacent feature, give them a pre-defined governance pathway rather than a bespoke review.
3. When compliance reviews an AI feature, give them a consistent, cross-market audit trail.
4. When a vendor needs to be swapped (pricing, capability, consolidation), the platform absorbs the change without breaking market-team integrations.

## 5. Architecture — Integration Pattern Decisions (rebuilt in v2)

This section previously listed vendor categories in a box diagram — that's a taxonomy, not architecture. Below are the actual design decisions a platform of this kind requires, at black-box level (no claim about Chase's real implementation):

**5.1 Sync vs. async, by data type**
- Customer profile reads (a market team checking current consent state before sending a message): synchronous API, low-latency, cacheable with short TTL.
- Data writes and cross-system propagation (a new consent choice, an updated address): asynchronous, event-driven (publish once, fan out to CDP/comms/CMS subscribers) — avoids the platform becoming a synchronous bottleneck across three downstream systems, and means one slow vendor doesn't block the other two.

**5.2 Consent propagation — the platform's actual hard problem**
When a customer opts out in one channel, every other system needs to reflect that before the next send, not eventually. Design decision: consent state is the one entity written *synchronously* to a central store before any comms send is permitted, even though other propagation is async — deliberately breaking the "everything is async" pattern for the one case where staleness creates real compliance risk (sending to an opted-out customer under GLBA/local law).

**5.3 API versioning and backward compatibility**
Market teams integrate once and shouldn't need to re-integrate every time the platform adds a market or swaps a vendor. Decision: version the platform's own API contract (not the vendor's), so a vendor swap behind the API is invisible to consuming market teams — this is what actually delivers "buy the CDP, build the governance/integration layer" as a real technical strategy rather than a one-line aspiration.

**5.4 Idempotency and retry semantics**
Communications sends must be idempotent (a retried request after a timeout doesn't double-send a customer message) — standard for any comms-adjacent platform, worth stating explicitly because it's a common real-world failure mode in exactly this kind of system.

**5.5 Where AI governance actually sits**
The AI Governance & Policy Engine is a *service the platform exposes*, not the platform's center of gravity. Any market team building an AI-adjacent feature calls this service to get a risk tier and required review path (see Section 6) — same integration pattern discipline as the data/comms/CMS layer, not a separate initiative bolted on top.

```
[Market Team] --sync (read)--> [Platform API — versioned contract]
[Market Team] --async (write)--> [Event Bus] --fan out--> [CDP | Comms | CMS]
                                                    |
                              [Consent Store — sync write, single source of truth]
                                                    |
                    [AI Governance & Policy Engine — called on demand, not sitting inline]
```

## 6. AI Governance (demoted from thesis to platform capability, per v1 critique)

Since SR 26-2 leaves gen-AI/agentic AI outside formal MRM scope, the platform offers a lightweight internal tiering model as one of its services — not its reason for existing:

- **Tier 1** (low-risk — e.g. content copy variants): automated policy check, no human review.
- **Tier 2** (medium-risk — e.g. next-best-action messaging using customer data): platform policy enforcement + logged human spot-check.
- **Tier 3** (high-risk — e.g. anything touching credit/account decisions): full human-in-the-loop, routed into existing model-risk processes rather than replacing them.

## 7. Success Metrics (rebuilt in v2 — genuine platform metrics)

| Metric | What it actually measures |
|---|---|
| Reuse rate | % of a new market's data/comms/CMS integration needs met via the platform vs. custom-built |
| Time-to-integrate a new market | Elapsed time from market kickoff to first live platform-based send/data sync |
| API SLA adherence | Uptime/latency of the platform's own API contract, independent of underlying vendor performance |
| Cost-per-market-onboarded | Marginal cost of adding a market vs. the first market's build cost — should trend down |
| Governance review cycle time (Tier 1/2/3) | Time from feature submission to governance decision, by tier |
| Consent propagation latency | Time from opt-out event to enforcement across all channels (governance sub-metric, not a headline platform metric) |

## 8. Delivery Cadence (new in v2 — addresses "light project-management oversight")

| Phase | Milestone | Decision gate |
|---|---|---|
| Q1 | Platform API contract + consent store design finalized with Eng/Tech Lead | Architecture review sign-off |
| Q2 | Governance tiering model reviewed with Risk/Compliance; Tier 1 automated checks live | Risk & Compliance sign-off |
| Q3 | First market (assume Germany, per public timeline) onboarded on platform for data/comms | Market GM + Legal sign-off on local compliance |
| Q4 | Tier 2/3 governance live; reuse-rate and cost-per-market metrics reported against baseline | Platform PM roadmap review with CCB/ICB leadership |

## 9. Vendor Management Approach

- Platform team, not individual market teams, owns commercial and technical relationships with CDP/comms/CMS vendors — market teams consume the platform's internal API, never integrate with a vendor directly.
- Vendor selection criteria (assumption, not Chase-specific): SOC 2 Type II / ISO 27001 minimum, demonstrated bank-sector references, contractual data-residency guarantees per market.
- Vendor swap is a platform-team-owned migration behind the versioned API (Section 5.3) — designed to be invisible to market teams when it happens.

## 10. Requirements (phased, ties to Section 8)

**Phase 1**: Platform API contract, consent store, event bus foundation.
**Phase 2**: Governance tiering (Tier 1 first), Risk/Compliance sign-off.
**Phase 3**: First market onboarding (Germany, per public timeline).
**Phase 4**: Full Tier 2/3 governance, reuse/cost metrics reported.

## 11. Risks & Regulatory Considerations

- SR 26-2's exclusion of gen-AI/agentic AI is recent and could be revised — governance model should be reviewed on a fixed cadence (proposed: semi-annually), not treated as a permanent design constraint. This is why Section 1 no longer treats it as the platform's primary justification.
- CFPB Section 1033 (open banking) is currently enjoined/under reconsideration — the platform's data layer should be built flexible to future open-banking API requirements without assuming the rule's final form.
- Cross-market privacy law variance (GLBA + US state law; UK/Germany data protection law) — no single compliance model should be assumed to work everywhere; Legal stakeholder sign-off (Section 2) is a hard gate per market, not a formality.

## 12. Open Questions

- What CDP/CMS/comms vendors does Chase already run for consumer digital channels? (Not public — would shape Section 5/9 significantly.)
- Does JPMC's AI Research program already have a gen-AI-specific governance framework not yet public? (Assume possible — this platform's governance layer would need to align with it, not duplicate it.)
- What's the actual Germany launch timeline and current platform readiness? (Public reporting only confirms "targeted 2026" — Section 8's Q3 milestone is illustrative, not a real commitment.)

## Changes from v1 → v2 (tied to reviewer critique)

1. **Rebalanced thesis**: multi-market reuse problem is now primary; SR 26-2/AI governance demoted from "reason to exist" to "capability the platform offers" (Sections 1, 6).
2. **Architecture rebuilt** with real integration-pattern decisions: sync/async split, consent propagation as the deliberate exception, API versioning strategy, idempotency (Section 5).
3. **Added stakeholder/RACI map** (Section 2) and **phased delivery cadence with decision gates** (Section 8) — directly answers the JD's "light project-management oversight."
4. **Metrics rebuilt**: added reuse rate, API SLA adherence, cost-per-market-onboarded; demoted consent-rate to a governance sub-metric (Section 7).
5. **Hedges tightened in-line**, not just in the disclaimer — Problem Statement and architecture claims now match the confidence level of the research notes throughout, not just at the top of the document.
