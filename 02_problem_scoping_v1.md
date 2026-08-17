# Problem Scoping — DRAFT v1
### Platform PM PoC for JPMC (International Consumer Bank, Digital)

## Framing Device

This is written as an **externally-informed product point of view**, not a proposal claiming insider access. Every internal-sounding claim is explicitly flagged as assumption/hypothesis, grounded in [01_research_notes.md](01_research_notes.md).

## Working Title

**"A Governed AI Capability Layer for Chase's Consumer Digital Platforms"**

## Problem Statement (draft)

Chase's International Consumer Bank is expanding into new markets (UK live, Germany targeted 2026) on a branchless, fully digital model. Each new market likely needs to stand up its own customer data, communications, and content workflows — repeating integration work market-by-market unless there's a shared platform underneath.

Separately, JPMorgan Chase has built LLM Suite, a firm-wide gen-AI capability layer (230K+ employees, 450+ use cases). But there's no public evidence LLM Suite's capabilities have been extended into the specific internal platforms that power consumer-facing digital channels (customer data, comms, CMS) — and as of April 2026, the Fed's SR 26-2 guidance explicitly leaves generative and agentic AI *outside* formal model-risk scope, creating a governance vacuum right as ICB scales into new, tightly-regulated markets.

**The problem**: as ICB scales into new markets, its digital-channel platforms (customer data, comms, martech, CMS) risk being rebuilt per-market, and any AI capability added to them has no standardized governance model to inherit — because the regulator explicitly declined to define one.

## User Persona (draft)

Two personas, matching the JD's "voice of both customers and internal platform users":

1. **Internal platform user**: a product/engineering team inside a new ICB market (e.g., Germany launch team) who needs to stand up customer data, messaging, and content capabilities fast, without reinventing integration patterns or waiting on a bespoke governance review for every AI-adjacent feature.
2. **End customer**: an ICB digital banking customer who receives more relevant, timely communications and content — but only if their data is handled consistently with GLBA/state privacy obligations regardless of which market they're in.

## Jobs to Be Done (draft)

- When a new market launches, the platform team needs to give them a **reusable integration pattern** for customer data/comms/CMS, so they don't rebuild it from scratch.
- When a product team wants to add an AI-driven feature (e.g., personalized nudges, next-best-action messaging) to a digital channel, they need a **pre-approved governance pathway** so they're not the first team inventing model-risk controls for a gen-AI feature.
- When compliance/risk reviews a new AI feature, they need **consistent audit trails and controls** across markets, not a bespoke review per country.

## Why This Scope (rationale)

- Ties directly to the JD's actual team (Digital, ICB, customer data/comms/martech/CMS) rather than a generic AI pitch.
- Uses the one clean, verifiable regulatory hook (SR 26-2) as the central tension — defensible, dated, primary-sourced.
- Avoids claiming to know Chase's internal architecture — frames the platform gap as a reasonable hypothesis given ICB's public multi-market expansion.
- Leaves room to reference JPMC's own LLM Suite as a firm-wide precedent this proposal would extend, not compete with.
