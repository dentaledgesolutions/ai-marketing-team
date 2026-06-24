# Skill: lead-magnet

## Description
Creates educational lead magnets — guides, checklists, FAQ sheets, comparison guides, cost guides, anxiety guides, insurance guides, and seasonal guides — that convert social media followers into booked patients. Each lead magnet addresses a specific patient concern related to a high-value dental service, delivers genuine educational value, and ends with a frictionless CTA. Validates topic demand against DataForSEO data before investing in full content production. All output requires compliance review before distribution.

## When to Use
- When a campaign brief includes a lead magnet content idea
- Before launching a paid social campaign for a high-revenue service (implants, veneers, Invisalign, full-mouth restoration)
- When the practice wants to grow an email or SMS list
- When a high patient-objection service needs educational nurturing before patients book
- Strongest use cases: implant campaigns, orthodontic campaigns, cosmetic campaigns, dental anxiety campaigns, year-end insurance deadline campaigns

Run after `keyword-research` (topic validation) and after `campaign-brief` is approved. Run before `lp-builder` (which builds the download page) and before `social-copy` (which promotes the magnet).

## Required Inputs

| Input | Priority | Source |
|---|---|---|
| Approved campaign brief — specific lead magnet idea | Required | `social/calendars/{client-slug}/` |
| `_context/clients/{client-slug}/keyword-clusters.md` | Required | keyword-research |
| `_context/clients/{client-slug}/patient-personas.md` | Required | market-research |
| `_context/clients/{client-slug}/brand-voice-guide.md` | Required | brand-voice-normalizer |
| `_context/clients/{client-slug}/brand-context.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/approved-claims.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/banned-claims.md` | Required | client-context-ingestion |
| `_context/global/compliance-rules.md` | Required | Global context |
| `_context/global/dental-service-taxonomy.md` | Required | Global context |
| `_context/global/southeast-florida-market.md` | Required | Global context |
| `_templates/lead-magnet-outline.md` | Required | Template |
| Bilingual flag | Required | Check brand-context.md |

## Lead Magnet Types
Select the type that best matches the patient concern and campaign goal:

| Type | Patient concern it addresses | Best service triggers | Typical length |
|---|---|---|---|
| **Guide / e-book** | "I want to understand this treatment before committing" | Implants, full-mouth restoration, clear aligners | 800–1,500 words, 4–8 pages |
| **Checklist** | "What do I need to do / ask / prepare?" | Pre-treatment prep, insurance use, back-to-school | 200–400 words, 1–2 pages |
| **FAQ Sheet** | "I have specific questions I'm too embarrassed to ask" | Any treatment; high anxiety value | 400–800 words, 2–4 pages |
| **Comparison Guide** | "I don't know which option is right for me" | Implants vs. dentures, braces vs. aligners, crowns vs. veneers | 600–1,200 words, 3–6 pages |
| **Cost Guide** | "I'm worried I can't afford this" | Implants, veneers, Invisalign, smile makeover | 400–800 words, 2–4 pages |
| **Anxiety Guide** | "I'm scared of the dentist / this procedure" | Any treatment; especially oral surgery, root canal, extractions | 500–900 words, 3–5 pages |
| **Insurance Guide** | "I don't know if my insurance covers this" | Year-end deadline campaigns; any PPO-covered service | 300–600 words, 2–3 pages |
| **Seasonal Guide** | "This moment is relevant to me right now" | Back-to-school, holiday, new year, tax season | 300–600 words, 2–3 pages |

## High-Value Lead Magnet Opportunities for SE Florida Dental
These service + concern combinations produce the highest lead magnet conversion:

| Service | Top patient concern | Best magnet type |
|---|---|---|
| Dental implants | Cost, pain, timeline | Cost Guide + Anxiety Guide |
| Clear aligners / Invisalign | Cost, effectiveness vs. braces | Comparison Guide |
| Veneers / smile makeover | Cost, process, permanence | Guide |
| Full-mouth restoration | Where to start, cost | Guide |
| Dental anxiety (any treatment) | Fear of pain, past trauma | Anxiety Guide |
| Year-end insurance deadline | What's covered, how to use benefits | Insurance Guide |
| Back-to-school orthodontics | Process, timeline, school impact | FAQ Sheet + Checklist |
| Emergency dentistry | What to do, how much it costs | FAQ Sheet |

## Process

### Step 1 — Load context and define parameters
Read all required inputs. From the campaign brief, extract the lead magnet idea:

```
Campaign brief idea #: [N]
Patient concern being addressed: [specific concern]
Service / topic: [from dental-service-taxonomy.md]
Target persona: [from patient-personas.md — name + primary concern]
Magnet type: [from type table above]
Language: [English / Spanish / Both]
Bilingual flag: [true / false from brand-context.md]
```

From `patient-personas.md`, identify the specific persona this magnet serves:
- What is their primary barrier to booking? (Cost / Fear / Confusion / Trust / Language)
- What question keeps them up at night about this service?
- What would make them feel safe enough to book a consultation?

The answer to the last question is the lead magnet's core promise.

### Step 2 — Validate topic demand (DataForSEO)
Before writing a single word, confirm the topic has enough patient search intent to generate leads.

Check `keyword-clusters.md` for this service's FAQ map and Informational/Consideration keywords. Identify the top 3 questions patients search when researching this service.

If DataForSEO credentials are available, run a targeted check:
```bash
AUTH=$(echo -n "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" | base64)
curl -s -X POST "https://api.dataforseo.com/v3/keywords_data/google_ads/search_volume/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keywords": ["dental implant cost miami", "how much do veneers cost"],
    "location_name": "Miami,Florida,United States",
    "language_name": "English"
  }]'
```

**Demand threshold:** The lead magnet topic should have at least one keyword with 100+ monthly searches in the service area. Below 100 searches/month for all related queries → choose a broader topic or combine with another concern.

Note the validated keyword(s) — this becomes the SEO anchor for the lead magnet title and the landing page (`lp-builder` will use it).

If DataForSEO is unavailable, check `keyword-clusters.md` Informational section for the service — if 3+ patient questions exist, topic demand is sufficient.

### Step 3 — Write the lead magnet title
The title is the opt-in hook. It must be specific, patient-benefit focused, and match the patient's own mental language for their concern.

**Title formula options:**

| Formula | Example |
|---|---|
| Number + benefit + location | "7 Things to Know Before Getting Dental Implants in Miami" |
| Question format | "Is Invisalign Right for You? A Coral Gables Patient's Guide" |
| The honest guide | "The Real Cost of Dental Implants in South Florida — No Hidden Fees" |
| Before you decide | "Before You Get Veneers: What Every Patient Should Ask" |
| Fear reduction | "The Nervous Patient's Complete Guide to Root Canals" |
| Urgency + benefit | "Use Your 2026 Dental Benefits Before They Expire: A Simple Guide" |

**Title rules:**
- Under 10 words for the main title (subheadline handles the expansion)
- Includes the patient's geography when relevant (Miami, South Florida, [city])
- Never uses the practice name in the title — this is patient education, not a brochure
- Matches the exact language patients use (check keyword-clusters.md patient vocabulary)

**Subheadline:** One sentence that expands on who this is for and what they'll learn.

### Step 4 — Write the outline
Using `_templates/lead-magnet-outline.md`, build the structure before writing any content.

**Section count by type:**
- Checklist: Introduction + 1 checklist section + CTA
- FAQ Sheet: Introduction + 5–8 Q&A pairs + CTA
- Guide (4–8 pages): Introduction + 3–5 content sections + FAQ + CTA
- Comparison Guide: Introduction + comparison section (table or side-by-side) + "Which is right for you?" + CTA
- Cost Guide: Introduction + cost breakdown section + financing/insurance section + "What affects the cost?" + CTA
- Anxiety Guide: Introduction + "What to actually expect" + fear-by-fear section + "Tips for anxious patients" + CTA
- Insurance Guide: Introduction + how dental insurance works + what's covered/not covered + "How to use benefits before [date]" + CTA

**Section naming rules:**
- Use patient-centric section headers, not clinical headers
- Good: "What Actually Happens During a Dental Implant Procedure"
- Bad: "Implant Osseointegration Process and Timeline"
- Good: "How Much Does It Actually Cost? Here's a Real Breakdown"
- Bad: "Pricing and Fee Structure"

### Step 5 — Write the full content
Write section by section using the approved outline. Apply these content rules throughout:

**Voice:** Pull from `brand-voice-guide.md`. The lead magnet should sound like the practice — warm and expert, never clinical and distant.

**Claim rules:**
- Every statistic, success rate, or clinical claim must be in `approved-claims.md` or sourced from a verifiable publication (ADA, NIH, peer-reviewed dental journal) — cited inline
- If a patient-benefit claim is not in `approved-claims.md`, write around it: "Many patients report..." (not "Our patients achieve...")
- Never guarantee outcomes: "may," "can," "many patients find" — not "will," "guaranteed"

**Jargon rule:** Every clinical term must be explained in plain language immediately after use. Example: "osseointegration (the process where the implant fuses to your jawbone)."

**SE Florida relevance:** Where natural, include local relevance:
- Cost comparisons should reflect SE Florida pricing ranges (when verified)
- Insurance mentions should include PPO plans common in the area (if verified)
- For bilingual clients: acknowledge Spanish-speaking support where applicable

**Section length:**
- Each section should feel complete on its own — a reader who stops after any section should have gotten value
- Aim for 2–4 paragraphs per section, 100–200 words each
- Checklists and FAQ answers should be short: 50–100 words per item

**Disclaimer:** Include at the end of every lead magnet, every time, verbatim:
> *This guide is for educational purposes only and does not constitute personalized dental advice. Results vary by individual. Please consult with a licensed dentist for treatment recommendations specific to your situation.*

### Step 6 — Write the CTA section
The CTA is the final section. It converts the reader's education into action.

**CTA structure:**
```
[Bridge sentence: acknowledges what they've learned]
[Value reinforcement: what this practice offers]
[Primary CTA: one specific action]
[Secondary CTA (optional): lower-friction alternative]
[Contact info: phone + booking URL]
```

**CTA examples by campaign type:**

*Implants:*
"Now that you know what dental implants involve, the best next step is a free consultation to find out if you're a candidate. [Practice name] offers no-commitment implant consultations in [city] — call [phone] or book at [URL]."

*Anxiety:*
"Dental anxiety is real, and you don't have to face it alone. Our team specializes in treating patients who've avoided the dentist for years. Your first step is a conversation — no treatment until you're ready. Call [phone] or book at [URL]."

*Year-end insurance:*
"Your unused dental benefits disappear on December 31st. Don't leave money on the table. Call [phone] today to schedule before your benefits expire."

**CTA rules:**
- CTA offer must be verified in `approved-claims.md` (free consultation, specific pricing, etc.)
- One primary action only
- Include practice phone number and booking URL
- No fake urgency — if there's a deadline, it must be real

### Step 7 — Spanish version (required if `bilingual: true`)
Produce a Spanish-language version adapted from the English content — not a word-for-word translation.

**Spanish lead magnet principles:**
- Title in Spanish: adapt the benefit-first formula using natural Spanish dental vocabulary
- Tone: may be slightly warmer and more familial than the English version (per brand-voice-guide.md Spanish profile)
- Cost and insurance content: acknowledge that many SE Florida Spanish-speaking patients may be self-pay or on state insurance — frame cost transparency as a trust signal
- Disclaimer: translate and include in the Spanish version
- Mark every Spanish section: `<!-- Spanish — needs native-speaker review before distribution -->`

### Step 8 — Design brief
The lead magnet needs visual design for distribution as a PDF. Write a design brief for `social-creative-designer`:

```markdown
## Lead Magnet Design Brief

**Title:** [Lead magnet title]
**Type:** [PDF / Fillable PDF]
**Page count:** [N pages]
**Brand reference:** brand-style-guide.md

**Cover page:**
- Headline: [exact title]
- Visual: [cover image direction — describe or reference creative-brief]
- Brand colors: [from brand-style-guide.md]

**Interior pages:**
- Typography: [from brand-style-guide.md]
- Color accents: [brand colors in headers and callout boxes]
- Callout box style: [light brand color background with border]
- Checklist / icon style: [simple, modern, branded]

**Back cover / final page CTA:**
- Practice name, logo, phone, URL
- Social handles
- Brief tagline

**Output:** `pages/lead-magnet-pages/{client-slug}/` as PDF
```

### Step 9 — Distribution brief
Produce a brief for `social-copy` and `campaign-brief` to use when promoting this lead magnet:

```markdown
## Distribution Brief: [Lead magnet title]

**Opt-in value proposition (for social copy / ad copy):**
[1–2 sentences: what the reader gets and why they should download it]

**Organic social:**
- Instagram Story: Link sticker → landing page
- Instagram carousel: Tease 3–5 key points from the guide (swipe to see more → download full guide)
- Facebook post: Long-form educational teaser with "Download free guide" CTA

**Paid social (if applicable):**
- Lead gen ad format (Meta lead form — no landing page required)
- Headline: [title]
- Body: [value prop]
- CTA button: "Download"

**GBP offer post:**
"Download our free guide: [title]. Available at [URL]."

**Landing page summary:**
Keyword: [validated keyword from Step 2]
Build with: `lp-builder` skill
Input: This lead magnet + keyword + offer details
```

### Step 10 — Compliance review
Before filing as complete, run through the full compliance checklist:

- [ ] No diagnosis or treatment-specific advice directed at the reader as an individual
- [ ] No guaranteed outcomes ("will", "guaranteed", "100% success")
- [ ] All statistics and clinical claims are in `approved-claims.md` or cited from verified sources (ADA, NIH, peer-reviewed)
- [ ] Sources cited inline for any data or claim not from `approved-claims.md`
- [ ] No patient testimonials or case descriptions without documented authorization
- [ ] No shame-based or fear-based language
- [ ] No implied competitor comparisons (e.g., "unlike chain dental offices...")
- [ ] Disclaimer included at end of every version
- [ ] CTA offer verified in `approved-claims.md`
- [ ] Spanish version flagged for native-speaker review (if produced)
- [ ] Design brief does not call for patient before/after imagery

**Risk score:**
- Low: Checklist, insurance guide, seasonal guide — no clinical claims
- Medium: FAQ sheet, comparison guide — some treatment comparison language
- High: Guide with clinical statistics, cost ranges, or clinical process descriptions — requires closest claim verification

### Step 11 — Save outputs
Fill `_templates/lead-magnet-outline.md` with the approved outline. Save full content as a separate file.

```
pages/lead-magnet-pages/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_lead-magnet-outline_v1.md   ← outline (filled template)
  {YYYY-MM-DD}_{client-slug}_{topic}_lead-magnet-content_v1.md   ← full copy
  {YYYY-MM-DD}_{client-slug}_{topic}_lead-magnet-content_v1_es.md ← Spanish (if bilingual)

creative/briefs/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_lead-magnet-design-brief_v1.md
```

Every file ends with: `**Status:** Needs review`

### Step 12 — Handoff notes
Append to `logs/decisions.md`:
```
## {date} — Lead Magnet Complete: {client-name}
- Title: [lead magnet title]
- Type: [type]
- Target persona: [persona name]
- Validated keyword: [keyword + monthly volume]
- Risk score: Low / Medium / High
- Spanish version: Produced / Not applicable
- Next steps: lp-builder (landing page) + social-copy (promotion posts)
```

## Output Files
```
pages/lead-magnet-pages/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_lead-magnet-outline_v1.md
  {YYYY-MM-DD}_{client-slug}_{topic}_lead-magnet-content_v1.md
  {YYYY-MM-DD}_{client-slug}_{topic}_lead-magnet-content_v1_es.md (if bilingual)

creative/briefs/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_lead-magnet-design-brief_v1.md

logs/
  decisions.md ← handoff notes appended
```

## Quality Checklist
- [ ] Campaign brief content idea identified and parameters defined
- [ ] Target persona identified with primary barrier and core question
- [ ] Topic demand validated: keyword with 100+ monthly searches OR 3+ patient FAQ questions in keyword-clusters.md
- [ ] Lead magnet type selected with rationale
- [ ] Title written using patient-benefit formula and patient vocabulary
- [ ] Outline complete and section headers use patient-centric language
- [ ] All content sections written to target word count
- [ ] Every clinical term explained in plain language on first use
- [ ] SE Florida relevance included where natural
- [ ] No claims used that are not in approved-claims.md or cited from verified source
- [ ] Disclaimer included verbatim at end of every version
- [ ] CTA section: one primary action, verified offer, phone + URL included
- [ ] Spanish version produced and flagged for native-speaker review (required if bilingual: true)
- [ ] Design brief written for social-creative-designer
- [ ] Distribution brief written for social-copy and lp-builder
- [ ] Compliance review complete — risk score assigned
- [ ] All files saved with correct naming to `pages/lead-magnet-pages/{client-slug}/`
- [ ] Handoff notes written to `logs/decisions.md`

## Failure Modes
- **Topic demand too low** — topic has fewer than 100 monthly searches and fewer than 3 FAQ questions; broaden the topic scope (e.g., "dental implants" instead of "mini implants"), combine with a related concern, or recommend a different magnet type
- **No verified claims available for the topic** — write content grounded in ADA or NIH educational material with citations; avoid any claim that requires practice-specific verification; flag all unverified claims for dentist sign-off
- **Lead magnet requires before/after imagery** — do not include patient before/after; use illustrated comparison graphics instead; note "before/after requires documented HIPAA authorization" in design brief
- **Spanish translation requested instead of adaptation** — refuse and produce a proper adaptation; flag the request: "Word-for-word translation of English dental content does not convert in SE Florida's Spanish-speaking market — native-language adaptation required"
- **Content is too long** — cut body sections to hit the type's target word count; do not cut the disclaimer or CTA; the guide should feel like a focused gift, not a textbook
- **Clinical claim needed but not in approved-claims.md** — write around it; add `<!-- CLAIM FLAG: "[claim]" — needs dentist verification before distribution -->`; do not publish without sign-off

## Dependencies
- DataForSEO (for topic demand validation — `DATAFORSEO_LOGIN`, `DATAFORSEO_PASSWORD`)
- `keyword-research` (provides keyword-clusters.md and patient FAQ map)
- `market-research` (provides patient-personas.md)
- `brand-voice-normalizer` (provides brand-voice-guide.md)
- `campaign-brief` (must be approved — provides the specific lead magnet idea)
- `compliance-review` (mandatory before distribution)
- `_context/global/dental-service-taxonomy.md`
- `_context/global/southeast-florida-market.md`
- `_templates/lead-magnet-outline.md`

**Feeds into:** `lp-builder` (landing page for the download), `social-copy` (promotional posts), `social-creative-designer` (cover image and PDF design), `compliance-qa-reviewer` agent (final approval before distribution)
