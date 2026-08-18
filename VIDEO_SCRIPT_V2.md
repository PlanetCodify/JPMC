# 3-Minute Video Script (v2) — JPMC Trust Platform
**For: Platform Product Manager, VP — International Consumer Bank Digital (Job ID 210778576)**

This is a rewrite of `VIDEO_SCRIPT.md`, not an edit — that file is left untouched. See the audit table below for what changed and why before the script itself.

---

## Audit: what was wrong with v1, and the fix

| # | v1 line | Risk | Fix in v2 |
|---|---|---|---|
| 1 | "This is three minutes on something I built specifically for this role — **not a portfolio piece**." | Defensive framing before the viewer has judged anything. Pre-empts a skepticism they haven't formed yet. | Cut the disclaimer. Let the specificity of the hook do that work instead. |
| 2 | "The one MRGR posting I found... **No gen-AI anywhere in it.**" | Stated as settled fact. The PRD itself treats this as *unverified* — it's a named Risk and the #1 Open Question ("Does MRGR's actual scope already cover gen-AI, contrary to what one posting suggests?"). A hiring manager who actually knows MRGR's real scope could catch this as overclaimed in the first 20 seconds. | Add the hedge explicitly: "the one posting I could verify" — keeps the hook's punch, matches what the artifact actually claims. |
| 3 | "A classifier **answering** in under two seconds." | States a latency *budget* (a target, never built or measured) as an accomplished fact about a working system. | Reframe as a target: "targeting under two seconds." |
| 4 | "classified... **not keyword guessing**." | References an internal build-history detail (an earlier prototype used keyword matching) the viewer has no context for. Reads as confusing, not clarifying. | Cut. State what the system does now, not what it used to do. |
| 5 | Metrics beat names latency/signal-to-noise/HHH but never says HHH is *the* framework used industry-wide for LLM evaluation. | Leaves the JD's preferred qualifier ("track record of introducing AI tools") as an implicit signal instead of an explicit one — the exact place to be direct about domain expertise, not subtle. | Name it: "the same framework used industry-wide to evaluate LLM output." |
| 6 | "Phase 1 ships governance alone, in the UK first — **Germany waits**." | **Direct factual contradiction with the artifact.** The PRD states, more than once, that UK-vs-Germany sequencing is "deliberately left unresolved... not silently decided." The script asserts the opposite as a settled fact. Anyone who reads the linked PRD after watching this would catch the contradiction immediately — which undermines the one thing the whole pitch is selling: that this person doesn't oversell certainty. | Fix to say the sequencing was *deliberately left open* — which is actually a stronger, more senior point than pretending a decision was made. Knowing what *not* to decide prematurely is itself the skill being demonstrated. |

The throughline across every real issue: v1 oversold certainty at exactly the points where the underlying PRD is deliberately honest about what's unproven. Fixed by matching the video's confidence to the artifact's actual confidence — nowhere more, nowhere less.

---

## Script

**~400 spoken words → 3:00–3:15 at a deliberate pace, screen-cut pauses included in that runtime.**

---

### [0:00–0:20] Open — 55 words
*[SCREEN: your face, webcam]*

> Hi, I'm Pranav Gupta. In April 2026, the Federal Reserve's SR 26-2 excluded generative and agentic AI from formal bank model-risk rules — firmwide, not scoped to one line of business. JPMorgan Chase already runs the function built to close exactly that kind of gap: Model Risk Governance and Review. The one MRGR posting I could verify, scoped to International Consumer Bank, doesn't mention gen-AI once.

---

### [0:20–0:45] The solution — 50 words
*[SCREEN: Solution Overview section]*

> So I designed what I'd actually propose in this seat — not a new governance function, but the missing technical layer that extends MRGR's existing authority into gen-AI. A platform that classifies every feature request, clears the low-risk ones automatically, and routes anything touching a credit decision straight into MRGR's real queue.

---

### [0:45–1:15] Architecture + vendor framing — 60 words
*[SCREEN: System Architecture diagram]*

> Here's the system boundary — what I'd build versus what already exists at the firm, kept deliberately separate. A versioned API a market engineer calls once. A classifier targeting under two seconds at P50, so nobody files a ticket and waits. And I treated MRGR the way I'd treat any vendor in this role: I don't own their roadmap, so this is built to validate with them first, not assume they say yes.

---

### [1:15–1:50] Live console + metrics — 70 words
*[SCREEN: console — Classify a Feature tab, submit one, then cut to Metrics tab]*

> This is a working prototype. A feature gets classified off four structured fields — the same ones the API contract actually requires. And because this touches customer data, I didn't stop at adoption. I tracked latency against that same budget, escalation signal-to-noise so MRGR doesn't get alert fatigue, and a quality score built on Honesty, Harmlessness, and Helpfulness — the same framework used industry-wide to evaluate LLM output.

---

### [1:50–2:10] The reviewer's view — 35 words
*[SCREEN: MRGR Queue tab, expand one row]*

> This is what a reviewer actually sees — the classifier's reasoning and the full audit trail, attached before a human opens it. That surface didn't exist in my first draft. A review caught that it was missing.

---

### [2:10–2:40] Rigor + what I left open — 55 words
*[SCREEN: Risks & Mitigations, then Open Questions]*

> That same review flagged a bigger problem: my first draft bundled this with a full customer-data platform, betting the largest investment on the least-validated assumption. I cut it. Phase 1 ships governance alone — and UK-versus-Germany sequencing, I left as an open question here, not a decision I made for the sake of a clean slide.

---

### [2:40–3:05] Close — 55 words
*[SCREEN: back to your face]*

> That's the muscle I'd bring to this role — read the regulation, find what the firm already has, build the smallest thing that closes the real gap, and stay honest about what's still open. Full write-up and the live console are linked below. I'd welcome ten minutes to walk through the rest. Thank you.

---

## Delivery notes (unchanged guidance, still applies)

- Slow down at 1:15–2:10 — the working prototype and the metrics design are the differentiated middle third. Don't rush it to protect the open/close.
- Let the screen sit on the architecture diagram and the MRGR Queue detail row for 2–3 seconds before speaking over them — that dwell time is part of the 3:00 runtime, not dead air to cut.
- End with `planetcodify.github.io/JPMC` as a text overlay for the last 5 seconds, not just spoken — for anyone watching on mute.
- If you're nervous about the "one posting I could verify" hedge sounding weaker than v1's flat claim — it isn't. A VP hiring manager will trust a claim more, not less, when it's precisely scoped to what you actually know.
