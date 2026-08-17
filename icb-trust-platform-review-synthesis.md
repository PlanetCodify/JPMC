---
prd: icb-trust-platform-solution-review.md
review_date: 2026-08-17
stage: Solution Review
agents: engineer, designer, executive, legal, uxr, skeptic, customer
---

# PRD Review Synthesis: ICB Trust Platform

**Reviewed:** August 17, 2026
**Current Stage:** Solution Review
**Reviewers:** Engineering, Design, Executive, Legal, UXR, Skeptic, Customer Voice

---

## TL;DR

**Overall Assessment:** Needs significant work before Phase 1 funding, not because the thesis is wrong, but because three of the PRD's own load-bearing assumptions turned out to be weaker under scrutiny than the document admits, and two whole product surfaces (the integrator experience, the governance reviewer tool) are implied but never specified.

**Critical Blockers:** 4
**Important Gaps:** 5
**Conflicting Perspectives:** 2

**Recommended Next Step:** Don't fund Phases 2-3 yet. Fund a bounded Phase 1 spike only, with the fixes below applied, and force the UK-vs-Germany and Risk & Compliance questions to real answers before anything past Phase 1 gets approved.

---

## Critical Blockers

### 1. The integrator experience and the governance reviewer experience are both undefined

**Flagged by:** Engineering, Design
**Issue:** The entire Phase 1 bet is that engineers won't route around the platform if it's fast enough. But there's no spec for what "fast" actually feels like: no API style, no docs, no sandbox, no error taxonomy, nothing describing what an engineer sees when their sync fails. Same problem on the other side: Tier 2 and Tier 3 both require a human reviewer to act on something, and there's no dashboard, queue, or audit-trail view described anywhere until a vague Phase 3 mention.
**Impact if not fixed:** The primary metric (time-to-first-integration) could get gamed by a shallow test integration while the real experience that determines sustained use goes undesigned. On the governance side, "human-in-the-loop" is a phrase with nothing behind it.
**Recommendation:** Add an "Integrator Experience" subsection to Phase 1 (docs, sandbox, error handling, self-serve status) and a "Governance Reviewer Experience" subsection no later than Phase 2, not Phase 3.
**Owner:** Platform PM plus Engineering Lead

---

### 2. The primary metric is gameable and doesn't measure what actually matters

**Flagged by:** Skeptic, echoed by Design
**Issue:** Time-to-first-integration measures how fast a team can complete a first handshake, not whether they keep using the platform. A team could hit the under-1-day target with a shallow test sync and never touch it again. The metric that actually matters, reuse rate, is currently a Phase 3 target, not something tracked from day one.
**Impact if not fixed:** The kill criteria is keyed to speed, not reuse. A genuinely unused platform could sail past its own kill criteria while quietly failing at its actual job.
**Recommendation:** Promote a 90-day sustained-reuse metric to co-primary status alongside time-to-first-integration. Demote time-to-first-integration to a guardrail if needed to keep the metric section honest about what it's actually protecting against.
**Owner:** Platform PM

---

### 3. Legal and regulatory specifics are named but not operationalized

**Flagged by:** Legal
**Issue:** GLBA, cross-border data transfer (UK/Germany), and vendor data processing terms are referenced in research but not written into the PRD as requirements. More seriously: the Tier 1/2/3 classifier is a system that self-routes decisions that could touch fair lending (ECOA/Reg B) scrutiny, and nothing in the PRD confirms the existing model-risk process Tier 3 "routes to" actually covers gen-AI outputs feeding credit decisions.
**Impact if not fixed:** This is the difference between a PRD that reads well and a system that survives a real compliance review. The self-governance concern (a system deciding its own review tier for credit-adjacent features) is exactly the kind of thing an examiner would probe.
**Recommendation:** Before any Phase 1 build, get a fair-lending review of the classifier's self-routing logic, and confirm in writing that the "existing model-risk process" Tier 3 depends on actually has scope for gen-AI outputs. Add a GLBA notice/opt-out mechanics spec, not just an opt-out rate metric.
**Owner:** Legal, with Risk & Compliance liaison

---

### 4. UK-vs-Germany sequencing has to be decided before Phase 1, not during Phase 2

**Flagged by:** Executive
**Issue:** NatWest's Cora and Lloyds' Envoy are live now, in the same UK market Chase competes in. The PRD leaves this as an open question, which means the roadmap in front of a VP is missing its most consequential prioritization call.
**Impact if not fixed:** Funding a roadmap with its biggest sequencing decision unresolved is asking for a blank check, not a plan.
**Recommendation:** Force this decision before Phase 1 kickoff. It changes what "market onboarding" in Phase 2 even means.
**Owner:** Platform PM, with a real stakeholder decision (not solvable inside this PoC)

---

## Important Gaps

### Missing: an honest reckoning with Risk & Compliance's incentive to resist, not just "low confidence"

**Flagged by:** Skeptic, UXR, Executive (three separate agents, independently)
**Gap:** The PRD calls Risk & Compliance's appetite for standardized tiering a "low confidence" assumption. The Skeptic review makes a sharper case: standardized tiering creates an audit trail that assigns Risk blame when the classifier is wrong, and automating Tier 1/2 could shrink the manual-review function Risk currently owns. That's a real resistance incentive, not just an unknown.
**Risk:** If Risk & Compliance actively resists rather than passively doubts, the entire governance value proposition, the second pillar of this platform, could collapse in Phase 2.
**Recommendation:** Talk to an actual Risk stakeholder before Phase 1 completes, not after. This is now the single highest-priority validation in the whole document.

---

### Missing: the disconfirming case that JPMC's engineers might not behave like the general 64%-bypass population

**Flagged by:** UXR
**Gap:** JPMC is a MACH Alliance member with an existing internal AI platform (LLM Suite). That's a real reason JPMC's engineers might already have better platform habits than the general population the 64% stat is drawn from. This point exists in the research appendix but never made it into the PRD's own Open Questions.
**Risk:** A kill criterion built on an unvalidated external benchmark could trigger (or fail to trigger) for the wrong reasons.
**Recommendation:** Add this to the PRD's Open Questions directly, and get one lightweight internal signal on current JPMC integration time before treating the 3-day threshold as binding.

---

### Missing: a customer consent UX, not just a consent policy

**Flagged by:** Customer Voice
**Gap:** The 39%-vs-64% personalization/privacy stat is about real people deciding whether to trust a consent flow. The PRD treats this as solved by "consent design" at the architecture level, without describing the actual consent screen or flow a customer sees.
**Risk:** Consent UX is where trust is typically lost (buried settings, pre-checked boxes), and the PRD's architecture-level fix doesn't reach that layer.
**Recommendation:** Add a note that the consent UX itself needs its own design pass, separate from the technical consent-store architecture.

---

### Missing: the cheapest alternative in the comparison set

**Flagged by:** Skeptic
**Gap:** "Alternatives Considered" compares building the platform against buying Adobe, doing nothing, or waiting for a federal standard. It never considers "Germany copies the UK's existing setup," which is the actual null hypothesis that would make this platform unnecessary.
**Risk:** A PRD that doesn't name its cheapest real alternative looks like it's building the comparison set to win, not to inform.
**Recommendation:** Add this alternative explicitly, with a real reason it's insufficient (if one exists) rather than omitting it.

---

### Missing: a stated exposure window for the two quarters when only Tier 1 governance is live

**Flagged by:** Customer Voice, echoing Legal's liability point
**Gap:** Phase 1-2 ship with only automated Tier 1 checks live. Tier 2 and Tier 3 don't land until Q4. Nothing in the PRD names what covers the gray-area cases in that window.
**Risk:** If something risky slips through during that window, "the safety tier wasn't built yet" is a bad position to defend from, to a regulator or a customer.
**Recommendation:** Add an explicit interim-coverage statement for the Phase 1-2 window.

---

## Enhancements to Consider

**From Engineering:**
- Define an audit-trail data model (what fields make a record "complete") before Phase 1 exit, since the rollback plan depends on this being machine-checkable.
- Add a classifier fail-safe statement: if the classifier itself is unavailable, default to Tier 2, matching the ambiguous-case rule.

**From Design:**
- Add a baseline accessibility flag for any Phase 3 reviewer dashboard, since compliance teams are exactly the audience where accessibility gets audited.

**From Legal:**
- Get Procurement and Legal to gate vendor selection for the composable CDP, not just Engineering, given GLBA data processing agreement requirements.

**From Skeptic:**
- Consider shipping the governance/tiering engine as a standalone service first, decoupled from the CDP/CMS/comms bundle, to de-risk the highest-uncertainty assumption (Risk & Compliance appetite) before spending on infrastructure.

**From Customer Voice:**
- Publish the Tier definitions and rollback trigger in plain language, similar to how a privacy policy addendum reads, if this ever becomes real.

---

## Conflicting Perspectives

### Conflict 1: Should the AI governance engine be bundled with the CDP/CMS/comms platform, or shipped standalone?

**Perspective A** (Engineering, Executive, implicitly): the bundled scope is reasonable. Governance-as-a-service sitting on top of a shared data/comms layer is the platform's actual differentiator, and building them together is more coherent than two separate initiatives.

**Perspective B** (Skeptic): if the governance piece is the urgent, differentiated part (closing the SR 26-2 gap), it doesn't need to ride on top of a full data-platform build. Shipping it standalone first would validate the riskiest assumption (Risk & Compliance appetite) before committing to the bigger, more expensive CDP/CMS integration.

**Decision needed:**
- Whether to de-scope Phase 1 to governance-only, validate with Risk & Compliance, then decide whether the CDP/CMS bundle is still justified.
- Trade-off: bundling is more efficient if both are needed anyway; unbundling is safer if Risk & Compliance's appetite turns out to be the wrong bet.
- No recommendation given here. This is a real strategic fork, not a details question.

---

### Conflict 2: Is speed-to-first-integration the right thing to optimize first, or does it risk hollow adoption?

**Perspective A** (this PRD's own Rollout Plan, and Engineering's implicit acceptance): ship the fast path first, since the biggest documented risk is engineers bypassing a slow platform. Speed has to come before governance maturity.

**Perspective B** (Skeptic, Design): optimizing for first-sync speed alone is gameable, and without a sustained-reuse metric running in parallel from day one, the platform could look successful on paper while going unused in practice.

**Decision needed:**
- Whether to keep time-to-first-integration as the sole Phase 1 gate, or require both time-to-first-integration and a real 90-day-reuse check-in before declaring Phase 1 a pass.
- Recommendation: track reuse from day one even if it's not a hard Phase 1 gate yet, so the data exists when Phase 3's reuse target comes due.

---

## Detailed Feedback by Perspective

### Engineering Review

**Strengths:** Phasing is sound (foundation and Tier 1 governance shipping together, not governance-after). Build-vs-buy calls are sensible. The AI Behavior Contract's default-deny posture (never auto-approve credit decisions, ambiguous defaults to Tier 2) is the right instinct.

**Concerns:** The "Platform API contract" has no actual spec (style, versioning, auth, multi-tenancy). The classifier's latency budget has no named architecture. Cross-border data residency for the consent store isn't addressed as a design fork, only as a legal footnote.

**Blockers:** No audit-trail data model exists despite the rollback plan depending on one. No monitoring/observability section, especially for classifier drift or false-negative tracking (a Tier 3 feature misclassified as Tier 1 is the worst failure mode here).

**Estimated Complexity:** M-L for Phase 1, contingent on resolving the two architecture-blocking Open Questions (existing vendor stack, existing model-risk tooling) early.

---

### Design Review

**Strengths:** The "never defaults to Tier 1 on ambiguity" rule is the single best UX decision in the document. Time-to-first-integration correctly treats integrator friction as a real UX problem.

**Concerns:** No integrator-facing surface described at all (docs, sandbox, errors). No governance reviewer UI described until a vague Phase 3 mention.

**Blockers:** Can't claim "a consistent, defensible review process" without specifying the tool reviewers actually use.

**Usability Risk:** High, specifically because the platform's core risk (workaround behavior) is a UX problem the PRD hasn't designed for yet.

---

### Executive Review

**Strengths:** Correctly timed against SR 26-2 and Germany's public 2026 target. Build-vs-buy call matches the actual market structure, not a default-to-build instinct. Kill criteria and rollback plan are real, not decorative.

**Concerns:** ROI is structurally unquantifiable as written, every dollar figure is explicitly illustrative. That's fine for a discovery pitch, not sufficient to fund a three-phase build.

**Blockers:** UK-vs-Germany sequencing is unresolved at exactly the moment it matters most for a funding decision.

**Business Impact:** Medium, pending the sequencing decision and Risk & Compliance validation.
**Strategic Fit:** Strong in direction, weak in current specificity.

---

### Legal Review

**Strengths:** The rollback plan (mandatory pull on incomplete Tier 3 audit trail) is the PRD's strongest risk control.

**Concerns:** GLBA, cross-border data transfer, and vendor DPA terms are named in research but not written into the PRD as requirements.

**Blockers:** The classifier's self-routing logic for credit-adjacent features needs a fair-lending (ECOA/Reg B) review before any build. Unconfirmed whether the "existing model-risk process" Tier 3 depends on actually has scope for gen-AI outputs.

**Legal Risk:** High until the four items in Critical Blocker 3 are resolved.
**Requires Legal Team Review:** Yes, before Phase 1 build starts.

---

### UX Research Review

**Strengths:** Every cited stat traces to a real external source. Unusually disciplined about labeling its own evidentiary weakness compared to most PRDs.

**Concerns:** The Hypothesis section cites research more confidently than the research document itself does. Two disconfirming considerations (JPMC's existing platform maturity, Germany/GDPR-specific attitudes) exist in the research appendix but never made it into the PRD's own Open Questions.

**Blockers:** None outright, but the Kill Criteria depends on an unvalidated external benchmark (the 64% bypass stat) being treated as binding for a specific company it was never tested against.

**Research Validation:** Moderate. Real sources, wrong population.

---

### Skeptic Review

**Questions to Answer:** Why does closing a governance gap require building shared data infrastructure? What happens if Germany just copies the UK's stack? Why is Risk & Compliance's resistance case treated as "low confidence" when there's a real incentive-based argument for active resistance?

**Risky Assumptions:** The 8-12 features/year sizing figure is sourced from competitors' public roadmaps, not any ICB-specific signal. The entire cost/impact calculation downstream inherits that guess.

**Flawed Logic:** Time-to-first-integration as primary metric rewards a fast first handshake, not sustained use, and the kill criteria could fail to trigger for a platform that's technically fast but practically unused.

**Alternatives:** Ship governance standalone first, decoupled from the CDP/CMS bundle, to validate the riskiest assumption before the bigger spend.

---

### Customer Voice Review

**User Value:** The Tier 3 "never auto-approve credit decisions" rule is exactly what a customer wants. The ambiguous-defaults-to-Tier-2 rule reads as a bank designing against its own incentive to move fast.

**User Friction:** Two full quarters where only Tier 1 governance is live is a real exposure window, and "we hadn't built the safety tier yet" isn't a reassuring answer if something goes wrong in that window.

**User Rejection:** Marketing this as "AI safety" externally when it's really an internal engineering-adoption play would read as dishonest if customers ever saw both framings side by side.

**User Sentiment:** Would use it, cautiously, contingent on seeing the actual consent flow rather than just a governance tier that references consent abstractly.

---

## Action Items

**Before Next Review:**
- [ ] Add Integrator Experience and Governance Reviewer Experience subsections - Owner: Platform PM + Engineering Lead
- [ ] Replace or supplement the primary metric with 90-day sustained reuse - Owner: Platform PM
- [ ] Get a fair-lending review of the Tier 1/2/3 classifier's self-routing logic - Owner: Legal
- [ ] Force the UK-vs-Germany sequencing decision - Owner: Platform PM, real stakeholder input needed

**Before Launch:**
- [ ] Talk to an actual Risk & Compliance stakeholder about tiering appetite - Owner: Risk & Compliance liaison
- [ ] Add the MACH/LLM Suite disconfirming point and Germany/GDPR gap to the PRD's own Open Questions - Owner: Platform PM
- [ ] Define audit-trail data model - Owner: Engineering Lead
- [ ] Design the actual customer consent UX, separate from the technical consent-store architecture - Owner: Design (if this becomes real)

**Decisions Needed:**
- [ ] Bundle governance with CDP/CMS, or ship governance standalone first - Owner: PM, needs real judgment call
- [ ] Whether reuse-at-90-days becomes a hard Phase 1 gate or stays a tracked-but-not-gating metric - Owner: PM

---

## Next Steps

1. **Immediate:** Update the PRD to add the four Critical Blocker fixes and promote the buried Open Questions.
2. **This week:** Decide the bundling question (Conflict 1), since it changes Phase 1's actual scope.
3. **Before next stage:** Get the fair-lending review and the Risk & Compliance conversation started, since both are now flagged as the highest-leverage unknowns by multiple independent reviewers.

**Recommended:**
- Update the PRD with this feedback before treating it as ready for a real stakeholder review.
- Treat the two Conflicting Perspectives as PM judgment calls, not oversights, and document whichever way they get resolved.

---

*Generated: August 17, 2026*
*Agents used: 7 (Engineering, Design, Executive, Legal, UXR, Skeptic, Customer)*
*Next: Update PRD to close Critical Blockers*
