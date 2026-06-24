# Skill: social-copy

## Description
Produces platform-specific social media copy for dental practices in Southeast Florida — Instagram captions, Facebook posts, TikTok captions, GBP posts, YouTube Shorts titles and descriptions, carousels, and Story frames. Draws hooks from `hook-library.md`, hashtags from `hashtag-sets.md`, and keywords from `keyword-clusters.md`. Every output requires a `compliance-review` pass before leaving the project.

## When to Use
- When executing a monthly content calendar after the campaign brief is approved
- When producing content for a specific campaign brief content idea
- When a one-off post is needed (offer, announcement, event, trend opportunity)
- When repurposing a piece of content for a different platform or format
- When producing A/B caption variants for testing

Do not run this skill before the campaign brief is approved. Do not deliver output to the client before compliance-review passes.

## Required Inputs

| Input | Priority | Source |
|---|---|---|
| Approved campaign brief | Required | `social/calendars/{client-slug}/{date}_{client-slug}_{month}-campaign-brief_v1.md` |
| `_context/clients/{client-slug}/brand-voice-guide.md` | Required | brand-voice-normalizer |
| `_context/clients/{client-slug}/hook-library.md` | Required | social-media-mining |
| `_context/clients/{client-slug}/hashtag-sets.md` | Required | social-media-mining |
| `_context/clients/{client-slug}/approved-claims.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/banned-claims.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/keyword-clusters.md` | Required | keyword-research |
| `_context/clients/{client-slug}/brand-context.md` | Required | client-context-ingestion |
| `_context/global/compliance-rules.md` | Required | Global context |
| `_context/global/content-pillars.md` | Required | Global context |
| `_context/global/platform-rules.md` | Required | Global context |
| `_templates/social-post-template.md` | Required | Template |
| `_templates/reel-script-template.md` | Required for scripts | Template |

## Copy Principles

### Hook first
Every caption must lead with a hook in the first 1–2 lines — the only thing visible before "more" is tapped. Always check `hook-library.md` first. If a matching hook pattern exists for the pillar and service, adapt it. If not, write from scratch using one of the 8 hook types below.

**Hook types (aligned with hook-library.md taxonomy):**

| Type | Pattern | Works best for |
|---|---|---|
| Question | Opens with a direct question to the reader | Education, Service Demand |
| Bold Statement | Opens with a provocative or surprising claim | Education, Trust |
| Surprising Fact | Opens with a counter-intuitive statistic or fact | Education, Promotional |
| Relatable Scenario | Opens with a situation the reader recognizes | Trust, Team Personality |
| Local Angle | Opens with a specific SE Florida reference | Local Community, Team |
| Challenge / Myth-Bust | Opens by calling out a common misconception | Education, Service Demand |
| How-To Tease | Opens with a promise of actionable information | Education, Promotional |
| Emotional / Aspiration | Opens with the desired outcome or feeling | Cosmetic services, Promotional |

**Hook rules:**
- Never start with "We," "Our," or the practice name
- The hook is about the reader — not the practice
- Test: does it work if read cold, with no context? If not, rewrite it

### Pillar tone guide

| Pillar | Tone | What to avoid |
|---|---|---|
| Education | Informative, jargon-free, treats reader as intelligent | Lecturing, alarming, oversimplifying |
| Trust & Social Proof | Warm, proud, human | Bragging, vague platitudes |
| Service Demand | Clear, benefit-forward, specific | Pushy, guaranteed outcomes, clinical jargon |
| Local Community | Proud, rooted, genuine | Performative, tokenizing |
| Team Personality | Real, warm, slightly playful | Unprofessional, off-brand humor |
| Promotional | Direct, urgent (genuine), friction-removing | Fake scarcity, fine-print burying |

### Platform copy specifications

**Instagram (Feed / Reel caption):**
- Hook within first 125 characters (before the "more" fold)
- 150–300 words for feed posts; shorter per slide for carousels
- One CTA only — placed near the end, not in the first line
- Hashtags: pull from `hashtag-sets.md` Instagram sets — 5–10 tags at end or in first comment
- Emojis: follow brand-voice-guide.md preference

**Facebook:**
- Longer-form performs for the 35–65 demographic
- 1–3 hashtags maximum; pull from `hashtag-sets.md` Facebook sets
- CTA can be more direct: "Call us at [number]" or "Click 'Book Now'"
- Emojis: use sparingly for 45+ audience unless brand voice calls for them

**TikTok:**
- Caption is secondary to the video; keep under 150 characters
- 3–5 hashtags; mix trending and dental-specific from `hashtag-sets.md` TikTok sets
- Hook must be on-screen in the video — caption supports, does not lead
- Write the caption as a teaser/extension of the video hook

**Google Business Profile (GBP):**
- "What's new" post: 300 characters max — news, team update, new service
- "Offer" post: include offer text (verified in approved-claims.md), expiration date, and redemption instructions
- "Event" post: include event name, date, time, description, booking link
- Include location keyword naturally in every post: "serving [city] patients since [year]" or "[service] in [city]"
- One clear CTA with phone number or booking URL
- No hashtags on GBP

**YouTube Shorts:**
- **Title (100 characters max):** Include service keyword + city: "Dental Implants in Boca Raton — What to Expect"
- **Description:** Practice name, one-sentence service description, phone number, booking URL, 3–5 relevant keywords
- Title is the primary SEO asset — spend more time here than on description
- Caption is the description field; optimize for Google search indexing

### Bilingual copy rules
Check `brand-voice-guide.md` for the client's language approach before writing. Do not default to English-first — the guide specifies:
- **English-primary:** Write English first; create a Spanish adaptation (not a translation) for bilingual posts
- **Spanish-primary:** Write Spanish first (for Hialeah, Doral, Little Havana clients); English is the adaptation
- **Bilingual-equal:** Write both versions simultaneously; do not derive one from the other

**In all cases:**
- Spanish copy must read naturally for the client's specific community (Cuban-American, Venezuelan, Colombian — see brand-voice-guide.md)
- Use the usted / tú register specified in brand-voice-guide.md — never mix
- Mark Spanish captions with `<!-- Spanish — needs native speaker review before publishing -->`
- Never publish Spanish copy without native-speaker review

### A/B variant production
When producing A/B variants:
- Vary ONE element per pair — never change multiple elements simultaneously
- **Hook A/B:** Same body and CTA; different hook type or phrasing (most common test)
- **CTA A/B:** Same hook and body; different CTA (e.g., phone call vs. online booking)
- **Length A/B:** Same content; short version vs. long version
- Name variants: `_v1a.md` and `_v1b.md` — document what changed in a comment at the top of each file

## Process

### Step 1 — Load context
Read in this order before writing anything:
1. Approved campaign brief — identify the specific content idea being produced (pillar, platform, format, hook angle, keyword tie-in, trend tie-in, language)
2. `brand-voice-guide.md` — tone, vocabulary, approved phrases, CTAs, platform adjustments, Spanish voice
3. `hook-library.md` — check for existing hook patterns matching the pillar and service
4. `hashtag-sets.md` — locate the correct hashtag set for this platform and service
5. `approved-claims.md` — confirm any claims the content idea requires are verified
6. `banned-claims.md` — flag any words or phrases in the idea that must be avoided
7. `keyword-clusters.md` — check for keyword tie-ins relevant to this post (especially for GBP and YouTube Shorts)

### Step 2 — Confirm post specifications
From the campaign brief content idea:
```
Content idea #: [N from campaign brief idea bank]
Platform: [Instagram / Facebook / TikTok / GBP / YouTube Shorts]
Format: [photo caption / carousel / Reel caption / Story / GBP post type / Shorts title+description]
Content pillar: [pillar name]
Service / topic: [specific service from taxonomy]
Hook angle: [from campaign brief — adapt using hook-library.md]
Keyword tie-in: [from campaign brief — use in GBP or Shorts]
Trend tie-in: [from campaign brief — if applicable]
Language: [English / Spanish / Bilingual]
A/B variant: [Yes / No — if Yes, what element is being varied?]
```

### Step 3 — Write 2–3 hook variants
Before writing the full caption, generate at least 2 hook variants and select the strongest.

Check `hook-library.md` first — adapt an existing high-signal pattern rather than inventing from scratch.

**Selection test for each variant:**
- Is this about the reader, not the practice?
- Does it create curiosity, relief, or recognition?
- Does it work read cold, with no prior context?
- Is it under 125 characters for Instagram? (Hard limit — test this)

Document the rejected variant(s) in a comment at the bottom of the file — rejected hooks often work for A/B tests later.

### Step 4 — Write the full copy by format

**Standard caption (Instagram / Facebook / TikTok):**
```
[Hook — 1–2 lines]

[Body — value, story, or information]
[3–8 lines depending on platform]
[Short paragraphs; one idea per paragraph]
[Use line breaks for scannability]

[CTA — 1 line]

[Hashtags — from hashtag-sets.md, platform-appropriate count]
```

**Carousel:**
Structure as a story arc — the reader moves through a logical progression:

```
Slide 1 (Cover): Hook headline only — make them swipe
Slide 2: Context or problem statement
Slide 3–N: One insight, tip, or step per slide (max 30 words per slide)
Second-to-last slide: Key takeaway or summary
Last slide (CTA): One clear action + booking info
```
- 5–8 slides is the sweet spot for dental education carousels
- Each slide should work as a standalone image (reader might screenshot any slide)
- Cover slide headline = the caption hook (they must match in intent)
- Write the caption for the carousel as a whole — it should tease the slides, not repeat them

**Story frames:**
```
Frame 1: Hook — one bold question or statement (under 15 words)
Frame 2–3: Key content points (under 25 words each; one point per frame)
Last frame: CTA with link sticker placeholder [LINK STICKER: booking URL]
```
- Stories live 24 hours — write with urgency
- Use the "tap to next" behavior intentionally — cliffhanger between frames works
- Maximum 4–5 frames; readers drop off after frame 3

**GBP post (What's new):**
```
[Opening hook — local or service-specific]
[1–2 sentences of value or context]
[Location keyword integrated naturally]
[CTA with phone number or booking URL]
```
Keep to 300 characters for "What's new" type.

**GBP post (Offer):**
```
[Offer headline — clear and specific]
[Offer details: what, how much, what's included]
[Expiration date]
[Eligibility or terms (if any)]
[Redemption CTA: "Call [phone] or book at [URL]"]
```
Must verify offer exists verbatim in `approved-claims.md` before writing.

**YouTube Shorts title + description:**
```
TITLE: [Primary keyword] in [City] — [Patient benefit in 4-6 words]
(Max 100 characters; keyword first)

DESCRIPTION:
[Practice name] | [Service one-liner]
📞 [Phone number]
🗓️ Book online: [booking URL]

[2–3 natural keyword sentences describing the video content]
[City + service keyword integration]
```

### Step 5 — Apply hashtags from hashtag-sets.md
Do not invent hashtags. Pull directly from `hashtag-sets.md` for the correct:
- Platform (Instagram / TikTok / Facebook)
- Service or topic (Implants / Whitening / General / etc.)
- Language (English / Spanish)

Select Tier 1 tags always. Add Tier 2 tags when service matches. Add Tier 3 tags sparingly to avoid diluting specificity.

For GBP posts: no hashtags.
For YouTube Shorts: keywords in description, not hashtags.

### Step 6 — Integrate keyword (GBP and YouTube Shorts only)
For GBP posts and YouTube Shorts titles/descriptions, pull the relevant keyword from `keyword-clusters.md`:
- Use the exact patient-phrased version (e.g., "dental implants Miami" not "osseointegrated implants")
- Integrate naturally — do not keyword-stuff
- For GBP: keyword should appear once in the post body
- For YouTube Shorts: keyword in the title and once in the description

For other platforms (Instagram, Facebook, TikTok): keywords inform the topic and vocabulary but are not forced into copy.

### Step 7 — Compliance pre-check
Before submitting for formal compliance review, self-check every piece of copy:

- [ ] No patient names, initials, or any identifying details
- [ ] No guaranteed results or outcome promises ("will", "guaranteed", "100%", "always works")
- [ ] No clinical diagnosis language directed at the reader
- [ ] No invented statistics, prices, awards, or credentials
- [ ] All factual claims appear in `approved-claims.md`
- [ ] No phrases from `banned-claims.md`
- [ ] No shame-based or fear-based language
- [ ] No before/after patient references without documented authorization
- [ ] Spanish copy marked for native-speaker review (if applicable)

If any check fails: flag it with `<!-- COMPLIANCE FLAG: [issue] -->` in the output file. Do not remove the flag or "fix it quietly" — the compliance reviewer must see it. Escalate to `compliance-review` skill.

### Step 8 — Write output files
Use `_templates/social-post-template.md` as the base for individual posts.

Save to the correct subfolder based on format:

| Format | Save path |
|---|---|
| Instagram / Facebook caption | `social/captions/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_{platform}_v1.md` |
| Carousel copy | `social/carousels/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_carousel_v1.md` |
| Reel / TikTok / Shorts script | `social/scripts/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_script_v1.md` |
| Story frames | `social/stories/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_story_v1.md` |
| GBP post | `social/captions/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_gbp_v1.md` |
| YouTube Shorts title + description | `social/scripts/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_yt-shorts_v1.md` |
| A/B variant A | `..._v1a.md` |
| A/B variant B | `..._v1b.md` |

Every output file ends with: `**Status:** Needs review`

Include at the top of every file:
```
<!-- Campaign brief idea #: [N] | Pillar: [pillar] | Platform: [platform] | Service: [service] -->
<!-- Compliance flags: [None / list any flags] -->
```

### Step 9 — Submit for compliance review
Hand off all output files to the `compliance-review` skill.
Do not move files to `social/approvals/` until compliance-review returns Approve or Approve with Edits.
Do not deliver any copy to the client before approval.

## Output Files
```
social/captions/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_{platform}_v1.md
  {YYYY-MM-DD}_{client-slug}_{topic}_gbp_v1.md

social/carousels/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_carousel_v1.md

social/scripts/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_script_v1.md
  {YYYY-MM-DD}_{client-slug}_{topic}_yt-shorts_v1.md

social/stories/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_story_v1.md
```

## Quality Checklist
- [ ] Approved campaign brief loaded — specific content idea identified by number
- [ ] `brand-voice-guide.md` loaded before writing any copy
- [ ] `hook-library.md` checked before writing hooks — existing patterns adapted where available
- [ ] `hashtag-sets.md` consulted — hashtags pulled from library, not invented
- [ ] `keyword-clusters.md` checked for GBP and YouTube Shorts keyword integration
- [ ] 2–3 hook variants written; strongest selected; rejected variants saved in comment
- [ ] Hook is about the reader, not the practice; does not start with "We" or practice name
- [ ] Copy matches the content pillar's tone guide
- [ ] Platform character limits and format specs followed
- [ ] Carousel: 5–8 slides, cover → context → content → CTA arc
- [ ] Story: max 4–5 frames, last frame has link sticker placeholder
- [ ] GBP: location keyword integrated; correct post type structure used
- [ ] YouTube Shorts: keyword in title (first), description includes phone + booking URL
- [ ] Hashtags applied from `hashtag-sets.md` correct tier for this platform and service
- [ ] All claims cross-checked against `approved-claims.md`
- [ ] No banned phrases from `banned-claims.md`
- [ ] Spanish copy marked for native-speaker review (if applicable)
- [ ] Compliance pre-check completed — flags documented, not self-cleared
- [ ] Campaign brief idea number and compliance flag status in file header comment
- [ ] Output saved to correct subfolder with correct filename convention using `{client-slug}`
- [ ] Status line at end of every file: `**Status:** Needs review`
- [ ] Submitted to `compliance-review` before any client delivery

## Failure Modes
- **No approved campaign brief exists** — do not produce copy; run `campaign-brief` first
- **`hook-library.md` does not exist** — run `social-media-mining` first; do not invent a hook library from scratch
- **`hashtag-sets.md` does not exist** — run `social-media-mining` first; note hashtag gap in output file
- **Claim needed but not in `approved-claims.md`** — write copy without the claim; add `<!-- CLAIM NEEDED: "[claim]" — awaiting client sign-off -->` comment; do not invent
- **Bilingual Spanish needed but no Spanish voice profile in brand-voice-guide.md** — flag and halt Spanish production; run `brand-voice-normalizer` first
- **Trend hook feels forced for this practice's voice** — do not use the trend; note in output file; a bad trend fit is worse than no trend participation
- **Topic requires clinical explanation beyond safe scope** — write a simple patient-friendly version; add `<!-- CLINICAL REVIEW FLAG: dentist should verify accuracy of [claim] before publishing -->`

## Dependencies
- `campaign-brief` (must be approved before this skill runs)
- `social-media-mining` (provides hook-library.md and hashtag-sets.md)
- `keyword-research` (provides keyword-clusters.md)
- `brand-voice-normalizer` (provides brand-voice-guide.md)
- `compliance-review` (mandatory after production — no output leaves without it)
- `_context/clients/{client-slug}/approved-claims.md`
- `_context/clients/{client-slug}/banned-claims.md`
- `_context/global/compliance-rules.md`
- `_context/global/content-pillars.md`
- `_context/global/platform-rules.md`
- `_templates/social-post-template.md`
- `_templates/reel-script-template.md`

**Feeds into:** `compliance-review` skill, `compliance-qa-reviewer` agent, `social/approvals/` after approval
