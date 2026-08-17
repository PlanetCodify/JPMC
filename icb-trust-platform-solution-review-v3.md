# ICB Trust Platform

**Stage:** Solution Review
**Last Updated:** August 17, 2026 (v3, post existing-infrastructure research)
**Owner:** Product Team (PoC, Platform PM VP application)
**Status:** Draft
**Supersedes:** [icb-trust-platform-solution-review-v2.md](icb-trust-platform-solution-review-v2.md) — see [review synthesis](icb-trust-platform-review-synthesis.md) for the 7-agent review that shaped v2, and [01_research_notes.md](01_research_notes.md) Section 6 for the new findings behind this version.

**A note on how this was built:** this is a job-application PoC for JPMC's Platform Product Manager (VP) role, International Consumer Bank Digital team. It's grounded entirely in public sources, never in claimed knowledge of JPMC's actual systems or roadmap. v2 went through a 7-perspective review panel. v3 exists because a direct question ("how much of this is already built?") led to research that found real, existing JPMC infrastructure this PRD had wrongly assumed didn't exist. That finding reshapes the pitch, not just a footnote.

---

## What Changed Since v2

One finding changes the whole positioning: **JPMC already has a Model Risk Governance & Review (MRGR) function with a role explicitly scoped to International Consumer Banking**, found via a live job posting for a Frankfurt-based VP role. A firmwide Chief Data & Analytics Office (CDAO), led by Teresa Heitsenrether, also already sets data and AI governance strategy firmwide and owns LLM Suite as a platform product.

This means v2's framing, that ICB has no dedicated governance function, was wrong. But the MRGR/ICB posting's requirements are scoped to traditional quantitative models (financial math, probability theory, numerical methods), with no mention of generative AI, agentic AI, or LLMs anywhere. That matches SR 26-2's own carve-out exactly.

**The revised pitch: this platform isn't inventing governance from scratch. It's the automated triage layer that feeds MRGR's existing review authority, specifically for the gen-AI category MRGR's current scope doesn't appear to cover yet.** That's a narrower, more defensible claim than v2's, and it changes what Phase 1 should actually build: not a parallel governance function, but an extension point into one that already exists.

---

## Hypothesis

**If we** build an automated tiering layer for gen-AI/agentic features that routes the highest-risk cases into JPMC's existing MRGR review process, instead of building a new, separate governance function,
**then** ICB gets gen-AI governance without duplicating the model-risk expertise and regulatory relationships MRGR already has,
**because** MRGR's current public scope for ICB appears to cover traditional statistical models only, which is exactly the boundary SR 26-2 itself draws, and no public evidence suggests that boundary has moved to cover gen-AI yet.

**Supporting Evidence:**
- SR 26-2 (Federal Reserve, April 17, 2026) excludes generative and agentic AI from formal model-risk scope for banks over $30 billion in assets. [Primary source](https://www.federalreserve.gov/supervisionreg/srletters/SR2602.htm)
- A live MRGR job posting scoped to ICB lists only traditional-model skills (financial math, probability theory, advanced numerical methods) with no mention of gen-AI, agentic AI, or LLMs. [Job posting, medium confidence, single source](https://builtin.com/job/legal-entity-governance-international-consumer-banking-model-risk-governance-review-mrgr-vice-president-all-genders/8001311)
- The review panel's skeptic and executive perspectives (v2) both flagged that betting on Risk & Compliance's appetite for a brand-new governance model was the riskiest assumption in the platform. Positioning this as an extension of MRGR's existing authority, not a replacement or competitor to it, directly reduces that risk instead of just flagging it.

---

## Strategic Fit

**Why this? Why now?**

The "why now" hasn't changed: SR 26-2's gap is real and dated, and Germany's launch is publicly targeted for 2026. What's changed is the "why this." Building a new governance function next to an existing one (MRGR) that already has regulatory relationships, examiner trust, and model-risk expertise would be redundant at best and confusing at worst. The stronger pitch is narrower: automate the triage that decides what does and doesn't need MRGR's attention, and route the hard cases to the people already positioned to review them.

**Impact Sizing:**

**Step 1: Estimate Usage**
| Stage | Teams/Features | Notes |
|-------|-------|-----------------|
| AI-adjacent features that could route through the tiering layer in year one | Estimated 8-12 | Still sourced from peer-bank public roadmap language, not an ICB-specific signal. Unchanged from v2, flagged as directional only. |
| MRGR reviewers who'd receive Tier 3 escalations | Unknown | This is now answerable in principle, since the MRGR/ICB team is a real, named function, not a hypothetical stakeholder. Needs a real conversation, not a guess. |

**Step 2: Calculate Impact**
- *Risk impact:* reduces the chance a gen-AI feature ships with no review path at all, by giving MRGR a defined entry point for the cases that actually need their expertise, rather than leaving gen-AI ungoverned by default.
- *Efficiency impact:* if MRGR is already stretched covering traditional models, an automated Tier 1/2 layer that filters out low-risk gen-AI features before they reach MRGR could be a real efficiency gain for an existing team, not just a new capability.
- *Cost impact:* likely lower than v2's estimate, since this no longer requires standing up a new governance function's worth of process and headcount, only a technical triage layer plus an integration point into MRGR's existing workflow.

**Step 3: Confidence Assessment**
| Assumption | Confidence | Risk Level | De-risking Action |
|------------|------------|------------|-------------------|
| MRGR's ICB scope doesn't currently cover gen-AI/agentic AI | Medium, based on one job posting, not a policy document | Medium, this is the whole premise | Confirm directly with MRGR or CDAO before Phase 1, since this is now checkable against a real function, not a hypothetical |
| MRGR would want an automated triage layer feeding into them, rather than building their own | Low, still unvalidated | High | This replaces v2's "would Risk & Compliance want tiering at all" question with a sharper one: would MRGR specifically want this as an extension, not a competing system |
| The classifier's self-routing logic is legally sound for credit-adjacent features | Unconfirmed | High | Fair-lending (ECOA/Reg B) review required before any build, unchanged from v2 |

**Summary:**
- Scope: an automated tiering layer that extends MRGR's reach into gen-AI, not a new governance function
- Strategic value: higher than v2's version if the core assumption holds, because it's additive to something that already has institutional trust, rather than asking the bank to trust something new

**Alternatives Considered:**
- Build a new, standalone governance function (v2's plan) - not doing this anymore as the default, because it risks duplicating MRGR's existing regulatory relationships and expertise instead of using them.
- Ask MRGR to build gen-AI coverage into their own team without a separate technical platform - a real alternative. Not assumed away here, but noted: MRGR's posting emphasizes traditional quantitative skills, which suggests they may need a technical layer built for them rather than building one themselves. This needs a real conversation, not an assumption either way.
- Germany forks the UK's existing setup, no new platform at all - still the cheapest fallback, unchanged from v2's reasoning.
- Wait for a federal AI governance standard - still not doing this, unchanged from v2.

---

## Non-Goals

What this platform is explicitly NOT doing in v1:
- **Replacing or competing with MRGR.** This platform's governance layer is designed to feed into MRGR's existing authority, not stand up a parallel one.
- **Assuming MRGR wants this without asking.** The confidence table above is honest that this is unvalidated, not just a formality to check off.
- **Building the CDP/CMS/comms integration in Phase 1.** Still deferred to Phase 2+, per v2's reasoning.
- **Naming a specific vendor** for any future data-platform work.
- **Committing to a Germany launch date.** Public reporting only confirms "targeted 2026."
- **Deciding UK-first vs. Germany-first sequencing inside this document.** Still explicitly unresolved, not buried.

---

## Integrator Experience

Unchanged from v2: the Phase 1 tiering layer needs real docs, a sandbox, an error taxonomy, and self-serve status, because speed alone doesn't stop workaround behavior if the experience is unclear.

## Governance Reviewer Experience (revised)

This is no longer a generic "Risk & Compliance" reviewer experience. It's specifically an MRGR-facing surface: a queue for Tier 3 escalations that lands in MRGR's existing review process, with the classifier's reasoning and audit trail attached. Whatever this looks like should be designed with MRGR directly, not assumed from outside, since they're a real, findable team now, not a hypothetical stakeholder.

---

## Success Metrics

Unchanged from v2's structure, with one addition:

**Primary Metric:** Time from a market team's first tiering-layer integration attempt to a working classification call. Target: under 1 business day.

**Co-Primary Metric:** 90-day sustained use rate, tracked from day one.

**New Guardrail Metric:** Percentage of Tier 3 escalations MRGR actually accepts and reviews through the new routing, versus rejects or routes around. This directly tests whether the "extends MRGR" positioning is real or just a pitch.

**Kill Criteria:** If MRGR declines to accept tiered escalations through this layer after Phase 1, or if 90-day reuse is near zero, we stop. Unchanged in spirit from v2, sharpened to name MRGR specifically instead of a generic Risk & Compliance function.

---

## Rollout Plan

**Phase 1 (Q1-Q2):** Automated tiering layer, Tier 1 checks, Integrator Experience basics, validated directly with MRGR, not a generic Risk stakeholder.
- Passing criteria: time-to-first-integration under 1 day, and a real answer from MRGR on whether they'd accept escalations through this layer

**Phase 2 (Q3, conditional):** Tier 2/3 routing into MRGR's actual review workflow, CDP/CMS decision made based on Phase 1 results, UK-vs-Germany sequencing forced to a decision.

**Phase 3 (Q4, conditional):** Full platform build if justified, customer-trust dashboard, consent UX design.

**Rollback Plan:** Unchanged from v2.

---

## Interim Coverage Statement

Unchanged from v2: during Phase 1-2, anything that might plausibly need Tier 2 or Tier 3 review routes to full manual review by default, not through the unfinished tiering system.

---

## AI Behavior Contract (for the Governance & Policy Engine)

Unchanged from v2's specification, with one addition to the Constraints row: Tier 3 escalations route specifically into MRGR's existing review process, not a generic or newly invented one.

| Dimension | Specification |
|-----------|--------------|
| **Primary Task** | Classify an AI-adjacent feature request into Tier 1, 2, or 3, and route Tier 3 into MRGR's existing review workflow |
| **Inputs Available** | Feature description, data sources touched, customer-facing surface, whether it influences a credit or account decision |
| **Constraints** | Must never auto-approve anything touching a credit or account decision. Tier 3 routes to MRGR specifically, not a parallel process. Self-routing logic requires fair-lending review before this ships. |
| **Disallowed** | Silent approval of anything uncertain (defaults to Tier 2). Operating with no fail-safe if unavailable (also defaults to Tier 2). |
| **Latency Budget** | P50: under 2 seconds / P95: under 10 seconds |

Behavior examples unchanged from v2.

---

## Risks and Recovery

| Risk | Detection | Fallback | Kill Switch |
|------|-----------|----------|-------------|
| MRGR doesn't want an automated layer feeding into them | Direct conversation with MRGR in Phase 1 | Fall back to v2's plan: a standalone lightweight tiering tool, accepting the duplication risk this version was designed to avoid | Platform PM |
| MRGR's actual scope already covers gen-AI, just not reflected in this one job posting | Direct conversation with MRGR or CDAO | If true, this platform's job shrinks to a technical integration layer into an existing process, which is a smaller, cheaper build than assumed | Platform PM |
| Time-to-first-integration looks good but 90-day reuse doesn't follow | 90-day check-in | Pause Phase 2, diagnose the actual cause | Platform PM |
| A Tier 3 feature ships without a complete audit trail | Governance dashboard shows incomplete record | Immediate rollback, no exceptions | MRGR |

---

## Open Questions

- [ ] Does MRGR's actual scope for ICB already cover gen-AI/agentic AI, contrary to what one job posting suggests? This is now the single most important question in the document, and it's a checkable fact, not a guess, if you have access to ask. - @Risk & Compliance liaison
- [ ] Would MRGR want an automated triage layer feeding into their existing process, or would they see it as unnecessary or intrusive? - @Risk & Compliance liaison
- [ ] Does JPMC's engineering culture mean the general 64%-bypass adoption benchmark doesn't transfer here? Unchanged from v2. - @Engineering Lead
- [ ] Does German consumer attitude toward AI and data privacy differ from the UK/US research this PRD leans on? Unchanged from v2. - @Platform PM
- [ ] UK-first or Germany-first sequencing? Unchanged from v2, still unresolved on purpose. - @Platform PM
- [ ] Is CDAO's existing data-product discipline (define, develop, register, govern reusable data products) something this platform should plug into directly, rather than being a separate initiative? New question, prompted by the CDAO finding. - @Platform PM

---

## Appendix

Full research trail: [user-research-synthesis-icb-trust-platform-v2.md](user-research-synthesis-icb-trust-platform-v2.md), [competitor-analysis-icb-trust-platform-v2.md](competitor-analysis-icb-trust-platform-v2.md), [icb-trust-platform-review-synthesis.md](icb-trust-platform-review-synthesis.md), [01_research_notes.md](01_research_notes.md) (see Section 6 for the MRGR/CDAO findings behind this version), [market-research-icb-trust-platform-2026-08-17.md](market-research-icb-trust-platform-2026-08-17.md).
