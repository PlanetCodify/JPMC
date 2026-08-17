# Cover Note — How to Present This PoC

## Delivery mechanism

JPMC's ATS almost certainly won't accept a second attachment beyond resume/cover letter. Two viable paths:

1. **Cover letter reference + private link.** Mention the artifact in 1-2 sentences in your cover letter, with a link to a privately-hosted version (not a public blast — treat it as a leave-behind for whoever actually reads it).
2. **Recruiter/hiring-manager direct share.** If you get a name (LinkedIn, referral, interview scheduling email), send it directly as a follow-up — much higher read-through rate than anything routed through an ATS.

Don't attach it as a second PDF to the formal application unless the portal explicitly allows multiple attachments — an unexplained second file often gets auto-discarded by parsers.

## Suggested cover-letter paragraph (draft — edit to your voice)

> To ground my interest in this role, I put together a short illustrative product strategy document — a proposed shared platform for International Consumer Bank's customer data, communications, and content workflows across markets, with AI governance built in as a response to the Fed's April 2026 SR 26-2 guidance. It's built entirely from public information and flags its own assumptions throughout; I'd welcome the chance to walk through it and hear where it's wrong about how Chase actually operates.

**Why this framing works:** it's confident about the work without claiming insider accuracy, and the closing line ("hear where it's wrong") pre-empts the most likely reviewer reaction — a VP who knows the real internal picture will have specific corrections, and inviting that is more senior than pretending you already know.

## What NOT to say

- Don't call it a "proposal I'd build" — it's a point of view, not a commitment. Overpromising here reads as naive about how a $30B+-asset bank actually approves platform investment.
- Don't claim it reflects "research into Chase's systems" — it reflects public research *about* the regulatory and market context Chase operates in. Keep that distinction sharp if asked about it in an interview.

## If asked about it in an interview

Be ready to:
- Walk through the RICE trade-off (Section 3 of the final doc) — this is the strongest signal of judgment in the whole artifact; know it cold.
- Explain why you chose SR 26-2 as a supporting condition rather than the platform's core justification — this shows you can take feedback on your own thinking (it wasn't the original framing — see 03_platform_strategy_prd_v1.md).
- Have a real answer for "what would you do differently if I told you our actual CDP is [X]" — the honest answer is "the API-versioning pattern in Section 2 is specifically designed so that answer doesn't change the platform's shape, only its vendor integration detail."

## File manifest (see [00_README.md](00_README.md) for full guide)

Send/reference: **04_final_strategy_doc_v2.md** (plus the two SVGs it embeds).
Keep in reserve, don't send unsolicited: 01, 02, 03 — offer them only if someone asks for "the fuller version."
