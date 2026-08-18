# 3-Minute Video Script — JPMC Trust Platform
**For: Platform Product Manager, VP — International Consumer Bank Digital (Job ID 210778576)**

---

## How this maps to the JD (why each beat is there)

| Script beat | JD requirement it demonstrates |
|---|---|
| SR 26-2 / MRGR discovery | "Proactively monitor trends to propose new solutions" |
| Tiering layer, not new governance | "Define the long-term vision, roadmap, and strategic goals for internal platforms" |
| Architecture + API + latency budget | "Technical fluency… designing journeys, integration patterns, APIs, system architecture" |
| MRGR treated like a vendor you don't control | "Collaborate with vendors and manage vendor relationships" (translated — MRGR is an internal partner with its own roadmap, not a compliance department you can mandate) |
| Live console, structured intake | "Track record of introducing AI tools into product/engineering environments" (preferred) |
| HHH / latency / signal-to-noise metrics | "Define product and platform success metrics, analyze user engagement" |
| MRGR Queue reviewer view | "Translate [stakeholder requirements] into detailed requirements for engineers" |
| 7-perspective review, cut the bundle | "Commercial acumen: data-driven decisions… building cases for cost optimization" |
| UK-before-Germany sequencing | "Proven ability to align diverse business and technical teams across regions" |
| Regulatory grounding throughout | "Meets all regulatory, risk, and control requirements" |

The one JD line this *doesn't* claim: "experience in retail financial services." Don't oversell it — the video is framed as *how I'd operate in this seat*, not *I've done this exact job before*. That honesty is itself evidence of the judgment the JD is testing for.

---

## Script

**Target: ~420 spoken words → 3:00–3:10 at a deliberate, confident pace (not rushed — pauses at screen-share cuts are part of the runtime, don't try to fill them with words).**

---

### [0:00–0:15] Open — 35 words
*[SCREEN: your face, webcam, not the site yet]*

> Hi, I'm Pranav Gupta. This is three minutes on something I built specifically for this role — not a portfolio piece. A working proof of concept for a real, dated gap I found in how JPMorgan governs generative AI.

---

### [0:15–0:40] The discovery — 55 words
*[SCREEN: cut to the case study hero, then scroll to Executive Summary]*

> In April 2026, the Fed's SR 26-2 excluded generative and agentic AI from formal bank model-risk rules — firmwide, not scoped to one line of business. JPMorgan already has the function built to close that: Model Risk Governance and Review. But the one public MRGR posting I found, scoped to International Consumer Bank, only lists traditional statistical-model skills. No gen-AI anywhere in it.

---

### [0:40–1:05] The solution — 55 words
*[SCREEN: Solution Overview section]*

> So I designed what I'd actually propose in this seat — not a new governance function, but the missing technical layer that extends MRGR's existing authority into gen-AI. A tiering platform that classifies every feature request, auto-clears the low-risk ones, and routes anything touching a credit decision straight into MRGR's real queue, reasoning and audit trail already attached.

---

### [1:05–1:35] Architecture + how I treated MRGR — 60 words
*[SCREEN: System Architecture diagram]*

> This is the system boundary — what gets built versus what already exists at the firm, kept deliberately separate. A versioned API a market engineer calls once. A classifier answering in under two seconds at P50. And I treated MRGR the way I'd treat any vendor relationship in this role: I don't own their roadmap, so Phase 1 is built to validate with them directly, not assume they'll say yes.

---

### [1:35–2:05] Live console + metrics — 60 words
*[SCREEN: switch to the live console — Classify a Feature tab, submit one, then cut to Metrics tab]*

> This is a working prototype, not a mockup. A feature gets classified deterministically off four typed fields, not keyword guessing. And because this touches customer data, I tracked more than adoption — latency against SLA, escalation signal-to-noise so MRGR doesn't get alert fatigue, and an AI-quality score sampled straight from the review process that already exists.

---

### [2:05–2:25] The reviewer's view — 35 words
*[SCREEN: MRGR Queue tab, expand one row]*

> This is what a reviewer actually sees — the classifier's reasoning and the full audit trail, attached before a human ever opens it. That surface didn't exist in my first draft. It got added because a review caught that it was missing.

---

### [2:25–2:45] Rigor + regional sequencing — 45 words
*[SCREEN: Risks & Mitigations, then Rollout Plan]*

> That same review flagged a bigger problem: my first draft bundled this with a full customer-data platform, betting the biggest investment on the least-validated assumption. I cut it. Phase 1 ships governance alone, in the UK first — Germany waits.

---

### [2:45–3:05] Close — 50 words
*[SCREEN: back to your face]*

> That's the muscle I'd bring to this role — read the regulation, find what the firm already has, build the smallest thing that closes the real gap, and stay honest about what's still unproven. Full write-up and the live console are linked below. I'd welcome ten minutes to walk through the rest. Thank you.

---

## Delivery notes

- **Don't narrate every word on screen** — the viewer can read the architecture diagram; your job is to say the *one thing the diagram doesn't say by itself* (e.g., "kept deliberately separate," not "here's a box for the API").
- **Slow down at 1:05–2:05** — this is the differentiated middle third (working prototype + real metric design). It's the part a generic candidate can't fake. Don't rush it to save time elsewhere.
- **Record the screen-share pass separately from voice if easier** — narrate to a stopwatch first without screen to nail timing, then re-record with the cursor following your own script.
- **Cut "not a portfolio piece" if it reads as defensive on a re-watch** — it's there to preempt the obvious skepticism ("is this just a generic AI demo"), but say it once, don't return to it.
- **End with the link visible on screen**, not just spoken — `planetcodify.github.io/JPMC` in text overlay for the last 5 seconds, since a hiring manager watching on mute or skimming will look for it.
