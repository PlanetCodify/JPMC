# ICB Trust Platform

**Stage:** Solution Review
**Last Updated:** August 17, 2026
**Owner:** Product Team (PoC, Platform PM VP application)
**Status:** Final

**A note on how this was built:** this is a job-application PoC for JPMC's Platform Product Manager (VP) role, International Consumer Bank Digital team. It's grounded entirely in public sources, cited throughout, never in claimed knowledge of JPMC's actual systems or roadmap. It went through real research, a synthesized user study, a competitive teardown, and a 7-perspective review panel before landing here. Full trail in the Appendix.

---

## Hypothesis

**If we** build an automated tiering layer for gen-AI and agentic features, one that routes the highest-risk cases into JPMC's existing Model Risk Governance & Review (MRGR) process instead of standing up a new governance function next to it,
**then** International Consumer Bank gets real gen-AI oversight without duplicating the model-risk expertise and regulatory relationships MRGR already has,
**because** MRGR's current public scope for ICB appears to cover traditional statistical models only, the same boundary the Fed's SR 26-2 guidance itself draws, and nothing public suggests that boundary has moved to cover generative or agentic AI yet.

**Supporting Evidence:**
- SR 26-2 (Federal Reserve, April 17, 2026) excludes generative and agentic AI from formal model-risk scope for banks over $30 billion in assets. [Primary source](https://www.federalreserve.gov/supervisionreg/srletters/SR2602.htm)
- A live MRGR job posting scoped to ICB lists only traditional-model skills (financial math, probability theory, advanced numerical methods), with no mention of gen-AI, agentic AI, or LLMs. [Job posting](https://builtin.com/job/legal-entity-governance-international-consumer-banking-model-risk-governance-review-mrgr-vice-president-all-genders/8001311), medium confidence, one source.
- ICB is scaling a branchless, digital-only retail model across markets (UK live since 2021, Germany publicly targeted for 2026), which means more gen-AI-adjacent features are coming, not fewer, right as this gap sits open.

---

## Who This Serves

| Stakeholder | Need |
|---|---|
| MRGR (Model Risk Governance & Review) | An entry point for gen-AI features that fits their existing review authority, instead of a parallel process competing for the same ground |
| Market launch team (engineers) | A fast, self-serve way to know what review path a new feature needs, without guessing or waiting on an ad hoc judgment call |
| ICB customer | Consistent treatment: no AI-driven decision about their account or credit ships without the review it actually needs |

---

## Strategic Fit

**Why this? Why now?**

SR 26-2 didn't just leave a gap, it left a gap at the exact moment ICB is scaling into a second market. Every new gen-AI-adjacent feature since April 2026 has had nowhere clear to go for review. The stronger move isn't building a new governance team to fill that gap. It's building the technical layer that routes the hard cases to the team that already has the standing, the expertise, and the regulatory relationships to handle them: MRGR.

**Impact Sizing**

**Step 1: Estimate usage**
| Stage | Estimate | Notes |
|---|---|---|
| Gen-AI-adjacent features expected per year across ICB | 8-12 | Sourced from peer-bank public roadmap language (Citi's Arc, Standard Chartered's AI Factory), not an ICB-specific number. Directional only. |
| MRGR reviewers who'd receive Tier 3 escalations | Unknown | Now answerable, since MRGR is a real, named function. Needs a direct conversation before Phase 1 closes. |

**Step 2: Calculate impact**
- *Risk:* closes the gap between "SR 26-2 doesn't require formal gen-AI review" and "gen-AI features ship with no review at all." Right now that gap is open by default.
- *Efficiency:* if MRGR is already stretched on traditional models, an automated Tier 1/2 filter that only escalates genuinely high-risk cases is a real efficiency gain for a team that already exists, not a new cost center.
- *Cost:* lower than building a parallel governance function from scratch, since this is a technical integration layer plus a routing agreement, not a new team's worth of process.

**Step 3: Confidence assessment**
| Assumption | Confidence | Risk | De-risking action |
|---|---|---|---|
| MRGR's ICB scope doesn't currently cover gen-AI | Medium (one job posting, not a policy document) | Medium | Confirm directly with MRGR or CDAO before Phase 1 starts |
| MRGR would accept an automated triage layer feeding into them | Low, unvalidated | High, this is the entire bet | Talk to MRGR directly, don't build past Phase 1 on an assumption |
| The classifier's self-routing logic is legally sound for credit-adjacent features | Unconfirmed | High | Fair-lending (ECOA/Reg B) review required before any build |

**Alternatives Considered:**
- Build a new, standalone governance function - rejected as the default, because it risks duplicating MRGR's existing regulatory relationships instead of using them.
- Ask MRGR to build gen-AI coverage into their own team, no separate technical layer - a real option. MRGR's posting emphasizes traditional quantitative skills, which suggests they may need a technical layer built for them, but this needs a real conversation, not an assumption either way.
- Germany forks the UK's existing setup, no new platform at all - the cheapest fallback. Doesn't close the SR 26-2 gap, but stays on the table if MRGR declines the automated-routing approach.
- Wait for a federal AI governance standard - rejected, since SR 26-2 explicitly declined to set one and the gap doesn't close on its own.

---

## Non-Goals

What this platform is explicitly NOT doing:
- **Replacing or competing with MRGR.** The tiering layer feeds into their existing authority, it doesn't stand up a parallel one.
- **Assuming MRGR wants this without asking.** The confidence table above treats this as a real open question, not a formality.
- **Building the customer data, communications, and CMS integration in Phase 1.** That's a Phase 2+ decision, made only after the governance layer proves itself.
- **Naming a specific vendor** for any future data-platform work.
- **Committing to a Germany launch date.** Public reporting only confirms "targeted 2026."
- **Deciding UK-first vs. Germany-first sequencing.** Left explicitly open below, not silently resolved.

---

## Solution Overview

**Integrator Experience:** market teams need a real integration path for the tiering layer, not just an API in name. That means working docs, a sandbox to test classification without touching real customer data, a clear error taxonomy (including what happens if the classifier itself is down), and a self-serve way to check why a submitted feature is stuck in review.

**MRGR-Facing Experience:** Tier 3 escalations need a queue that lands in MRGR's existing review process, with the classifier's reasoning and full audit trail attached. This should be designed with MRGR directly, not assumed from outside.

**Interim Coverage:** while only Tier 1 is live (Phase 1-2), anything that might plausibly need Tier 2 or Tier 3 review routes to full manual review by default. This isn't a quiet gap. It's a stated fallback, so "the safety tier wasn't built yet" is never the real answer if something goes wrong.

---

## AI Behavior Contract

| Dimension | Specification |
|---|---|
| Primary Task | Classify a gen-AI or agentic feature request into Tier 1, 2, or 3, and route Tier 3 into MRGR's existing review workflow |
| Inputs Available | Feature description, data sources touched, customer-facing surface, whether it influences a credit or account decision |
| Constraints | Never auto-approves anything touching a credit or account decision. Tier 3 always routes to MRGR, not a parallel process. Self-routing logic requires fair-lending review before this ships. |
| Disallowed | Silent approval of anything uncertain (defaults to Tier 2, never Tier 1). No fail-safe if the classifier is unavailable (also defaults to Tier 2, logged, never a silent failure). |
| Latency Budget | P50 under 2 seconds, P95 under 10 seconds |

| Scenario | Feature | Expected Output | Category |
|---|---|---|---|
| Low risk | Marketing copy A/B variants | Tier 1, automated approval | Good |
| Medium risk | Next-best-action messaging using purchase history | Tier 2, policy check plus logged spot-check | Good |
| High risk | AI-drafted credit decision narrative | Tier 3, routed to MRGR, full human review | Good |
| Ambiguous | Doesn't clearly match a known pattern | Defaults to Tier 2, never silently ships | Handled gracefully |
| Classifier down | Service unavailable or times out | Defaults to Tier 2, logged, no silent failure | Handled gracefully |
| Should reject | Would auto-approve a credit line increase with no human check | Blocked from Tier 1 or 2 under any circumstance | Reject |

---

## Success Metrics

**Primary Metric:** time from a market team's first integration attempt to a working classification call
- Target: under 1 business day

**Co-Primary Metric:** 90-day sustained use rate, tracked from day one
- Why it exists: time-to-first-integration alone is gameable with a shallow test call. This metric exists so the platform can't pass its own bar by being fast but unused.

**Guardrail Metrics:**
- Governance review cycle time, Tier 1: under 8 hours
- Tier 3 audit trail completeness: 100%, no exceptions
- Percentage of Tier 3 escalations MRGR actually accepts and reviews through this routing, versus rejects or works around: this is the metric that tests whether the "extends MRGR" pitch is real

**Kill Criteria:**
If MRGR declines to accept escalations through this layer after Phase 1, or if 90-day reuse is near zero despite a fast first integration, this stops here. We don't build a bigger data platform to solve a governance problem that turned out not to want solving this way.

---

## Rollout Plan

**Phase 1 (Q1-Q2):** Automated tiering layer, Tier 1 checks, integrator basics, validated directly with MRGR.
- Passing criteria: time-to-first-integration under 1 day, and a real answer from MRGR on whether they'd accept escalations through this layer

**Phase 2 (Q3, conditional on Phase 1):** Tier 2/3 routing into MRGR's actual workflow. The customer data, communications, and CMS integration decision gets made here, informed by whether the governance layer proved itself. UK-vs-Germany sequencing gets forced to a real decision, not silently deferred.
- Passing criteria: 90-day reuse data reviewed, Legal sign-off on cross-border data handling if the platform build proceeds

**Phase 3 (Q4, conditional on Phase 2):** Full platform build if justified by Phase 1-2 data. Customer-trust dashboard and a dedicated consent UX design, separate from the technical consent-store architecture.

**Rollback Plan:** any Tier 3 feature that ships without a complete audit trail gets pulled immediately, no exceptions. If Phase 1's kill criteria triggers, no Phase 2 spend gets approved.

---

## Risks and Recovery

| Risk | Detection | Fallback | Owner |
|---|---|---|---|
| MRGR doesn't want an automated layer feeding into them | Direct conversation in Phase 1 | Fall back to a standalone lightweight tiering tool, accepting the duplication this design was meant to avoid | Platform PM |
| MRGR's scope already covers gen-AI, just not reflected in the one job posting this PRD relies on | Direct conversation with MRGR or CDAO | The build shrinks to a technical integration into an existing process, smaller and cheaper than assumed | Platform PM |
| Fast integration but low 90-day reuse | 90-day check-in | Pause Phase 2, diagnose whether it's the integration experience or the governance value prop | Platform PM |
| Classifier's self-routing logic fails fair-lending review | Legal review before build | Redesign so credit-adjacent classification always requires human review, no automated path for that category | Legal |
| A Tier 3 feature ships without a complete audit trail | Governance dashboard shows an incomplete record | Immediate rollback, no exceptions | MRGR |

---

## Open Questions

- [ ] Does MRGR's actual scope for ICB already cover gen-AI/agentic AI? This is checkable, not a guess, for anyone with access to ask. - @Risk & Compliance liaison
- [ ] Would MRGR want an automated triage layer feeding into their process, or see it as unnecessary? - @Risk & Compliance liaison
- [ ] Does the existing model-risk process actually have scope to cover gen-AI outputs feeding credit decisions, once Tier 3 routes there? - @Legal
- [ ] Does JPMC's engineering culture (MACH Alliance membership, existing LLM Suite platform) mean general platform-adoption research doesn't fully transfer here? - @Engineering Lead
- [ ] Does German consumer attitude toward AI and data privacy differ from the UK/US research this PRD leans on, given GDPR-native expectations? - @Platform PM
- [ ] UK-first or Germany-first sequencing? NatWest's Cora and Lloyds' Envoy are already live in the UK market. - @Platform PM, needs real stakeholder input
- [ ] Should this plug into CDAO's existing firmwide data-product discipline, rather than standing alone? - @Platform PM

---

## Appendix

Research and process trail:
- [01_research_notes.md](01_research_notes.md) — every factual claim, sourced (Section 6 has the MRGR/CDAO findings this version is built on)
- [market-research-icb-trust-platform-2026-08-17.md](market-research-icb-trust-platform-2026-08-17.md) — CDP, martech, and AI-governance market context
- [user-research-synthesis-icb-trust-platform-v2.md](user-research-synthesis-icb-trust-platform-v2.md) — synthesized (not primary) research on the three personas
- [competitor-analysis-icb-trust-platform-v2.md](competitor-analysis-icb-trust-platform-v2.md) — build-vs-buy teardown and peer-bank benchmarks
- [icb-trust-platform-review-synthesis.md](icb-trust-platform-review-synthesis.md) — the 7-perspective review panel that shaped this document
- [icb-trust-platform-solution-review-v3.md](icb-trust-platform-solution-review-v3.md) — the version this one consolidates and finalizes
