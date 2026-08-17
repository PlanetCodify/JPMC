# Problem Scoping — v2 (FINAL for Week 1)
### Platform PM PoC for JPMC (International Consumer Bank, Digital)

## Framing Device

Written as an **externally-informed product point of view**, not a proposal claiming insider access. Every internal-sounding claim is flagged as assumption/hypothesis against [01_research_notes.md](01_research_notes.md). This document does not claim JPMC lacks AI governance broadly — JPMC's AI Research program and LLM Suite governance are real and referenced. The specific claim is narrower: **generative and agentic AI are excluded from the Fed's April 2026 formal model-risk framework (SR 26-2)**, so any bank operating at JPMC's scale has to build that governance layer itself, on its own terms, rather than inherit one from the regulator.

## Product Name (working)

**"ICB Trust Platform"** — a shared customer data, communications, and content layer for International Consumer Bank markets, with AI governance built in as a first-class capability rather than bolted on per-market.

*(Chosen over the v1 title — sounds like a platform a bank would actually name, and "Trust" signals the governance angle without over-explaining it.)*

## Problem Statement (v2 — single primary problem, one supporting condition)

**Primary problem**: As International Consumer Bank expands into new markets (UK live; Germany targeted 2026) on a fully digital, branchless model, each market's launch team faces the same build: stand up customer data, messaging, and content workflows from scratch, under local privacy law, on a compressed timeline — with no shared platform to inherit integration patterns or governance controls from.

**Supporting condition that raises the stakes now**: As of April 2026, the Fed's SR 26-2 guidance (superseding SR 11-7) explicitly excludes generative and agentic AI from formal model-risk scope for banks over $30B in assets — a threshold JPMC clears many times over. That means any AI capability a market team wants to add to these platforms (e.g., personalized messaging) has no standardized federal governance model to plug into. Each market team would otherwise have to invent its own AI risk controls, which doesn't scale and creates inconsistent customer protection across markets.

**One sentence version**: *ICB's multi-market digital expansion needs a shared, governed platform for customer data/comms/content — and the timing is forcing the issue, because the regulator just left AI governance for exactly this kind of capability undefined.*

## In Scope / Out of Scope (new in v2)

**In scope**: proposing a platform layer (governance model + integration pattern + success metrics) that new ICB markets could adopt for customer data, communications, and content workflows, with AI governance as a built-in capability.

**Explicitly out of scope**: this PoC does not claim to know Chase's actual CDP/CMS vendor stack, does not propose specific vendor selections, and does not assume ICB currently lacks any governance — it proposes what a *shared, AI-ready* version of that governance could look like, positioned as additive to (not a replacement for) LLM Suite and JPMC's existing AI Research program.

## User Personas (v2 — three, not two)

1. **Internal platform user** — a product/engineering team inside a new ICB market (e.g., Germany launch team) needing to stand up customer data/messaging/content capability fast, without rebuilding integration patterns per market.
2. **Risk & Compliance stakeholder** *(added in v2)* — the function responsible for signing off on any AI-adjacent customer-facing feature, who currently has no standardized federal framework to point to for gen-AI/agentic systems and needs a repeatable, auditable review pathway instead of a bespoke one per market/feature.
3. **End customer** — an ICB digital banking customer who receives more relevant, timely communications, with data handled consistently regardless of market, under GLBA and applicable state/local privacy law.

*(Adding the Risk/Compliance persona directly strengthens the thesis — the platform's core value prop is a governance capability, so the persona list should reflect that, not just engineering + end customer.)*

## Jobs to Be Done (v2 — tightened, each now maps to a Week 2 metric)

| JTBD | Will map to metric in Week 2 |
|---|---|
| When a new market launches, the platform team needs a reusable integration pattern for customer data/comms/CMS, so they don't rebuild it from scratch. | Time-to-integrate a new market |
| When a product team wants to add an AI-driven feature to a digital channel, they need a pre-approved governance pathway, so they're not the first to invent gen-AI risk controls. | Time-to-approval for AI-adjacent features |
| When compliance/risk reviews a new AI feature, they need consistent audit trails and controls across markets, not a bespoke review per country. | Audit-trail completeness / review consistency rate |
| When a customer receives a communication or personalized experience, they need it to reflect consistent data-handling standards regardless of market. | Consent/compliance rate across markets |

## Why This Scope — Reviewer-Facing Rationale

- Ties directly to the JD's actual team (Digital, ICB, customer data/comms/martech/CMS), not a generic AI pitch.
- Anchors on one clean, dated, primary-sourced regulatory hook (SR 26-2) rather than vague "AI governance is important" framing.
- Treats LLM Suite as precedent to extend, not compete with — shows awareness of what JPMC has already built.
- The in/out-of-scope section pre-empts the most obvious reviewer objection ("how would you know what we run internally?") before it's asked.

## Changes from v1 → v2

1. Collapsed two loosely-linked problems into one primary problem + one supporting condition, with a one-sentence distillation.
2. Added explicit in/out-of-scope section — removes any appearance of overclaiming insider knowledge.
3. Added a third persona (Risk & Compliance) — makes the governance thesis structurally central, not an afterthought.
4. Renamed from a generic descriptive title to a plausible internal product name ("ICB Trust Platform").
5. Pre-mapped each JTBD to a metric category, setting up Week 2's success-metrics section cleanly.
