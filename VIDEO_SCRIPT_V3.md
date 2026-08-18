# 5–6 Minute Video Script (v3) — JPMC Trust Platform
**For: Platform Product Manager, VP — International Consumer Bank Digital (Job ID 210778576)**

New file — `VIDEO_SCRIPT.md` and `VIDEO_SCRIPT_V2.md` are untouched. This version keeps every honesty fix from v2 (hedged MRGR claim, latency framed as a target not a measured fact, UK/Germany framed as deliberately open, HHH named explicitly) and goes deeper everywhere v2 had to compress for time: the three-tier logic, the actual architecture decisions, real findings from the review panel, and the full metrics taxonomy with the reasoning behind each one.

---

## Why longer, and what changed structurally

v2 was tight enough to lose the reasoning behind the design, not just the summary of it. A VP-level reviewer is evaluating judgment, not just output — judgment only shows up when you explain *why* a decision was made, not just *what* it was. Five new segments earn their runtime:

1. **The three-tier logic**, spelled out (v2 only implied it existed).
2. **Two of the three architecture decisions**, explained with the trade-off each one resolves — this is where "technical fluency... system architecture, non-functional scalability requirements" actually gets demonstrated, not just claimed.
3. **The review panel's actual findings** — two concrete before/after examples, not just "a review caught something."
4. **The full metrics taxonomy**, with what failure mode each one exists to catch — this is the single most differentiated section of the whole platform and deserved more than one dense sentence.
5. **A direct, personal close** — the technical AI-PM background isn't stated anywhere in v1/v2; it's the reason this person reached for an HHH-style eval framework instead of a generic KPI list, and that's worth saying once, plainly.

---

## Script

**829 spoken words (verified by count, not estimate) → 5:45–6:20 at a deliberate pace, before screen-cut dwell time. If you deliver faster (~150 wpm) it lands closer to 5:30.**

---

### [0:00–0:25] Open — 55 words
*[SCREEN: your face, webcam]*

> Hi, I'm Pranav Gupta. I work in AI product, and a lot of that work is evaluation — deciding whether an AI system's output can actually be trusted, not just whether it works. So when I looked at this role, I went looking for a place where that specific skill was missing at JPMorgan Chase. I found one.

---

### [0:25–1:05] The regulatory gap — 95 words
*[SCREEN: cut to the case study hero, then Executive Summary]*

> In April 2026, the Federal Reserve issued SR 26-2, superseding SR 11-7. It explicitly excludes generative and agentic AI from formal bank model-risk rules — for any bank over thirty billion dollars in assets, firmwide, not scoped to one line of business. JPMorgan Chase already runs the function built to close exactly that kind of gap: Model Risk Governance and Review, MRGR. Their own most recent 10-K names MRGR as the reviewer for approval-gated models — then discusses generative and agentic AI as a separate risk factor, with no governance owner named for it anywhere.

---

### [1:05–1:40] The proposal — 65 words
*[SCREEN: Solution Overview]*

> Read together, those two facts point the same direction from opposite sides: the expertise to govern this almost certainly exists inside the firm. The entry point doesn't. So I designed what I'd actually propose in this seat — not a new governance function competing with MRGR, but the missing technical layer that extends their existing authority into the one category their scope doesn't appear to reach yet.

---

### [1:40–2:20] The three tiers, explained — 80 words
*[SCREEN: Requirements / AI Behavior Contract]*

> The platform classifies every gen-AI feature request into one of three tiers. Tier one — no customer data touched — clears an automated policy check, ships immediately. Tier two — uses customer data to personalize something, but doesn't touch a credit decision — gets the same automated check, plus a logged human spot-check on a fixed cadence. Tier three — anything touching a credit or account decision — gets no automated path at all. It routes straight into MRGR's real review queue, full human sign-off required, every time.

---

### [2:20–3:00] Architecture — two decisions that matter — 90 words
*[SCREEN: System Architecture diagram]*

> Two design decisions I'd stand behind in a review. First: everything in this system is synchronous except one thing — the audit trail write. An engineer never waits on a human for tier one or two, targeting under two seconds at the fiftieth percentile. The one place it deliberately breaks that pattern is logging, because a delayed audit entry is a much smaller risk than a delayed response. Second: market teams integrate against this platform's own versioned API, never against MRGR's internal systems directly — so if MRGR's tooling changes, that change is invisible to everyone already integrated.

---

### [3:00–3:35] Live console — 70 words
*[SCREEN: console — Classify a Feature tab, submit one]*

> This is a working prototype, not a mockup. Submitting a feature isn't a text box — it's a structured form with the same fields the API contract actually requires: description, which data sources it touches, whether it's customer-facing, whether it influences a credit decision. The tier is a deterministic function of those fields. If I can't say which field drove the outcome, the system hasn't actually explained anything — it's just guessed convincingly.

---

### [3:35–4:15] The reviewer's view — 70 words
*[SCREEN: MRGR Queue tab, expand one row]*

> And this is what an MRGR reviewer actually sees for a tier-three escalation — the classifier's full reasoning, the exact data inputs it used, and the complete audit trail, attached before a human ever opens it. This surface didn't exist in my first draft. The adoption thesis assumed MRGR would just use the routing. A structured review caught that "a consistent review process" is meaningless without something a reviewer can actually sit down and use.

---

### [4:15–5:00] What a real review changed — 85 words
*[SCREEN: Risks & Mitigations]*

> That review — seven perspectives, run against my first full draft — found two things worth naming. It flagged that my primary adoption metric could be gamed by one shallow test call, so I added a ninety-day sustained-use metric alongside it, tracked from day one. And it flagged that my first draft bundled this governance layer with a full customer-data platform — betting the largest investment on the single least-validated assumption in the whole thesis, whether MRGR even wants this. I cut it. Phase one ships governance alone.

---

### [5:00–5:35] Metrics that aren't decorative — 75 words
*[SCREEN: Metrics tab]*

> Every metric on this page maps to a specific failure mode, not a vanity number. Latency, checked against the same budget from the architecture. Escalation signal-to-noise, separate from MRGR simply engaging — because too many over-triggered escalations cause alert fatigue even if MRGR is technically using the routing. And a quality score built on Honesty, Harmlessness, and Helpfulness — the same framework used industry-wide to evaluate LLM output — sampled straight from the human review that already exists, not a new pipeline.

---

### [5:35–5:55] What I left open — 45 words
*[SCREEN: Open Questions]*

> I also left real things unresolved on purpose. Whether MRGR's true scope already covers this. Whether UK-first or Germany-parallel sequencing is right. I'd rather hand you two sharp open questions than a confident answer to something I can't actually verify from outside the firm.

---

### [5:55–6:15] Close — 55 words
*[SCREEN: back to your face]*

> That's the muscle I'd bring to this role — read the regulation, find what the firm already has, design the smallest thing that closes the real gap, and evaluate it with the same rigor I'd apply to any AI system's output. Full write-up and the live console are linked below. I'd welcome ten minutes to walk through the rest. Thank you.

---

## Delivery notes

- **This runs long on purpose.** If you need to cut for time, cut from [2:20–3:00] (architecture decisions) or [5:35–5:55] (open questions) first — both are strong but the least load-bearing if you're tight. Don't cut [4:15–5:00] (the review panel) or [5:00–5:35] (metrics) — those two are what separate this from a generic "I built an AI demo" pitch.
- **Slow down noticeably at 4:15–5:35.** This is the most differentiated minute of the whole video — the review process and the metrics design. Everything before it is table-setting.
- Let the screen sit on the architecture diagram and the MRGR Queue detail row for 2–3 seconds before you speak over them — that's real runtime, not dead air to trim.
- The line "if I can't say which field drove the outcome, the system hasn't actually explained anything" is worth landing deliberately — it's the clearest single sentence of your actual point of view in the whole script.
- End with `planetcodify.github.io/JPMC` as a text overlay for the last 5 seconds.
