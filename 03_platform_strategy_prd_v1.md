# ICB Trust Platform — Platform Strategy Document (DRAFT v1)
### A Governed Customer Data, Communications & AI Capability Layer for International Consumer Bank

**Author's note**: This is an illustrative product strategy document prepared as part of a job application for the Platform Product Manager (VP) role, International Consumer Bank Digital team. It is grounded entirely in publicly available information (see [01_research_notes.md](01_research_notes.md)) and does not claim knowledge of JPMorgan Chase's actual internal systems, vendors, or roadmap. Where the document makes assumptions, they are labeled explicitly.

---

## 1. Problem Statement

As International Consumer Bank (ICB) expands into new markets on a fully digital, branchless model — Chase UK live since 2021, Germany targeted for 2026 — each market launch team faces the same build: customer data, messaging, and content workflows stood up from scratch, under local privacy law, on a compressed timeline, with no shared platform to inherit integration patterns or governance controls from.

This is compounded by timing: in April 2026, the Federal Reserve's SR 26-2 guidance (superseding SR 11-7) explicitly excluded generative and agentic AI from formal model-risk scope for banks over $30B in assets. Any AI capability a market team wants to add to its digital channels — personalized messaging, next-best-action content — currently has no standardized federal governance model to plug into. Left alone, each market would invent its own AI risk controls, which doesn't scale and risks inconsistent customer protection across markets.

## 2. User Personas

| Persona | Need |
|---|---|
| Internal platform user (market launch team, e.g. Germany) | Reusable integration pattern for customer data/comms/CMS — don't rebuild per market |
| Risk & Compliance stakeholder | Standardized, auditable review pathway for AI-adjacent features, given the SR 26-2 gap |
| End customer | Consistent, relevant communications with data handled uniformly across markets |

## 3. Jobs to Be Done

1. When a new market launches, give the team a reusable pattern for data/comms/CMS integration.
2. When a product team wants to add an AI feature, give them a pre-approved governance pathway.
3. When compliance reviews an AI feature, give them consistent audit trails across markets.
4. When a customer interacts with any market's digital channel, ensure consistent data handling.

## 4. Governance & Vendor Model

**Assumption flagged**: this section proposes a governance *model*, not a specific vendor stack, since Chase's actual vendor choices aren't public.

- **Build vs. buy framing**: the platform's core value is the *governance and integration layer*, not the underlying CDP/CMS/comms tooling itself. Recommend a composable approach (per MACH Alliance pattern, of which JPMorgan is already a member) — buy best-of-breed CDP/CMS/comms components, build the governance/orchestration layer in-house since that's the differentiated, non-outsourceable part.
- **AI governance model**: since SR 26-2 leaves gen-AI/agentic AI outside formal MRM scope, the platform proposes a lightweight internal equivalent — a three-tier review model:
  - Tier 1 (low-risk, e.g. content personalization copy variants): automated policy checks, no human review required.
  - Tier 2 (medium-risk, e.g. next-best-action messaging using customer data): platform-level policy enforcement + logged human spot-check.
  - Tier 3 (high-risk, e.g. anything influencing credit/account decisions): full human-in-the-loop review, tied into existing model-risk processes.
- **Vendor management**: platform team owns the integration contract/SLA layer with any third-party CDP/comms/CMS vendor, so market teams consume a standardized internal API rather than integrating with vendors directly.

## 5. Architecture (black-box level)

```
[Market Launch Team] --- consumes ---> [ICB Trust Platform API Layer]
                                              |
                    -----------------------------------------------------
                    |                    |                    |
            [Customer Data       [Communications      [Content/CMS
             Integration]         Integration]          Integration]
                    |                    |                    |
             (composable CDP)    (comms vendor,       (CMS vendor,
              per-market data     consent-aware)        localized)
              residency handled
              at this layer)
                    |
          [AI Governance & Policy Engine — tiered review, audit log]
                    |
          [Risk & Compliance Dashboard — cross-market visibility]
```

- Data residency/localization handled at the integration layer per market (Germany's data residency requirements differ from UK's).
- AI Governance & Policy Engine sits centrally so every market's AI-adjacent feature passes through the same tiering logic — this is the platform's actual product, more than the CDP/CMS integrations themselves.

## 6. Success Metrics

- Time-to-integrate a new market onto the platform (target: reduce from an assumed multi-quarter build to weeks)
- Time-to-approval for a new AI-adjacent feature (Tier 1/2/3 review cycle time)
- Audit-trail completeness rate across markets
- Platform adoption rate (number of ICB markets/teams onboarded)
- Consent/compliance rate (opt-out honored within SLA, per market's privacy law)

## 7. Requirements (phased)

**Phase 1**: Governance/policy engine + integration API layer (foundation)
**Phase 2**: Onboard Germany launch (first real multi-market test)
**Phase 3**: Extend AI governance tiering to cover agentic use cases (multi-step actions, not just content generation)

## 8. Risks & Regulatory Considerations

- SR 26-2's exclusion of gen-AI/agentic AI is recent and could be revised — the platform's governance model should be reviewed periodically, not treated as permanent.
- CFPB Section 1033 (open banking rule) is currently enjoined/under reconsideration — data-sharing requirements could change; the platform's data layer should be built flexible to open banking API requirements without assuming the rule's final form.
- Cross-market privacy law variance (GLBA + state laws in the US; UK/Germany data protection law) — the platform must not assume a single compliance model works everywhere.

## 9. Open Questions

- What CDP/CMS/comms vendors does Chase already run for consumer digital channels? (Not public — would need internal input.)
- Does JPMC's AI Research program already have a gen-AI-specific governance framework not yet public? (Assume possible — this PoC would need to align with it, not duplicate it.)
- What's the actual Germany launch timeline and platform readiness state? (Public reporting only confirms "targeted 2026.")
