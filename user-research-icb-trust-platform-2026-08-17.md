## User Research: ICB Trust Platform — Three-Persona Synthesis

*Prepared as part of a Platform Product Manager (VP) job-application PoC. This is a **synthesis exercise, not primary research** — the platform doesn't exist, so there are no interviews, surveys, or usage data to draw from. Every claim below is either (a) cited from real, published research on comparable populations, or (b) explicitly flagged as an inference from domain-standard patterns. Nothing here should be read as reflecting actual JPMC employee or customer input.*

---

### 1. Research Objective

**Question**: If this platform were built, would the three groups who touch it — internal builders, risk/compliance reviewers, and end customers — actually adopt it, resist it, or ignore it? And which of our design assumptions (Section 5 of the strategy doc: sync/async split, consent-as-exception, tiered AI governance) are riskiest if those groups behave differently than assumed?

**Decision this informs**: Whether the platform's phased rollout (Section 8 of the PRD) sequences the right work first — specifically, whether "build the governance service" or "make the integration effortless for engineers" is the higher-leverage early bet.

### 2. User Persona

**Who was "studied"**: No one — this is inference from three sources: (1) published research on internal-developer-platform adoption generally, (2) published consumer research on banking trust/personalization/privacy, (3) the SR 26-2 regulatory text and general risk-function operating patterns in regulated industries. No JPMC-specific individual, team, or customer was consulted.

**Recruitment criteria**: N/A — synthesis only. *If this were a real initiative, actual recruitment would target: 3–5 engineers/PMs from a real or hypothetical market-launch team; 2–3 Risk & Compliance reviewers who've handled an AI-adjacent feature approval; and a representative sample of ICB UK customers (existing) plus prospective German customers (survey-only, pre-launch).*

### 3. Jobs to Be Done

**Persona A — Internal platform user (market launch team)**
- *Primary job (functional)*: Get customer data, messaging, and content infrastructure working for a new market launch, on a deadline set by the business, without becoming the team that reinvents integration patterns from scratch.
- *Secondary jobs (emotional/social)*: Not be blamed for launch delays; not be the team whose AI feature becomes the compliance incident that sets a bad precedent for everyone else.
- *Current workarounds (inferred)*: In the absence of a shared platform, teams under launch pressure default to the fastest path available — direct vendor integration, custom scripts, or "raw" solutions that bypass any shared standard. This mirrors a documented pattern in internal-platform research generally: **64% of engineers still bypass purpose-built platform tooling in favor of direct, lower-level solutions when the platform is slower than going around it** — "not laziness, but engineers rationally optimizing for the fastest path to their actual job." [Platform Engineering research, via HackerNoon/platformengineering.org](https://hackernoon.com/internal-developer-platforms-are-booming-but-adoption-is-failing)

**Persona B — Risk & Compliance stakeholder**
- *Primary job (functional)*: Approve or block AI-adjacent features fast enough that the business doesn't route around Risk entirely, while still being able to defend every approval if a regulator or auditor asks.
- *Secondary jobs*: Not be the bottleneck that gets blamed for slow time-to-market; not be the reviewer who approved something that later became a public incident.
- *Current workarounds (inferred)*: Given SR 26-2 leaves gen-AI/agentic AI outside formal MRM scope, the domain-standard pattern is that risk functions fall back to "broader risk management and governance practices" on a case-by-case basis — i.e., **ad hoc, non-standardized review**, which is slower and less defensible per-instance than a codified framework, even though no such framework is federally mandated yet.

**Persona C — End customer (ICB UK / prospective Germany)**
- *Primary job (functional)*: Bank digitally without friction, and receive communications that are relevant rather than noise.
- *Secondary jobs*: Feel that the bank is handling their data responsibly, especially now that AI is visibly part of banking products.
- *Current state (cited, not inferred)*: Consumers are genuinely torn — **64% of consumers prefer personalized experiences, but only 39% believe it's worth the privacy trade-off**, and 53% report being highly concerned about data privacy even while wanting personalization. [Qualtrics XM Institute, 2025](https://www.qualtrics.com/m/www.xminstitute.com/wp-content/uploads/2025/03/XMI_RR-DS_ConsumerPreferencePrivacyPersonalization-2025.pdf) Separately, **88% of respondents say they trust their bank to protect their data** — but that trust is explicitly described as fragile, not durable. [Integris, 2026](https://integrisit.com/blog/the-integris-banking-trust-and-technology-outlook-for-2026-increases-in-technology-spending-but-also-fragile-trust/)

### 4. Key Findings

1. **The platform's real adoption risk isn't Risk & Compliance — it's engineering apathy.** Published IDP research finds **45.3% of platform teams cite developer adoption, not technical complexity, as their top challenge**, and that the dominant failure mode is building the platform "as an infrastructure project rather than a product" and mandating use before it's actually better than the status quo. [platformengineering.org, 2026](https://platformengineering.org/blog/the-biggest-challenges-platform-engineering-teams-are-facing-in-2026) This directly threatens the PRD's Section 8 sequencing, which puts governance ahead of a compelling day-one integration experience for market teams.
2. **AI anxiety among bank customers is now specific, not vague.** 52% of customers worry AI could mistakenly freeze their account, and 40% fear AI could expose personal data. [amraandelma.com / cited via search synthesis] This means the Tier-3 "human-in-the-loop for credit/account decisions" design choice (governance simulator) isn't just a regulatory hedge — it's addressing a named, majority-adjacent customer fear.
3. **A single data breach is disproportionately costly to trust.** 62% of customers say they'd lose confidence in their bank after a breach, and 43% say they'd stop engaging entirely. [ABA Banking Journal survey, 2025](https://bankingjournal.aba.com/2025/03/survey-most-customers-trust-their-banks-to-keep-their-data-secure/) This raises the stakes on the consent-store design (Section 5.2 of the strategy doc) — the "sync write, single source of truth" choice isn't over-engineering; it's proportionate to what a single miss actually costs.
4. **Risk & Compliance's real incentive, absent a federal framework, is speed-with-defensibility, not caution alone.** SR 26-2's exclusion of gen-AI doesn't mean Risk wants slower review — it means every review is currently bespoke and harder to defend after the fact. A standardized internal tiering model is plausibly something Risk would *want*, not just tolerate — this reframes governance from "a gate market teams resent" to "a tool Risk itself is under-served by today."
5. **Customer appetite for personalization is real but conditional on visible trust signals**, not unconditional — 64% want it, but only 39% think it's worth the trade-off today. This suggests the platform's success metric shouldn't just be "personalization deployed" but "personalization deployed *and* trust maintained" — the two aren't automatically correlated.

### 5. Pain Points & Unmet Needs

*Ranked by inferred frequency/severity, most acute first:*

1. **[Persona A, severe, likely frequent]** Platform is slower than the workaround → teams route around it, exactly as the 64%-bypass IDP statistic suggests happens generally. Gap between current (assumed ad hoc, per-market builds) and ideal (a platform genuinely faster than building it yourself) is the platform's single biggest existential risk.
2. **[Persona B, severe, moderately frequent]** No standardized, defensible review process for gen-AI features post-SR-26-2 → each review is a one-off, slower and riskier for the reviewer personally. Gap between current (ad hoc judgment calls) and ideal (a tiering model with an audit trail) is large and, per Finding 4, may be *wanted* rather than resisted.
3. **[Persona C, moderate, latent]** Desire for relevant communications coexists with active distrust of how that relevance is generated (52% AI-freeze fear, 53% privacy concern). Gap between current (banks personalizing anyway, per the "Financial Brand" finding that banks keep personalizing while customers keep leaving) and ideal (personalization customers actually trust) is a real, documented industry-wide gap, not one specific to this platform. [The Financial Brand](https://thefinancialbrand.com/news/personalization/banks-keep-personalizing-customers-keep-leaving-195183)
4. **[Persona A, moderate, likely at launch]** Under launch-deadline pressure, market teams' bandwidth for learning a new platform is lowest exactly when the platform is supposed to help most — a timing mismatch inherent to any "platform launches alongside first real market" strategy (as this PRD's Germany-parallel-track proposes).

### 6. Implications for Product

- **Resequence, don't just re-message, the roadmap.** The RICE-scored roadmap in the strategy doc already ranks Tier-1 governance highest by score — this research adds a sharper reason: if the day-one engineering experience isn't visibly faster than the workaround, the governance layer will have nothing routed through it to govern. **Recommend**: add an explicit "integration time-to-first-success" target (e.g., a market team's first live data sync in under 1 day) as a Phase-1 gate, not just a lagging metric.
- **Reframe governance's audience internally.** Finding 4 suggests pitching the Tier 1/2/3 model to Risk & Compliance as "a tool that makes your reviews faster and more defensible," not "a constraint you're being asked to accept." This is a stakeholder-management/positioning change, not an architecture change.
- **Add a customer-trust metric, not just a personalization-adoption metric.** Section 7 of the strategy doc doesn't currently include a customer-trust proxy (e.g., opt-out rate trend, complaint rate tied to AI-driven communications). Given Finding 5, "personalization shipped" without a trust counter-metric risks optimizing the wrong side of the trade-off the research shows customers are actually weighing.
- **Treat the Consent Store design as validated, not gold-plated.** Given Finding 3's breach-trust-cost data, the earlier VP-critique concern that the sync-consent-store design might be "over-engineering" is not supported by this research — the cost of getting it wrong is asymmetrically high enough to justify the added complexity.

---

**Open Questions**
- Would actual JPMC market-launch engineers experience the same 64%-bypass dynamic, or does JPMC's engineering culture (formal MACH Alliance membership, existing platform discipline) already mitigate it more than the general IDP research suggests?
- Is Risk & Compliance's real appetite for a standardized tiering model (Finding 4) accurate, or would actual JPMC risk stakeholders prefer to keep case-by-case discretion specifically because SR 26-2 doesn't mandate otherwise? This is the single highest-leverage assumption to validate before Phase 2 (governance tiering) is built.
- What specific opt-out/complaint-rate baseline exists today (pre-platform) that a "customer trust" metric could be measured against — without a baseline, the recommended trust counter-metric in Section 6 has nothing to compare to.
- Do German prospective customers (no current relationship with Chase) exhibit the same personalization/privacy trade-off numbers as the general and UK-specific research cited here, or does GDPR-native consumer expectation shift the balance further toward privacy?
- Would UK customers' expectations be already anchored by NatWest's Cora and Lloyds' Envoy (per the market-research report) such that Chase UK is now competing against an already-normalized AI-personalization bar, not a blank slate — this changes how "success" should be defined for the platform's first deployment.
