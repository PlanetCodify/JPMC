# User Research Synthesis: ICB Trust Platform
**Date:** August 17, 2026
**Interviews:** 0 (see note below)
**Synthesized by:** Product Team (PoC, Platform PM VP application)

**A note before you read this.** This isn't synthesis of real interviews. The platform doesn't exist yet, so there's no one to interview. What follows is built from published research on comparable populations (bank customers, internal platform engineers, regulated-industry risk teams), with every claim traced to a source. Treat it the way you'd treat a well-researched hypothesis, not validated findings. The "Missing Segments" section at the end says this more bluntly.

---

## Executive Summary

**Top 3 Insights:**
1. The platform's biggest threat isn't Risk & Compliance pushback. It's engineers ignoring it because it's slower than just building the thing themselves. That's the documented failure mode for most internal platforms.
2. Risk & Compliance's actual problem right now, since SR 26-2 left gen-AI out of formal model-risk rules, is that every AI feature review is a one-off judgment call. A standard tiering model might be something they'd want, not something they'd fight.
3. Customers are stuck between wanting personalization and not trusting the trade-off. Only 39% think it's worth the privacy cost, even though 64% say they want it. Building this platform without a trust metric alongside the personalization metric misses that tension entirely.

**Recommended Actions:**
1. Build first: a fast, obvious integration path for engineers. If a market team can't get a live data sync working in under a day, nothing else about this platform matters.
2. Explore next: pitching the AI governance tiering model to Risk & Compliance as something that makes their job faster and easier to defend, not a gate they have to accept.
3. Deprioritize: shipping personalization features before a trust metric (opt-out rate, complaint rate) exists to catch the downside.

---

## Theme 1: Engineers Route Around Slow Platforms
**User Impact:** Applies to every market launch team that would touch this platform
**Severity:** High (this is the platform's existential risk)
**Current Workaround:** Direct vendor integration or custom scripts, whatever gets the job done fastest

### The Problem
When an internal platform is slower than the alternative, engineers don't use it. They build around it. This isn't a training problem or a culture problem you can fix with a mandate. It's engineers doing the rational thing given their actual incentives.

### Supporting Evidence
- **Data point:** 45.3% of platform teams cite developer adoption, not technical complexity, as their top challenge. [platformengineering.org, 2026](https://platformengineering.org/blog/the-biggest-challenges-platform-engineering-teams-are-facing-in-2026)
- **Data point:** 64% of engineers still bypass purpose-built platform tooling for a faster, lower-level path when the platform gets in the way. [HackerNoon, via platformengineering.org research](https://hackernoon.com/internal-developer-platforms-are-booming-but-adoption-is-failing)
- **Root cause pattern:** the most common failure mode is treating the platform as an infrastructure project instead of a product, then mandating adoption before it's actually better than what people already do.

### Recommended Solution
**Build:** a fast integration path first. Specifically, get a market team from zero to a live data sync in under a business day.

**Why this solution:**
- Addresses the root cause directly: speed beats mandate, every time this has been studied
- Fits how engineers actually behave, not how we'd like them to behave
- Matches the pattern in the research: platforms succeed when they're the fastest path, not the sanctioned one

**What NOT to build:**
- A governance-first rollout, where teams have to clear a compliance review before their first integration. This is the exact sequencing that research says fails.

**Success Metrics:**
- Primary: time from platform onboarding start to first live data sync (target: under 1 business day)
- Guardrail: percentage of market teams who complete integration through the platform vs. a workaround (shouldn't drop below 80%)

**Open Questions:**
- [ ] Does JPMC's engineering culture (MACH Alliance membership, existing platform habits) make this less likely than the general research suggests? - @Engineering Lead
- [ ] What does "slower than the workaround" actually mean in hours, for an ICB market team specifically? - @Platform PM

---

## Theme 2: Risk & Compliance Has No Standard to Point To
**User Impact:** Every AI-adjacent feature that touches customer data, going forward
**Severity:** High (creates both a compliance risk and a speed risk)
**Current Workaround:** Ad hoc, case-by-case judgment calls, presumably

### The Problem
SR 26-2 (the Fed's April 2026 guidance) explicitly leaves generative and agentic AI out of formal model-risk scope. That's not a loophole Risk & Compliance is exploiting. It's a gap they're stuck operating in, without a rulebook. Every review becomes a first-time decision, which is slower to make and harder to defend later if anyone asks why.

### Supporting Evidence
- **Regulatory fact:** SR 26-2 (April 17, 2026), superseding SR 11-7, explicitly excludes generative and agentic AI from formal model-risk scope for banks over $30 billion in assets. [Federal Reserve, primary source](https://www.federalreserve.gov/supervisionreg/srletters/SR2602.htm)
- **Inference, not confirmed:** absent a federal template, the standard pattern in regulated industries is that risk functions fall back on "broader risk management practices," meaning ad hoc review rather than a repeatable process.

### Recommended Solution
**Build:** a tiered governance model (Tier 1 automated, Tier 2 policy-plus-spot-check, Tier 3 human-in-the-loop), pitched to Risk & Compliance as a tool that makes their reviews faster and more defensible.

**Why this solution:**
- Addresses the root cause: no standard exists today, so build one instead of waiting for the Fed to
- Reframes governance from "a constraint on the business" to "a tool for the reviewer," which changes who's likely to champion it internally
- Gives every review an audit trail, which matters if a regulator or auditor asks later why something was approved

**What NOT to build:**
- A governance model that only serves as a gate for engineering teams, without making Risk & Compliance's own job easier. That version gets resisted by both sides.

**Success Metrics:**
- Primary: governance review cycle time by tier (target: Tier 1 under 8 hours)
- Guardrail: percentage of Tier 3 decisions with a complete audit trail (should be 100%, no exceptions)

**Open Questions:**
- [ ] Would actual Risk & Compliance stakeholders want a standardized model, or would they prefer to keep discretion precisely because SR 26-2 doesn't require otherwise? This is the single riskiest unvalidated assumption in this whole synthesis. - @Risk & Compliance liaison

---

## Theme 3: Customers Want Personalization But Don't Trust the Trade-off
**User Impact:** Every ICB customer, UK live today, Germany prospective
**Severity:** Medium (latent, not blocking, but shapes what "success" should mean)
**Current Workaround:** None. Banks are personalizing anyway, and customers are tolerating it without necessarily trusting it.

### The Problem
Customers say they want relevant communications. They also don't think it's worth what it costs them in privacy. Building a platform that optimizes for "more personalization" without watching the trust side of that trade is optimizing half the problem.

### Supporting Evidence
- **Direct data:** 64% of consumers prefer personalized experiences, but only 39% believe it's worth the privacy trade-off. [Qualtrics XM Institute, 2025](https://www.qualtrics.com/m/www.xminstitute.com/wp-content/uploads/2025/03/XMI_RR-DS_ConsumerPreferencePrivacyPersonalization-2025.pdf)
- **Direct data:** 53% of consumers are highly concerned about data privacy even while wanting personalization. [Same source]
- **Direct data:** 52% of banking customers worry AI could mistakenly freeze their account. 40% fear AI use could expose their personal data.
- **Direct data:** 62% of customers say they'd lose confidence in their bank after a data breach. 43% say they'd stop engaging with the bank entirely. [ABA Banking Journal survey, 2025](https://bankingjournal.aba.com/2025/03/survey-most-customers-trust-their-banks-to-keep-their-data-secure/)
- **Industry pattern:** banks keep personalizing and customers keep leaving anyway, according to industry coverage of this exact tension. [The Financial Brand](https://thefinancialbrand.com/news/personalization/banks-keep-personalizing-customers-keep-leaving-195183)

### Recommended Solution
**Build:** a customer-trust metric (opt-out rate, complaint rate tied to AI-driven communications) that ships alongside any personalization metric, not after it.

**Why this solution:**
- Addresses the root cause: personalization and trust aren't the same axis, and treating them as one metric hides the trade-off customers are actually making
- Matches the breach-cost data: getting this wrong costs more than getting it slowly right
- Gives the platform an early warning signal before a trust problem shows up as churn

**What NOT to build:**
- Personalization features with no counter-metric watching for backlash. The research shows this is exactly how you end up personalizing your way into customer distrust.

**Success Metrics:**
- Primary: opt-out rate on AI-driven communications (target: flat or improving, not just "personalization adoption rising")
- Guardrail: complaint rate tied to AI features (any sustained increase triggers a review)

**Open Questions:**
- [ ] What's the current opt-out/complaint baseline, pre-platform? Without one, the trust metric has nothing to compare against. - @Platform PM

---

## Contradictions & Open Questions

**Contradiction:** Theme 1 says ship fast, don't gate on governance. Theme 2 says governance is something Risk & Compliance would want early. These aren't actually opposed (the recommendation is to run both in parallel, not sequence one after the other), but it's worth stating plainly: if forced to pick one first, the adoption research in Theme 1 should win. A governance model with nothing routed through it doesn't help anyone.

**Open assumption running through all three themes:** none of this has been validated with an actual JPMC engineer, risk reviewer, or ICB customer. Every recommendation here is a best guess built on outside research, not inside knowledge.

---

## Missing Segments (Research Gaps)

**Who this synthesis is based on:**
- General internal-platform adoption research (not JPMC-specific)
- General financial-services consumer surveys (US and UK-weighted, not Germany-specific)
- One regulatory text (SR 26-2) read directly, not interpreted by anyone inside a bank's risk function

**Who wasn't consulted, because this is a job-application PoC and they don't exist yet as reachable stakeholders:**
- Any actual JPMC engineer who'd build against this platform
- Any actual JPMC Risk & Compliance reviewer
- Any actual ICB customer, UK or prospective Germany
- Any German-specific consumer research (GDPR-native expectations may shift the privacy/personalization trade-off differently than the UK/US data above suggests)

**Risks this creates:**
- The adoption-bypass finding (Theme 1) comes from general platform-engineering research. JPMC's engineering culture, given its MACH Alliance membership and existing internal AI platform (LLM Suite), might already behave differently than the general population this stat is drawn from.
- The Risk & Compliance "would want this" hypothesis (Theme 2) is the least grounded claim in this document. It's a reasonable guess based on incentives, not a validated preference.
- German consumer attitudes toward AI and data privacy weren't researched directly. GDPR familiarity could make German customers either more comfortable (used to strict data rules) or less comfortable (more attuned to what's being asked of them) than the UK/US figures suggest.

**Recommendation:**
Before Phase 2 of the platform (the governance tiering work), talk to an actual Risk & Compliance stakeholder, even informally, to check Theme 2's core assumption. That's the one finding here that would change the roadmap the most if wrong.

---

## Appendix: Raw Observations

| # | Type | Source | Finding |
|---|---|---|---|
| 1 | Adoption pattern | platformengineering.org, 2026 | 45.3% of platform teams cite developer adoption as top challenge |
| 2 | Adoption pattern | HackerNoon / platformengineering.org | 64% of engineers bypass platforms slower than the workaround |
| 3 | Regulatory fact | Federal Reserve, SR 26-2 | Gen-AI/agentic AI excluded from formal model-risk scope, banks over $30B |
| 4 | Consumer survey | Qualtrics XM Institute, 2025 | 64% want personalization, 39% think it's worth the privacy trade-off |
| 5 | Consumer survey | Qualtrics XM Institute, 2025 | 53% highly concerned about data privacy |
| 6 | Consumer survey | (aggregated banking AI sentiment research) | 52% fear AI could mistakenly freeze their account, 40% fear data exposure |
| 7 | Consumer survey | ABA Banking Journal, 2025 | 62% would lose confidence after a breach, 43% would stop engaging |
| 8 | Industry commentary | The Financial Brand | Banks keep personalizing, customers keep leaving anyway |
