# Skill: brand-voice-normalizer

## Description
Takes the first-pass `brand-voice-guide.md` (Draft) produced by `client-context-ingestion` and deepens it into a production-ready voice system. Analyzes all available brand materials, identifies what the practice actually sounds like vs. what would best serve their audience, and reconciles the two into a finalized voice guide that every downstream skill and agent reads before writing a single word.

## When to Use
- After `client-context-ingestion` completes and before any content production begins
- When a practice rebrands or significantly shifts tone
- When content is consistently coming back off-brand and the root cause is an incomplete voice guide
- When the `brand-voice-guide.md` still has the `Draft` marker from client onboarding

Run this skill after `client-context-ingestion` and before `social-copy`, `short-form-video-script`, `campaign-brief`, or any other content-producing skill.

## Required Inputs

| Input | Priority | Source |
|---|---|---|
| `_context/clients/{client-slug}/brand-context.md` | Required | client-context-ingestion output |
| `_context/clients/{client-slug}/brand-voice-guide.md` (Draft) | Required | client-context-ingestion output |
| `_context/clients/{client-slug}/doctor-bios.md` | Required | client-context-ingestion output |
| `_context/clients/{client-slug}/brand-style-guide.md` | Preferred | client-context-ingestion output |
| `data/raw/firecrawl/{client-slug}/website-crawl-*.md` | Required | Raw website copy |
| Sample social posts (5–20 posts) | Preferred | Provided by client or scraped publicly |
| Any other brand copy (emails, brochures, flyers) | Optional | Provided by client |
| `_context/global/content-pillars.md` | Required | Global context |
| `_context/global/platform-rules.md` | Required | Global context |
| `_context/global/southeast-florida-market.md` | Required | Regional context |

Before starting, check `brand-context.md` for: `bilingual: true`. If set, Spanish voice profile is required in Step 7.

## Process

### Step 1 — Gather all voice samples
Collect every piece of existing written content from or about the practice:

**From the website crawl:**
- Homepage tagline and hero copy
- "About Us" / "Our Philosophy" page copy
- Service page descriptions (how they describe treatments)
- Any patient-facing instructions or FAQ language
- Any existing blog posts

**From provided social posts (if available):**
- Caption openings (hooks)
- CTA phrasing
- Hashtag choices
- Emoji usage patterns
- Response to comments (if visible)

**From doctor bios:**
- First-person language (how the doctor describes themselves)
- Stated philosophy of care
- What they emphasize: skill / empathy / technology / community

**From any other materials:**
- Email subject lines and body copy
- Print ad or flyer language
- Reception area signage copy

If samples are limited (fewer than 5 total pieces), note this in the voice guide header as "Limited samples — voice profile is partially inferred from market context." Do not fabricate samples.

### Step 2 — Analyze voice across 5 dental-specific dimensions

Score the practice on each dimension from 1 (left end) to 5 (right end). Use direct evidence from samples for each score.

**Dimension 1 — Warmth spectrum**
```
1 ←——————————————————→ 5
Clinical / Professional        Warm / Personal / Relational
```
Evidence markers for high warmth: "family," "comfortable," "gentle," "we love our patients," first-name references, personal stories.
Evidence markers for low warmth: procedure-first language, credential-heavy copy, minimal personal pronouns.

**Dimension 2 — Communication style**
```
1 ←——————————————————→ 5
Formal / Traditional           Conversational / Casual / Friendly
```
Evidence markers for conversational: contractions, short sentences, colloquial phrases, humor.
Evidence markers for formal: no contractions, complex sentence structure, passive voice, titles used consistently.

**Dimension 3 — Primary value emphasis**
```
1 ←——————————————————→ 5
Expertise / Authority          Empathy / Patient Experience
```
Evidence markers for expertise: degrees, certifications, technology, years of experience, awards.
Evidence markers for empathy: "we understand," "anxiety-free," "your comfort," patient feelings language.

**Dimension 4 — Content orientation**
```
1 ←——————————————————→ 5
Promotional / Offer-first      Educational / Value-first
```
Evidence markers for promotional: prices, deals, "call now," urgency language.
Evidence markers for educational: "did you know," explanatory language, "here's what to expect."

**Dimension 5 — Personality register**
```
1 ←——————————————————→ 5
Reserved / Neutral             Distinctive / Personality-forward
```
Evidence markers for distinctive: humor, specific cultural references, strong opinions, memorable phrasing.
Evidence markers for reserved: generic language, no strong opinions, could belong to any dental practice.

For each dimension, record:
- Current score based on samples
- Recommended score for their audience and competitive position
- Gap (if any): the difference between current and recommended voice

If current and recommended scores differ by 2+ points on any dimension, flag it as a **Voice Gap** and note the recommended shift.

### Step 3 — Identify vocabulary patterns

From all samples, extract:

**Words and phrases the practice uses repeatedly:**
List actual recurring words and phrases. These are their natural vocabulary.

**Words and phrases to amplify:**
Terms that appear in strong-performing sample content or align with recommended voice.

**Words and phrases to retire:**
Generic dental marketing language that sounds like every other practice:
- "state-of-the-art" (unless they can be specific: "same-day CEREC crowns")
- "cutting-edge technology"
- "your smile is our priority"
- "compassionate care"
- "our team of professionals"
- "comprehensive dental services"

For SE Florida specifically, watch for: over-reliance on English-only phrasing when the practice serves Spanish speakers, formal vocabulary that doesn't land with Miami's conversational culture.

### Step 4 — Build the approved phrase library

Create three categories of approved phrases grounded in the practice's actual voice:

**Opening hooks (how content starts):**
6–10 hook starters in the practice's voice. Examples based on voice dimensions:
- High warmth + conversational: "Here's something we tell every patient who walks in nervous..."
- High expertise + formal: "Understanding your treatment options starts with accurate information."
- High personality: "Real talk — most people have no idea what a root canal actually feels like."

**CTAs (how content ends):**
4–6 approved CTA phrasings that match brand tone.
- Soft CTAs: "Curious? Start with a free consult."
- Direct CTAs: "Call us today at [phone]."
- Low-friction CTAs: "Book in 2 minutes at [URL]."
Specify which CTA style matches the practice's preferred register.

**Transition phrases:**
4–6 natural in-copy transitions: "Here's what that means for you:", "The good news:", "What most people don't realize:", etc.

**Pillar-specific openers:**
One opener style per content pillar (Education, Trust, Service Demand, Local, Team, Promotional) that fits the practice's voice.

### Step 5 — Build the banned phrase list (brand-specific)

Beyond the global compliance bans in `_context/global/compliance-rules.md`, identify brand-specific phrases this practice should never use:

- Phrases the practice has explicitly asked to avoid (from `banned-claims.md`)
- Generic phrases that contradict their positioning (e.g., a high-end cosmetic practice should not use "affordable" as a lead word)
- Competitor-adjacent language that implies comparison
- Any phrase that sounds out of character for this specific practice

For each banned phrase, provide a substitute: "Instead of 'X', use 'Y'."

### Step 6 — Define platform-specific tone adjustments

The core voice stays consistent, but platform register shifts. For each platform the practice is active on, define one adjustment sentence:

**Instagram:** [core voice] + [adjustment] — e.g., "Warm and educational, but hook-first and emoji-light."
**Facebook:** [core voice] + [adjustment] — e.g., "Slightly more formal than Instagram; longer body copy acceptable."
**TikTok:** [core voice] + [adjustment] — e.g., "Most casual register; punchy, trend-aware but never forced."
**YouTube Shorts:** [core voice] + [adjustment] — e.g., "Clear and calm narration; SEO-informed titles."
**GBP:** [core voice] + [adjustment] — e.g., "Professional, local, keyword-aware; no emoji."
**Website/blog:** [core voice] + [adjustment] — e.g., "Most formal register; complete sentences; no hashtags."

Only document platforms the practice is active on. Mark others as "not currently active."

### Step 7 — Spanish voice profile (required if bilingual: true)

If `brand-context.md` contains `bilingual: true`:

**Dialect and register:**
Identify the predominant Spanish-speaking community served (Cuban-American, Colombian, Venezuelan, Puerto Rican, Central American, mixed). In Miami-Dade this is most commonly Cuban-American Spanish; Broward and Palm Beach are more mixed. Dialect affects vocabulary and register.

**Tone in Spanish:**
Spanish dental marketing in SE Florida tends to run warmer and more relational than English equivalents. Formality level: document whether the practice uses "usted" (formal) or "tú" (informal) when addressing patients. Default to "usted" unless the practice explicitly prefers "tú."

**Spanish hook starters:**
3–5 approved opening hooks in Spanish, matched to the practice's voice.

**Spanish CTAs:**
3–4 approved CTAs in Spanish.

**Translation notes:**
Flag any approved English phrases that do not translate naturally. Provide natural Spanish equivalents — never word-for-word translations.

**Avoid:**
Machine-translated copy. All Spanish content should read as if written by a native speaker. If in-house Spanish fluency is not available, flag for external review before publishing.

### Step 8 — Doctor voice notes (multi-doctor practices)

If the practice has multiple doctors, each may carry a slightly different personality in content:

For each doctor, document:
- Preferred tone register (e.g., Dr. A is warm and humorous; Dr. B is calm and authoritative)
- First-person phrase style if they write or appear in their own content
- Content types they are best suited for (Dr. A → Team Personality Reels; Dr. B → Education carousels)

If the practice wants a single unified practice voice regardless of doctor, document that decision here.

### Step 9 — PHI check before writing output

Before writing the finalized guide, verify:
- No patient names, identifying details, or clinical specifics appear anywhere in the voice analysis
- All review and sample quotes are paraphrased, not copied verbatim if they could identify a patient
- Voice examples invented for the guide are clearly fictional (no real patient references)

### Step 10 — Write the finalized brand-voice-guide.md

Overwrite the Draft file at `_context/clients/{client-slug}/brand-voice-guide.md`.

**Required file structure:**

```markdown
# Brand Voice Guide — {Practice Name}
**Status:** Final
**Last Updated:** {YYYY-MM-DD}
**Bilingual:** true / false
**Samples analyzed:** {N} (note if limited)

---

## Voice Dimensions

| Dimension | Current Score | Recommended Score | Voice Gap |
|---|---|---|---|
| Warmth | N/5 | N/5 | None / Shift warmer / Shift colder |
| Communication Style | N/5 | N/5 | ... |
| Value Emphasis | N/5 | N/5 | ... |
| Content Orientation | N/5 | N/5 | ... |
| Personality Register | N/5 | N/5 | ... |

**Voice Gap Notes:** [If any dimension has a gap of 2+, explain the recommended shift and why]

---

## Voice in One Sentence
[A single sentence that captures this practice's ideal voice.
Example: "The trusted neighbor who happens to be a great dentist — warm, direct, and
never talks down to you."]

---

## Vocabulary

### Use
[Bulleted list of words and phrases that belong to this voice]

### Retire
[Bulleted list of generic phrases to avoid, each with a substitute]

---

## Approved Phrase Library

### Hook Starters
[6–10 numbered hook starters]

### CTAs
[4–6 approved CTA phrasings with preferred register noted]

### Transition Phrases
[4–6 natural in-copy transitions]

### Pillar-Specific Openers
[One opener style per active pillar]

---

## Brand-Specific Banned Phrases
[Bulleted list: "Instead of '[phrase]', use '[substitute]'"]

---

## Platform Voice Adjustments
[One adjustment sentence per active platform]

---

## Spanish Voice Profile
[Include only if bilingual: true — dialect, register, usted/tú, hooks, CTAs, translation notes]

---

## Doctor Voice Notes
[Include only if multi-doctor — per-doctor personality and content fit]
```

### Step 11 — Update downstream files if needed

If the voice analysis uncovered brand-specific phrases to avoid that are not yet in `banned-claims.md`, append them there with a note: `[Added by brand-voice-normalizer — {date}]`.

If the practice's `brand-voice-guide.md` had significant Voice Gaps (2+ points on any dimension), add a note to `logs/decisions.md`:
```
## {date} — Voice Gap Identified: {client-name}
Dimension: [name]
Current: N/5 | Recommended: N/5
Action: Content production will use recommended voice. Client to be shown examples for approval on first content review.
```

## Output Files
```
_context/clients/{client-slug}/
  brand-voice-guide.md      ← Draft marker removed; Final status set
  banned-claims.md          ← Updated if new brand-specific bans found

logs/
  decisions.md              ← Updated if Voice Gaps were identified
```

## Quality Checklist
- [ ] All available brand materials reviewed (website, social, bios, other copy)
- [ ] All 5 voice dimensions scored with evidence from actual samples
- [ ] Voice Gaps documented and noted in decisions.md if ≥ 2 points
- [ ] Vocabulary list built from real samples, not invented
- [ ] Approved phrase library has content for all 6 hook starters, CTAs, transitions, and pillar openers
- [ ] Brand-specific banned phrases include substitutes
- [ ] Platform voice adjustments defined for all active platforms
- [ ] Spanish voice profile complete if bilingual: true
- [ ] Doctor voice notes included if multi-doctor
- [ ] PHI check passed — no patient identifiers in guide
- [ ] brand-voice-guide.md Draft marker removed and status set to Final
- [ ] banned-claims.md updated if new brand-specific avoidances found
- [ ] decisions.md updated if Voice Gaps identified

## Failure Modes
- **Fewer than 5 brand samples available** — build recommended voice from market context (patient personas, competitive landscape, SE Florida norms for their service focus); mark guide as "Inferred — limited samples; validate with client on first content review"
- **Contradictory voice across channels** (formal website, very casual social) — document both registers; flag the inconsistency; recommend which register to standardize on and why
- **No Spanish samples despite bilingual: true** — build Spanish voice profile from scratch using SE Florida dialect norms and the English voice as a guide; mark as "Inferred — no Spanish samples provided"
- **Multi-doctor with conflicting personalities** — document both honestly; recommend one unified practice voice with doctor-specific carve-outs for personal content; flag for orchestrator decision

## Dependencies
- `client-context-ingestion` (must run first)
- `_context/global/content-pillars.md`
- `_context/global/platform-rules.md`
- `_context/global/southeast-florida-market.md`

**Feeds into:** `social-copy`, `short-form-video-script`, `campaign-brief`, `lead-magnet`, `lp-builder`, `content-creator` agent, and any other skill or agent that produces written content
