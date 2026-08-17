# JPMC Platform PM (VP) — PoC Workspace

Built for: application to "Platform Product Manager - Vice President," International Consumer Bank Digital team, JPMorgan Chase (Bengaluru, req 210778576, apply-by 14/09/2026).

## Send with the application

| File | What it is |
|---|---|
| **icb-trust-platform-prd.html** ([live link](https://claude.ai/code/artifact/2bf8663a-05d3-4dce-81f8-b1601627d4b3)) | ★★★★★ The PRD, designed to read. Same content as the markdown version below, built as a proper document with a sticky table of contents, persona cards, and priority/risk badges. **This is the one to send or share.** |
| icb-trust-platform-prd-v4-full-template.md ([markdown/artifact link](https://claude.ai/code/artifact/4abc4bc2-8521-485d-9771-22b1da112333)) | Same content, plain markdown. Full VP-level template (Executive Summary, Business Impact, Milestones, Solution Overview, User Stories, GTM, Risks, Timeline, Team & Resources, Open Questions, Assumptions). |
| **06_prototype.html** ([live link](https://claude.ai/code/artifact/25ec9ebe-0c69-4bfa-9e41-19277edd9c5e)) | ★ Clickable product console prototype — architecture, RICE roadmap, live governance-tier simulator, metrics, market status |
| 05_cover_note.md | For your reference — how to present this, draft cover-letter paragraph, interview prep |
| icb-trust-platform-prd-final.md | The same thesis in a lighter, Hypothesis-driven format. Kept as an alternate version, not the primary send anymore. |

## The final PRD's core thesis

*Build an automated tiering layer for gen-AI and agentic features that routes the highest-risk cases into JPMC's existing Model Risk Governance & Review (MRGR) process, instead of standing up a new governance function next to it. MRGR's current public scope for ICB appears to cover traditional statistical models only, the same boundary the Fed's SR 26-2 guidance draws, and nothing public suggests that's changed for gen-AI yet.*

This is a materially different platform than where this started. Early versions proposed a full customer data, communications, and CMS platform with governance as one feature. That thesis went through research, a synthesized user study, a competitive teardown, and a 7-agent review panel, and the panel found the riskiest assumption in the whole thing was betting a big platform build on an unvalidated governance appetite. A direct question ("how much of this is already built at JPMC?") then surfaced a real finding: JPMC already runs dedicated model-risk governance scoped to ICB, just not (as far as public information shows) extended to gen-AI. The final PRD is built around that finding, not around the original pitch.

## Full research and process trail (reference only, don't send)

| File | What it is |
|---|---|
| icb-trust-platform-solution-review-v3.md | The version immediately before final — same thesis, less polished |
| icb-trust-platform-solution-review-v2.md | Built with the real PM-OS toolkit (~/Desktop/PM-OS-AG), passed through its actual 7-agent review panel |
| icb-trust-platform-review-synthesis.md | The 7-perspective review (engineer, designer, executive, legal, UXR, skeptic, customer voice) that reshaped the platform |
| user-research-synthesis-icb-trust-platform-v2.md | Synthesized (not primary) research on three personas, in PM-OS's real theme-cluster format |
| competitor-analysis-icb-trust-platform-v2.md | Build-vs-buy teardown (Adobe, Segment, ValidMind, Databricks, do-nothing) plus peer-bank benchmarks (Citi, Standard Chartered, NatWest, Lloyds), in PM-OS's real SWOT/positioning-map format |
| market-research-icb-trust-platform-2026-08-17.md | CDP, martech, and AI-governance market sizing and trends |
| 01_research_notes.md | Source-of-truth public research, every claim cited. Section 6 has the MRGR/CDAO findings the final PRD is built on |
| 07_final_prd_v3.md, 04_final_strategy_doc_v2.md, 03_platform_strategy_prd_v2.md, 02_problem_scoping_v2.md | Earlier drafts, before the real PM-OS toolkit was found and before the MRGR discovery. Kept to show how the thinking evolved, not to send |
| architecture_diagram.svg, roadmap_timeline.svg | Visuals from an earlier draft, not embedded in the current final PRD |
| Platform Product Manager - ... .pdf | Original job posting |

## How this evolved, briefly

1. Scoped the problem from public sources only, built a strategy doc, tightened it, added a RICE roadmap, fact-checked it.
2. Built a clickable prototype of the platform concept as a product console.
3. Redid the whole thing properly using the actual PM-OS toolkit at `~/Desktop/PM-OS-AG` (its real skills, templates, and 7 sub-agent reviewers) after discovering that's what "PM-OS" actually referred to, not a generic workflow.
4. The review panel found the original bundled platform (data + comms + CMS + governance, all at once) was betting its biggest investment on its least-validated assumption. Rescoped to governance-first.
5. Asked directly whether any of this already existed at JPMC. Found a real Model Risk Governance & Review function scoped to ICB, for traditional models, with no public evidence it covers gen-AI. Rebuilt the platform's whole positioning around extending that function, not replacing it.
6. Consolidated into the final PRD.

## Guardrail this workspace maintains throughout

Every file distinguishes verified public fact from inference or assumption. Nothing here claims to know JPMC's actual internal systems, vendors, or roadmap. Confidence levels are stated explicitly wherever a claim rests on a single source (like the one MRGR job posting the final PRD's core thesis depends on).
