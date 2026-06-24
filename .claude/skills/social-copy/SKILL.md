# Skill: social-copy

## Description
Produces platform-specific social media copy — captions, hooks, carousel slide text, story copy, and CTAs — aligned with the practice's brand voice, campaign brief, and content pillar. All output requires a compliance-review pass before leaving the project.

## When to Use
- When executing a monthly content calendar
- When a campaign brief has been approved and content production begins
- When a one-off post is needed (offer, announcement, event)
- When repurposing a piece of content for a different platform or format
- When A/B testing caption variants

Do not run this skill before the campaign brief is approved. Do not send output to the client before compliance-review passes.

## Required Inputs
- Approved campaign brief from `_templates/campaign-brief.md`
- `_context/clients/{client}/brand-voice-guide.md`
- `_context/clients/{client}/brand-context.md`
- `_context/clients/{client}/approved-claims.md`
- `_context/clients/{client}/banned-claims.md`
- `_context/global/compliance-rules.md`
- `_context/global/content-pillars.md`
- `_context/global/platform-rules.md`
- Trend report (if trend-informed content): `reports/weekly-trend-reports/`
- Social post template: `_templates/social-post-template.md`

## Copy Principles

### Hook first
Every caption must lead with a hook in the first 1–2 lines. The hook is the only thing visible before "more" is tapped.

**Hook types that work for dental:**
- Question: "What's the real reason your teeth feel sensitive?"
- Bold statement: "Most people brush wrong. Here's what to do instead."
- Surprising fact: "Your toothbrush is 10 times dirtier than your toilet seat after 3 months."
- Relatable scenario: "You know that feeling when you smile in a photo and immediately regret it?"
- Local angle: "Miami summers are tough on your teeth — here's why."

Avoid hooks that start with "We" or the practice name. Hooks should be about the reader.

### Pillar tone guide

| Pillar | Tone | What to avoid |
|---|---|---|
| Education | Informative, jargon-free, treats reader as intelligent | Lecturing, alarming, oversimplifying |
| Trust & Social Proof | Warm, proud, human | Bragging, vague platitudes ("we care about you") |
| Service Demand | Clear, benefit-forward, specific | Pushy, guaranteed outcomes, clinical jargon |
| Local Community | Proud, rooted, genuine | Performative, tokenizing |
| Team Personality | Real, warm, slightly playful | Unprofessional, off-brand humor |
| Promotional | Direct, urgent (genuine), friction-removing | Fake scarcity, fine-print burying |

### Platform-specific copy rules

**Instagram:**
- Hook in first 125 characters
- 150–300 words optimal for feed posts; carousels can be shorter per slide
- 3–10 hashtags at end or in first comment
- One CTA only

**Facebook:**
- Longer-form performs with 35–65 demographic
- No hashtag stuffing — 1–3 max
- CTA can be more direct: "Call us at [number]" or "Click 'Book Now'"
- Emojis optional; use sparingly for 45+ audience

**TikTok:**
- Caption is secondary to the video; keep under 150 characters
- 3–5 hashtags; mix trending and dental-specific
- Hook must be on-screen in the video — caption supports, not leads

**Google Business Profile:**
- 300–1,500 characters; keep to 300 for "What's new" posts
- Include location keyword naturally: "serving Coral Gables patients since 2012"
- One clear CTA with phone number or booking URL

**YouTube Shorts:**
- Caption = title + description; optimize for search
- Include service keyword + city: "dental implants Boca Raton — what to expect"
- Description should include practice name, phone, and booking link

### Bilingual copy rules
- Write English first; adapt to Spanish (do not translate literally)
- Spanish copy must read naturally for a mixed Latin American audience
- Mark Spanish captions for native speaker review before publishing
- For practices in Hialeah or Doral: Spanish may be the primary language

## Process

### Step 1 — Load context
Read brand-voice-guide, approved-claims, banned-claims, and compliance-rules before writing a single word.

### Step 2 — Identify the post specs
From the content calendar entry or campaign brief:
- Platform
- Content type (photo, carousel, Reel, Story, GBP)
- Content pillar
- Service or topic
- Working hook or topic angle

### Step 3 — Write 2–3 hook variants
Write at least 2 hook variants before choosing one. Test against:
- Is this about the reader, not the practice?
- Does it create curiosity, relief, or recognition?
- Does it work without context (cold scroll)?

### Step 4 — Write the full caption
Structure:
1. Hook (1–2 lines)
2. Body — value, story, or information (3–8 lines)
3. CTA (1 line)
4. Hashtags (platform-appropriate count)

For carousels: write slide-by-slide. Each slide has one idea. Last slide = CTA.
For Stories: write short punchy text (under 30 words per frame). Include CTA frame.

### Step 5 — Compliance pre-check
Before submitting for formal compliance review, self-check:
- [ ] No patient names, initials, or identifying details
- [ ] No guaranteed results or outcome promises
- [ ] No clinical diagnosis language
- [ ] No invented statistics, prices, or credentials
- [ ] All claims present in approved-claims.md
- [ ] No banned phrases from banned-claims.md
- [ ] No shame-based or fear-based language

Flag any item that fails. Do not remove the flag to "clean it up" — escalate to Compliance Reviewer.

### Step 6 — Write output files
Use `_templates/social-post-template.md` for each post.
Save to `social/captions/YYYY-MM-DD_{client}_{topic}_{platform}_v1.md`.

For carousels: save to `social/carousels/`.
For scripts (Reels, TikTok, Shorts): use `_templates/reel-script-template.md` and save to `social/scripts/`.
For Stories: save to `social/stories/`.

### Step 7 — Submit for compliance review
Hand off to compliance-review skill. Do not deliver to client until approved.

## Output Files
- `social/captions/YYYY-MM-DD_{client}_{topic}_{platform}_v1.md`
- `social/carousels/YYYY-MM-DD_{client}_{topic}_carousel_v1.md`
- `social/scripts/YYYY-MM-DD_{client}_{topic}_script_v1.md`
- `social/stories/YYYY-MM-DD_{client}_{topic}_story_v1.md`

## Quality Checklist
- [ ] Brand voice guide loaded before writing
- [ ] Hook leads every caption — not the practice name
- [ ] 2–3 hook variants written before selecting one
- [ ] Copy matches the content pillar's tone guide
- [ ] Platform-specific word count and format followed
- [ ] All claims cross-checked against approved-claims.md
- [ ] No banned phrases used
- [ ] Compliance pre-check completed — no self-cleared flags
- [ ] Output filed using correct filename convention
- [ ] Submitted to compliance-review before client delivery

## Failure Modes
- **No brand-voice-guide exists** — run `client-context-ingestion` first; do not invent a voice
- **Claim needed but not in approved-claims.md** — write the copy without the claim; flag the gap for client to verify; do not invent
- **Topic requires clinical explanation beyond your knowledge** — write a simple patient-friendly version and flag for dentist review
- **Bilingual copy needed but no native speaker available** — write English; flag Spanish as "needs native speaker review" before publishing
- **Trend hook feels forced for this practice** — do not use the trend; a bad trend fit is worse than no trend

## Dependencies
- `compliance-review` skill (mandatory post-production)
- `_context/clients/{client}/brand-voice-guide.md`
- `_context/clients/{client}/approved-claims.md`
- `_context/clients/{client}/banned-claims.md`
- `_context/global/compliance-rules.md`
- `_context/global/content-pillars.md`
- `_context/global/platform-rules.md`
- `_templates/social-post-template.md`
- `_templates/reel-script-template.md`
