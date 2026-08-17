# ICB Trust Platform

**Stage:** Solution Review
**Last Updated:** August 17, 2026 (v2, post 7-agent review)
**Owner:** Product Team (PoC, Platform PM VP application)
**Status:** Draft
**Supersedes:** [icb-trust-platform-solution-review.md](icb-trust-platform-solution-review.md) — see [review synthesis](icb-trust-platform-review-synthesis.md) for what changed and why.

**A note on how this was built:** this is a job-application PoC for JPMC's Platform Product Manager (VP) role, International Consumer Bank Digital team. It's grounded entirely in public sources, never in claimed knowledge of JPMC's actual systems or roadmap. This version went through a 7-perspective review panel (engineering, design, executive, legal, UXR, skeptic, customer voice), and the two judgment calls the reviewers surfaced as genuine forks, not just gaps, are made explicitly below, with reasoning, instead of left open.

---

## What Changed Since v1

Two real judgment calls, made:

1. **Phase 1 is now governance-only, not bundled with the CDP/CMS build.** The skeptic review made a strong case: if Risk & Compliance's appetite for standardized tiering is the riskiest, least-validated assumption in this whole PRD, betting a full data-platform build on it before testing it is backwards. Phase 1 now ships the AI Governance & Policy Engine as a standalone callable service first. The CDP/CMS/comms integration becomes a Phase 2+ decision, made after Risk & Compliance's actual appetite is known.
2. **Reuse rate is now tracked from day one, not just a Phase 3 target.** Time-to-first-integration stays as the Phase 1 gate (it's still a useful leading indicator), but a 90-day reuse check-in is now a required gate before any Phase 2 spend, specifically so the platform can't sail past its own kill criteria by being fast but unused.

Everything else below reflects direct fixes from the review: an Integrator Experience section, a Governance Reviewer Experience section, explicit compliance gates, and Open Questions that were previously buried in the research appendix.

---

## Hypothesis

**If we** ship a standalone AI Governance & Policy Engine first, validated directly with Risk & Compliance, before committing to a shared customer data and communications platform,
**then** we'll know within one phase whether the governance value proposition actually holds, instead of discovering it doesn't after a much bigger CDP/CMS investment,
**because** the review panel's skeptic and executive perspectives both independently flagged Risk & Compliance's appetite for standardized tiering as the single highest-leverage unvalidated assumption in the platform's whole thesis.

**Supporting Evidence:**
- SR 26-2 (Federal Reserve, April 17, 2026) excludes generative and agentic AI from formal model-risk scope for banks over $30 billion in assets. [Primary source](https://www.federalreserve.gov/supervisionreg/srletters/SR2602.htm)
- The user research synthesis itself calls the Risk & Compliance "would want this" claim its least-grounded finding, not a lightly caveated one. [User research synthesis, Theme 2]
- The skeptic review adds a sharper version of this concern: standardized tiering creates an audit trail that assigns Risk blame when the classifier is wrong, and could shrink the manual-review function Risk currently owns. That's an incentive to resist, not just an unknown.

---

## Strategic Fit

**Why this? Why now?**

SR 26-2 closed a regulatory window in April 2026, and ICB's Germany launch is publicly targeted for the same year. But the review panel surfaced a sharper reason to sequence this way: the governance piece is the urgent, differentiated part of the platform. It doesn't need to ride on top of a full data-platform build to prove its value. Validating it standalone is both faster and cheaper than finding out it doesn't work after the bigger build is already underway.

**Impact Sizing:**

**Step 1: Estimate Usage**
| Stage | Teams/Features | Notes |
|-------|-------|-----------------|
| AI-adjacent features that could route through the governance engine in year one | Estimated 8-12 | Sourced from peer-bank public roadmap language (Citi's Arc, Standard Chartered's AI Factory), not any ICB-specific signal. The skeptic review is right to flag this as an outside-in guess; treat it as directional only. |
| Risk & Compliance reviewers who'd interact with the tiering model | Unknown, illustrative estimate 2-4 | Needs real headcount input before Phase 1 completes |

**Step 2: Calculate Impact**
- *Risk impact:* a validated governance model reduces the chance an AI feature ships without a defensible review trail. Unquantifiable in dollars without real incident-cost data, which doesn't exist for a platform that hasn't shipped.
- *Speed impact:* if Risk & Compliance adopts the tiering model, review cycle time should drop from an assumed multi-day ad hoc process to a matter of hours for Tier 1.
- *Cost impact:* deferred. The CDP/CMS cost case only gets built once Phase 1 confirms the governance layer is wanted.

**Step 3: Confidence Assessment**
| Assumption | Confidence | Risk Level | De-risking Action |
|------------|------------|------------|-------------------|
| Risk & Compliance wants a standardized tiering model | Low, and possibly lower than "low confidence" suggests, per the skeptic review's incentive argument | High, this is now the entire Phase 1 bet | Talk to an actual Risk stakeholder before Phase 1 completes, not after |
| The classifier's self-routing logic is legally sound for credit-adjacent features | Unconfirmed | High | Fair-lending (ECOA/Reg B) review required before any build, per the legal review |
| Time-to-first-integration predicts sustained platform use | Medium, and gameable on its own | Medium | Track 90-day reuse from day one, require a real check-in before Phase 2 |

**Summary:**
- Scope: one standalone governance service, not a full platform, in Phase 1
- Strategic value: high if Risk & Compliance validates the model, unproven otherwise, which is exactly why this is now the first thing tested

**Alternatives Considered:**
- Bundle the governance engine with a full CDP/CMS/comms platform build (the original v1 plan) - not doing this first anymore, because it bets the bigger investment on the least-validated assumption.
- Germany forks the UK's existing setup, no new platform at all - genuinely the cheapest option. Not recommended as the sole path, because it doesn't close the SR 26-2 governance gap, but it's a real fallback if Phase 1 shows Risk & Compliance doesn't want a standardized model. Previously omitted from this document; the skeptic review is right that it should have been here from the start.
- Wait for a federal AI governance standard - not doing this, since SR 26-2 explicitly declined to set one, and the gap doesn't close itself.

---

## Non-Goals

What this platform is explicitly NOT doing in v1:
- **Building the CDP/CMS/comms integration in Phase 1.** That's now a Phase 2+ decision, made after governance validation, not a Phase 1 commitment.
- **Naming a specific vendor** for any future data-platform work.
- **Committing to a Germany launch date.** Public reporting only confirms "targeted 2026."
- **Building Tier 3 model-validation logic from scratch.** The plan is to call into a specialized tool (ValidMind-class), pending confirmation of whether one already exists inside JPMC's stack.
- **Deciding UK-first vs. Germany-first sequencing inside this document.** That decision needs real stakeholder input this PoC can't provide, and the executive review is right that it shouldn't be silently deferred to Phase 2. It's called out here as unresolved, on purpose, not buried.

---

## Integrator Experience (new, per engineering and design review)

The Phase 1 governance service needs a real integration path, not just an API contract in name:
- **Docs:** a working quickstart showing how to call the classifier and interpret a tier result.
- **Sandbox:** a non-production environment where a market team can test classification without touching real customer data.
- **Error taxonomy:** clear, specific errors for malformed requests, timeout, and classifier unavailability (which fails to Tier 2, matching the ambiguous-case rule, not a silent failure).
- **Self-serve status:** a way for an engineer to check why a submitted feature is stuck in review, without filing a ticket.

This exists because the design review flagged that speed alone doesn't stop workaround behavior if the actual experience of using the platform is unclear or frustrating.

## Governance Reviewer Experience (new, per design review)

Tier 2 and Tier 3 both require a human reviewer to act on something. This needs:
- **A review queue:** where flagged features land, with the classifier's reasoning attached.
- **Audit trail visibility:** the reviewer can see the full record before approving or rejecting.
- **A stated SLA:** so a Tier 2 spot-check or Tier 3 review doesn't sit indefinitely.

This can't be fully designed until Phase 1 confirms Risk & Compliance wants this workflow at all. A stub version should exist by the end of Phase 1, not be discovered late.

---

## Success Metrics

**Primary Metric:** Time from a market team's first governance-engine integration attempt to a working classification call
- Current: unmeasured
- Target: under 1 business day
- Timeline: measured from Phase 1 start

**Co-Primary Metric (new, per skeptic and design review):** 90-day sustained use rate
- Current: unmeasured
- Target: tracked from day one, required check-in before any Phase 2 approval, no fixed target yet since this is the first real data point on whether the platform gets used or just handshaken with
- Why this exists: time-to-first-integration alone is gameable by a shallow test call. This metric exists specifically so the platform can't pass its own kill criteria by being fast but unused.

**Guardrail Metrics:**
- Governance review cycle time (Tier 1): under 8 hours
- Tier 3 audit trail completeness: 100%, no exceptions

**Kill Criteria:**
If Risk & Compliance explicitly declines to adopt the tiering model after Phase 1, or if 90-day reuse is near zero despite a fast first integration, we stop here. We don't proceed to a CDP/CMS platform build to solve a governance problem that turned out not to want solving this way.

---

## Rollout Plan

**Approach:** Phased, governance-first, CDP/CMS deferred

**Phase 1 (Q1-Q2):** Standalone AI Governance & Policy Engine, Tier 1 automated checks, Integrator Experience basics, validated directly with a real Risk & Compliance stakeholder.
- Passing criteria: time-to-first-integration under 1 day, and a real answer (not an assumption) on whether Risk & Compliance wants this model

**Phase 2 (Q3, conditional on Phase 1 passing):** Tier 2/3 governance, Governance Reviewer Experience stub, and the CDP/CMS/comms integration decision gets made, informed by whether the governance layer proved itself. UK-vs-Germany sequencing gets forced to a real decision here, not silently deferred.
- Passing criteria: 90-day reuse data exists and is reviewed, Legal sign-off on cross-border data handling if the CDP/CMS work proceeds

**Phase 3 (Q4, conditional on Phase 2):** Full platform build, if justified by Phase 1-2 data. Customer-trust dashboard and consent UX design (separate from consent-store architecture, per customer voice review) ship here.

**Rollback Plan:**
If a Tier 3 feature ships without a complete audit trail, it gets pulled immediately, no exceptions. If Phase 1's kill criteria triggers, no Phase 2 spend gets approved.

---

## Interim Coverage Statement (new, per legal and customer voice review)

During Phase 1-2, only Tier 1 automated checks are live. Any feature that might plausibly be Tier 2 or Tier 3 during this window routes to full manual review by default, not through the unfinished tiering system. This isn't a gap the platform quietly accepts, it's an explicit fallback stated here so "the safety tier wasn't built yet" is never the actual answer if something goes wrong.

---

## AI Behavior Contract (for the Governance & Policy Engine)

| Dimension | Specification |
|-----------|--------------|
| **Primary Task** | Classify an AI-adjacent feature request into Tier 1, 2, or 3, and return the required review path |
| **Inputs Available** | Feature description, data sources touched, customer-facing surface, whether it influences a credit or account decision |
| **Constraints** | Must never auto-approve anything touching a credit or account decision (always Tier 3). Self-routing logic for credit-adjacent features requires fair-lending review before this ships, per the legal review. |
| **Disallowed** | Silent approval of anything the classifier is uncertain about (defaults to Tier 2, never defaults to Tier 1). Operating with no fail-safe if the classifier itself is unavailable (also defaults to Tier 2). |
| **Latency Budget** | P50: under 2 seconds / P95: under 10 seconds |

**Behavior Examples:**

| Scenario | Feature Description | Expected Output | Category |
|----------|------------|-----------------|-------------------|
| Happy path, low risk | Marketing copy A/B test variants | Tier 1, automated approval | Good |
| Happy path, medium risk | Next-best-action messaging using purchase history | Tier 2, policy check plus logged spot-check | Good |
| Happy path, high risk | AI-drafted credit decision narrative | Tier 3, full human-in-the-loop | Good |
| Edge case, ambiguous | Feature description doesn't clearly match a known pattern | Defaults to Tier 2, never silently ships | Bad, handled gracefully |
| Edge case, classifier down | Classifier service unavailable or times out | Defaults to Tier 2, request logged, no silent failure | Bad, handled gracefully |
| Should reject | Feature that would auto-approve a credit line increase with no human check | Blocked from routing through Tier 1 or 2 under any circumstance | Reject |

---

## Audit Trail Data Model (new, per engineering review)

Before Phase 1 exit, define what makes a Tier 3 record "complete": the classifier's reasoning, the reviewer's identity and decision, timestamp, and the specific data inputs the classification was based on. The rollback plan depends on this being machine-checkable, not just described in prose.

---

## Risks and Recovery

| Risk | Detection | Fallback | Kill Switch |
|------|-----------|----------|-------------|
| Risk & Compliance doesn't want a standardized tiering model | Direct stakeholder feedback during Phase 1 | Fall back to a lightweight decision-logging tool with no forced tiering, re-propose the full model later if trust builds | Platform PM |
| Time-to-first-integration looks good but 90-day reuse doesn't follow | 90-day check-in shows low sustained use | Pause Phase 2, diagnose whether the integration experience or the governance value prop is the actual problem | Platform PM |
| Classifier self-routing logic doesn't pass fair-lending review | Legal review before build | Redesign so credit-adjacent classification always requires human review, no automated path at all for that category | Legal |
| A Tier 3 feature ships without a complete audit trail | Governance dashboard shows an incomplete record | Immediate rollback, no exceptions | Risk & Compliance |

---

## Open Questions

- [ ] Would actual Risk & Compliance stakeholders want this tiering model, or prefer case-by-case discretion? Now the single highest-priority question in this document, flagged independently by three review perspectives. - @Risk & Compliance liaison
- [ ] Does JPMC's engineering culture (MACH Alliance membership, existing LLM Suite platform) mean the 64%-bypass benchmark from general platform research doesn't transfer here? Previously buried in the research appendix; promoted here per the UXR review. - @Engineering Lead
- [ ] Does German consumer attitude toward AI and data privacy differ from the UK/US research this PRD leans on, given GDPR-native expectations? Also previously buried in the appendix. - @Platform PM
- [ ] UK-first or Germany-first sequencing? NatWest's Cora and Lloyds' Envoy are already live in the UK market. Explicitly unresolved here, not silently deferred. - @Platform PM, needs real stakeholder decision
- [ ] Does the existing model-risk process Tier 3 would route into actually have scope for gen-AI outputs feeding credit decisions? - @Legal
- [ ] Is there a ValidMind-class tool already inside JPMC's model-risk stack that Tier 3 should call into? - @Engineering Lead

---

## Appendix

Full research trail: [user-research-synthesis-icb-trust-platform-v2.md](user-research-synthesis-icb-trust-platform-v2.md), [competitor-analysis-icb-trust-platform-v2.md](competitor-analysis-icb-trust-platform-v2.md), [icb-trust-platform-review-synthesis.md](icb-trust-platform-review-synthesis.md), [01_research_notes.md](01_research_notes.md), [market-research-icb-trust-platform-2026-08-17.md](market-research-icb-trust-platform-2026-08-17.md).
