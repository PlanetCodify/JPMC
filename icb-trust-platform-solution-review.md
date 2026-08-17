# ICB Trust Platform

**Stage:** Solution Review
**Last Updated:** August 17, 2026
**Owner:** Product Team (PoC, Platform PM VP application)
**Status:** Draft

**A note on how this was built:** this is a job-application PoC for JPMC's Platform Product Manager (VP) role, International Consumer Bank Digital team. It's grounded entirely in public sources (Federal Reserve guidance, published market and consumer research), never in claimed knowledge of JPMC's actual systems or roadmap. Where this PRD states something as fact, you can check it. Where it assumes something, it says so.

---

## Hypothesis

**If we** build a shared platform for customer data, communications, and content, with AI governance offered as a callable service inside it,
**then** new ICB markets will onboard faster and AI-adjacent features will ship with a consistent, defensible review process,
**because** the alternative today is that each market rebuilds this infrastructure from scratch, and every AI feature gets a one-off governance judgment call since the Fed's SR 26-2 guidance left generative and agentic AI outside formal model-risk rules.

**Supporting Evidence:**
- SR 26-2 (Federal Reserve, April 17, 2026) explicitly excludes generative and agentic AI from formal model-risk scope for banks over $30 billion in assets. [Primary source](https://www.federalreserve.gov/supervisionreg/srletters/SR2602.htm)
- 64% of engineers bypass internal platforms that are slower than the workaround, which is the single biggest threat to this platform actually getting used. [User research synthesis, Theme 1]
- 39% of consumers think personalization is worth the privacy trade-off, even though 64% say they want it. That gap is exactly what the platform's consent design and Tier 3 human-in-the-loop requirement are built to respect. [User research synthesis, Theme 3]

---

## Strategic Fit

**Why this? Why now?**

ICB is scaling a branchless, digital-only retail bank across markets. The UK's been live since 2021, and Germany is publicly targeted for 2026. Two things make this the right moment: Germany's launch means a second real market is about to test whatever infrastructure exists today, and SR 26-2 just closed a regulatory window that used to leave banks with more ambiguity, not less, about how to govern gen-AI features.

**Impact Sizing:**

**Step 1: Estimate Usage**
| Stage | Teams/Features | Notes |
|-------|-------|-----------------|
| ICB markets that could use this platform | 2 (UK, Germany), more if the model expands | UK's already live, so this is really about Germany plus whatever comes after |
| AI-adjacent features expected per year, platform-wide | Estimated 8-12 | Based on the pace of feature requests implied by peer-bank activity (Citi's Arc, Standard Chartered's AI Factory both cite multi-feature roadmaps) |
| Engineers who'd touch the platform in year one | Estimated 3-4 FTEs on the Germany launch team | Illustrative, not confirmed staffing |

**Step 2: Calculate Impact**
- *Engagement impact:* not directly measurable pre-launch. The proxy is time-to-first-integration, which should predict whether teams actually use the platform instead of routing around it.
- *Cost impact:* cost-per-market-onboarded should trend down as the platform matures, compared to the do-nothing baseline of rebuilding per market.
- *Risk impact:* a standardized AI governance tier reduces the chance that an ungoverned feature becomes an incident, which is hard to quantify in dollars but easy to quantify in reputational cost if it goes wrong.

**Step 3: Confidence Assessment**
| Assumption | Confidence | Risk Level | De-risking Action |
|------------|------------|------------|-------------------|
| Engineers will actually route through the platform if it's fast enough | Medium | High if wrong, since the whole platform depends on it | Ship the integration path first, measure time-to-first-sync, treat it as a hard gate before building more governance |
| Risk & Compliance wants a standardized tiering model | Low | High if wrong, since it reframes the whole governance pitch | Talk to an actual Risk stakeholder before Phase 2, even informally |
| Germany's launch timeline holds at 2026 | Medium | Medium | Design the platform to be useful even if Germany slips, since UK already benefits from a shared data/comms layer |

**Summary:**
- Markets affected: 2 today, more over time
- Cost impact: reuse should drive cost-per-market down, unverified until Germany onboards
- Strategic value: High, this closes a real regulatory gap and a real multi-market scaling problem at the same time

**Alternatives Considered:**
- Buy a full Adobe suite (RTCDP plus AEM) - not doing this as the default, because it's a bigger commitment than the platform needs and doesn't solve the AI governance gap on its own. Worth pursuing if JPMC already runs Adobe internally.
- Do nothing, let each market rebuild - not doing this, because it's the actual baseline this platform is trying to beat on cost and consistency.
- Wait for a federal AI governance standard - not doing this, because SR 26-2 explicitly declined to set one for gen-AI, and waiting means every market invents its own rules in the meantime.

---

## Non-Goals

What this platform is explicitly NOT doing in v1:
- **Naming a specific vendor** - this PRD recommends a category (composable CDP, Segment-class) over Adobe's full suite, but doesn't commit to a named vendor. That needs a real procurement process this PoC can't run.
- **Replacing an existing internal CDP or CMS, if one exists** - unconfirmed whether JPMC already runs one. If it does, this platform integrates with it through the versioned API, it doesn't replace it.
- **Committing to a Germany launch date** - public reporting only confirms "targeted 2026." This platform is designed to be useful regardless of exactly when that lands.
- **Building the Tier 3 model-validation logic from scratch** - the plan is to call into a specialized tool (ValidMind-class) for that work, not duplicate it.

**Trade-offs Made:**
- Choosing speed-to-first-integration over full governance maturity in Phase 1, because the adoption research says that's the bigger risk to solve first.

---

## Success Metrics

**Primary Metric:** Time-to-first-live-integration for a new market team
- Current: unmeasured (assumed multi-week custom build today)
- Target: under 1 business day for a first live data sync
- Timeline: measured from the start of Phase 1

**Guardrail Metrics:** (Must not get worse)
- Percentage of market teams integrating through the platform, not a workaround: shouldn't drop below 80%
- Governance review cycle time (Tier 1): under 8 hours
- Customer opt-out rate on AI-driven communications: flat or improving, not just "personalization adoption rising"

**Kill Criteria:**
If time-to-first-integration stays above 3 business days after two full quarters of iteration, we'll treat this as evidence the platform isn't actually faster than the workaround, and reconsider the build-vs-buy call entirely rather than keep investing in the same architecture.

---

## Rollout Plan

**Approach:** Phased rollout, engineering-first

**Phase 1 (Q1-Q2):** Platform API contract, composable CDP integration, consent store, and Tier 1 AI governance (automated checks) ship together, not governance-after-foundation.
- Passing criteria: time-to-first-integration under 1 day, measured with a real or simulated market team

**Phase 2 (Q3):** First market onboarding. Sequencing between UK-first (matching NatWest and Lloyds' existing UK AI features) and Germany-parallel (matching the original public launch target) is an open decision, not resolved in this PRD.
- Passing criteria: Legal sign-off per market, since privacy law differs between UK, Germany, and US federal rules

**Phase 3 (Q4):** Tier 2 and Tier 3 governance go live, along with the customer-trust dashboard.
- Passing criteria: reuse rate above 65%, opt-out rate holding flat or better

**Rollback Plan:**
If a Tier 3 (credit or account-decision) feature ships without a complete audit trail, that feature gets pulled immediately and routed back through manual review until the gap is fixed. No exceptions on this one.

---

## AI Behavior Contract (for the Governance & Policy Engine)

| Dimension | Specification |
|-----------|--------------|
| **Primary Task** | Classify an AI-adjacent feature request into Tier 1, 2, or 3, and return the required review path |
| **Inputs Available** | Feature description, data sources touched, customer-facing surface, whether it influences a credit or account decision |
| **Constraints** | Must never auto-approve anything touching a credit or account decision (that's always Tier 3) |
| **Disallowed** | Silent approval of any feature the classifier is uncertain about (defaults to Tier 2 pending human review, never defaults to Tier 1) |
| **Latency Budget** | P50: under 2 seconds / P95: under 10 seconds for classification |

**Behavior Examples:**

| Scenario | Feature Description | Expected Output | Category |
|----------|------------|-----------------|-------------------|
| Happy path, low risk | Marketing copy A/B test variants | Tier 1, automated approval | Good |
| Happy path, medium risk | Next-best-action messaging using purchase history | Tier 2, policy check plus logged spot-check | Good |
| Happy path, high risk | AI-drafted credit decision narrative | Tier 3, full human-in-the-loop, routes to existing model-risk process | Good |
| Edge case, ambiguous | Feature description doesn't clearly match a known pattern | Defaults to Tier 2 pending human review, never silently ships | Bad, handled gracefully |
| Should reject | Feature that would auto-approve a credit line increase with no human check | Classifier flags as Tier 3 and blocks any attempt to route it through Tier 1 or 2 | Reject |

---

## Risks and Recovery

| Risk | Detection | Fallback | Kill Switch |
|------|-----------|----------|-------------|
| Engineers route around the platform because it's slower than the workaround | Time-to-first-integration metric stays above target after Phase 1 | Pause new feature work, focus entirely on integration speed until the metric moves | Platform PM |
| SR 26-2's gen-AI exclusion gets revised, changing the regulatory footing | Fed publishes updated guidance | Review governance model within one quarter of any SR 26-2 update | Platform PM plus Risk & Compliance liaison |
| Risk & Compliance doesn't actually want a standardized tiering model | Direct stakeholder pushback during Phase 1 pitch | Fall back to a lighter-weight version that logs decisions without forcing a tier, then re-propose the full model once trust is built | Platform PM |
| A Tier 3 feature ships without a complete audit trail | Governance dashboard shows an incomplete record | Immediate rollback per the Rollback Plan above | Risk & Compliance |

---

## Open Questions

- [ ] UK-first or Germany-parallel sequencing? NatWest's Cora and Lloyds' Envoy are already live in the UK market, which the original RICE-only roadmap didn't account for. - @Platform PM, needs a real stakeholder decision
- [ ] Does JPMC already run a CDP, CMS, or comms vendor internally? Changes the buy-side recommendation from "adopt Segment-class" to "integrate with what's there." - @Vendor Management
- [ ] Would actual Risk & Compliance stakeholders want this tiering model, or prefer case-by-case discretion? The single highest-leverage assumption to check before Phase 2. - @Risk & Compliance liaison
- [ ] Is there a ValidMind-class tool already inside JPMC's model-risk stack that Tier 3 should call into? - @Engineering Lead

---

## Appendix

Full research trail: [user-research-synthesis-icb-trust-platform-v2.md](user-research-synthesis-icb-trust-platform-v2.md), [competitor-analysis-icb-trust-platform-v2.md](competitor-analysis-icb-trust-platform-v2.md), [01_research_notes.md](01_research_notes.md), [market-research-icb-trust-platform-2026-08-17.md](market-research-icb-trust-platform-2026-08-17.md).
