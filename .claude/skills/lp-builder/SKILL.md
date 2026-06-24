# Skill: lp-builder

## Description
Produces complete landing page wireframes and copy for dental practice conversion pages — lead magnet download pages, service-specific pages, new patient offer pages, and campaign pages. Selects the primary keyword from `keyword-clusters.md`, benchmarks top-ranking competitors via DataForSEO, writes all section copy, specifies form fields and CTA mechanics, and produces mobile-first conversion optimization notes. Output is a filled wireframe + full copy ready for a developer or website builder to implement. All copy requires compliance review before the page goes live.

## When to Use
- After `lead-magnet` completes — every lead magnet needs a download page
- When a campaign drives paid traffic (Meta ads, Google ads) to a dedicated page
- When a service page needs to be optimized for conversion, not just information
- When the campaign brief includes a landing page as a deliverable

Run after `keyword-research`, `lead-magnet` (if page supports a magnet), and `campaign-brief` approval. Run before driving any ad traffic. `social-copy` and ad copy are written after the LP is approved — ad copy must match LP copy.

## Required Inputs

| Input | Priority | Source |
|---|---|---|
| Approved campaign brief | Required | `social/calendars/{client-slug}/` |
| `_context/clients/{client-slug}/keyword-clusters.md` | Required | keyword-research |
| `_context/clients/{client-slug}/brand-voice-guide.md` | Required | brand-voice-normalizer |
| `_context/clients/{client-slug}/brand-context.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/approved-claims.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/banned-claims.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/patient-personas.md` | Required | market-research |
| Lead magnet content | Required (if lead magnet page) | `pages/lead-magnet-pages/{client-slug}/` |
| `_context/global/compliance-rules.md` | Required | Global context |
| `_context/global/platform-rules.md` | Required | Global context |
| `_context/global/southeast-florida-market.md` | Required | Global context |
| `_templates/landing-page-wireframe.md` | Required | Template |

## Landing Page Types

| Type | Primary goal | Traffic source | Complexity |
|---|---|---|---|
| **Lead magnet download** | Capture contact info in exchange for a free guide | Social organic, paid social, GBP | Simple — minimal copy, form is the hero |
| **Service page (conversion)** | Drive consultation bookings for a specific treatment | Google search, paid search, social | Full — all sections needed |
| **New patient offer** | Convert first-time visitors with a specific offer | Paid social, GBP, email | Medium — offer is the hero |
| **Campaign page** | Support a specific seasonal or thematic campaign | All sources | Medium to full |

## Keyword Selection for SEO

Landing pages are indexed by Google. The primary keyword determines the title tag, H1, URL slug, and meta description — and directly affects whether the page appears in search results for high-intent local queries.

**Select the primary keyword from `keyword-clusters.md`:**
- Use the **Local/Booking intent** cluster for the service being promoted
- Highest CPC keyword in the cluster = highest commercial value
- Include a geographic modifier: "[service] [city]" or "[service] near [neighborhood]"
- Must have at least 100 monthly searches in the service area

**Run DataForSEO search volume confirmation:**
```bash
AUTH=$(echo -n "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" | base64)
curl -s -X POST "https://api.dataforseo.com/v3/keywords_data/google_ads/search_volume/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keywords": ["dental implants miami", "implant dentist coral gables"],
    "location_name": "Miami,Florida,United States",
    "language_name": "English"
  }]'
```

**Primary keyword selection:** highest volume + highest CPC among Local/Booking intent options that match the practice's service area and the campaign's service focus.

## SERP Competitor Benchmark

Before writing copy, understand what top-ranking dental pages look like for the primary keyword. This prevents producing a page that's outclassed before it goes live.

**DataForSEO SERP pull:**
```bash
curl -s -X POST "https://api.dataforseo.com/v3/serp/google/organic/live/advanced" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keyword": "dental implants miami",
    "location_name": "Miami,Florida,United States",
    "language_name": "English",
    "depth": 5
  }]'
```

From the top 5 organic results, extract via Firecrawl (or Crawl4AI as fallback):
- H1 headlines — how do competitors frame the value proposition?
- CTA language — what actions are they asking visitors to take?
- Social proof signals — ratings, review counts, years in practice
- FAQ content — which patient questions are they answering?
- Page structure — how many sections? Where is the CTA?

Note what competitors are NOT doing — those are differentiation opportunities.

Save benchmark notes to: `research/market/{client-slug}/lp-benchmark-{YYYY-MM-DD}.md`

## Process

### Step 1 — Define page parameters
From the campaign brief and lead magnet outputs, establish:

```
Page type: [Lead magnet download / Service / Offer / Campaign]
Primary service: [from service-offers.md]
Campaign offer: [from approved-claims.md — verified offer or null]
Target persona: [from patient-personas.md — name + primary concern]
Primary keyword: [selected from keyword-clusters.md + DataForSEO confirmed]
Traffic source(s): [Organic / Meta ad / Google ad / GBP / Email]
Language: [English / Spanish / Both]
Bilingual flag: [from brand-context.md]
Lead magnet: [title + file path, or null]
```

**Ad traffic flag:** If Meta or Google ad traffic will land on this page, note it — ad compliance requirements apply to both the ad and the destination page. These will be checked in the compliance step.

### Step 2 — Write SEO metadata
Write these elements before drafting any page copy — they set the direction for every headline that follows.

**Title tag (60 characters max):**
```
[Primary keyword] — [Practice name] | [City]
```
Example: `Dental Implants Miami — Brickell Smiles | Coral Gables`

**Meta description (155 characters max):**
One sentence: patient benefit + service + location + CTA verb.
Example: `Permanent tooth replacement in Miami. Free implant consultations at Brickell Smiles — serving Coral Gables patients since 2008. Book online today.`

**URL slug:**
```
/[service]-[city]          → /dental-implants-miami
/[magnet-topic]-guide      → /dental-implant-guide-miami
/[offer]-[city]            → /new-patient-offer-coral-gables
```
Use hyphens only, all lowercase, no special characters.

**H1:**
The page's main heading. Must include the primary keyword and match the title tag closely — not identical, but thematically aligned.

### Step 3 — Write the hero section
The hero is the most important section. Everything a visitor needs to decide to keep reading must be here.

**Hero headline (H1):** Patient benefit + implied primary keyword. Under 10 words.

**Formula options:**
| Formula | Example |
|---|---|
| Outcome + Location | "A Permanent Smile Solution in Miami" |
| Service + Patient benefit | "Dental Implants That Feel Like Natural Teeth" |
| Fear reduction | "Dental Implants Without the Anxiety — Miami's Gentle Approach" |
| Question format | "Missing a Tooth? Here's Your Permanent Solution in Coral Gables" |
| Offer-led | "Free Dental Implant Consultation — Serving Miami Since 2008" |

**Subheadline:** Expands the headline — adds specificity, persona acknowledgment, or a secondary benefit. Under 20 words.

**Primary CTA button text:**
- Lead magnet page: "Download My Free Guide" / "Send Me the Guide"
- Service page: "Book a Free Consultation" / "Schedule My Visit"
- Offer page: "Claim My [offer]" / "Book My Appointment"

**Hero trust signals (above the fold):**
Pull from `approved-claims.md`. Maximum 3 signals, shortest possible format:
- ⭐ [N.N] Google rating ([N]+ reviews)
- Accepting new patients
- [N] years serving [city]

### Step 4 — Write the value/solution section
This section bridges the patient's concern to the practice's offering. Benefits, not features.

**Section headline:** "[Service] without [common fear or objection]"
- "Dental Implants Without the Scary Procedure"
- "A Straighter Smile Without Anyone Knowing You're in Braces"

**Benefits (3–5):**
Each benefit is one patient outcome, phrased from the patient's perspective:
- Good: "Wake up with a complete, confident smile"
- Bad: "Advanced implant technology using titanium posts"

Pull benefit language from `patient-personas.md` — what does the target persona want to feel after treatment?

### Step 5 — Write the trust/credibility section
Specifics outperform vague language. Every claim must be in `approved-claims.md`.

**Doctor block:**
`[Name], [DDS/DMD] — [N] years of experience — [specialty if applicable]`

**Practice facts (bullet format — verified only):**
Pull verbatim from `approved-claims.md`. Examples:
- "Accepting new patients"
- "In-network with most PPO plans"
- "Saturday appointments available"
- "Bilingual team — Spanish-speaking staff available" (if bilingual: true)

**GBP rating line (if included in approved-claims.md):**
`⭐ [N.N] rating on Google — [N]+ reviews`

Do not include a GBP rating that isn't verified as current in `approved-claims.md`.

### Step 6 — Write the process/what-to-expect section
Uncertainty is one of the highest barriers to dental booking. This section removes it.

**Section headline:** "Here's What Happens at Your [Appointment type]"
- "Here's What Happens at Your First Implant Consultation"
- "Your Invisalign Journey — 4 Simple Steps"

**Steps (3–5):**
Written from the patient's perspective — what they do, what they experience:
1. "Call or book online — same-day appointments often available"
2. "Meet Dr. [Name] — no pressure, no commitment, just a conversation"
3. "Get your personalized treatment plan with clear pricing"
4. [Continue per service]

Each step: one sentence. Plain language. No clinical jargon.

### Step 7 — Lead magnet offer block (for download pages only)
**Offer headline:** "Get Your Free [Guide Title]"

**Value description (2-3 sentences):**
What does the patient learn? What decision does it help them make? Why is it worth their contact info?

**Form specification:**
```
Field 1: First name (required)
Field 2: Phone number (required, tel: format for mobile auto-dial)
Field 3: Email address (required or optional — match campaign CRM setup)
Field 4: Service interest (optional dropdown — helps qualify leads)

Submit button: "Download My Free Guide" / "Send Me the Guide"
Privacy note: "We respect your privacy. Your information is never shared or sold."
Delivery method: Immediate email delivery / On-screen PDF link / Both
```

**Form data handling note:**
Specify where submissions go — this must be answered by the practice:
- Email notification to front desk?
- CRM integration (GoHighLevel, etc.)?
- Practice management system integration?

Leave as a placeholder if not yet determined: `[Practice to confirm: form submissions deliver to ___]`

### Step 8 — Write the FAQ section
Pull directly from the `keyword-clusters.md` FAQ map for the primary service. Prioritize questions marked High priority.

Write 4–8 FAQ entries. Each answer: under 100 words, patient-friendly language, no clinical jargon without explanation.

**Compliance check on FAQ answers:**
- No answer should constitute personalized treatment advice
- No answer should diagnose or prescribe
- Every answer about cost or outcomes that makes a specific claim must be in `approved-claims.md`
- Add this at the end of cost or outcome answers: "Specific costs depend on your situation — your free consultation includes a personalized treatment plan with clear pricing."

### Step 9 — Write the final CTA section
Mirror the hero CTA. Add genuine urgency only if the campaign has a real deadline.

**Headline:** Reinforce the core benefit one more time — different phrasing from hero.
**Supporting copy (1–2 sentences):** What happens when they click? Remove friction.
**CTA button:** Same as hero.
**Urgency line (if real):** "This offer expires [verified date]."

**Below the CTA — friction reducers:**
- "No commitment required"
- "Free consultation — no pressure"
- "We accept most PPO plans" (verified)

### Step 10 — Spanish version (required if `bilingual: true`)
Produce a full Spanish-language version of all copy. Not a translation — an adaptation.

**Spanish LP considerations:**
- Hero headline in Spanish may lead with trust and relationship rather than outcome: "Su dentista de confianza en Miami" vs. the English outcome-first approach
- Cost section: acknowledge self-pay and financing options prominently (common concern for Spanish-speaking patients in SE Florida)
- Use usted/tú register from `brand-voice-guide.md` Spanish voice profile — never mix
- Mark all Spanish copy: `<!-- Spanish — needs native-speaker review before going live -->`

**Separate Spanish URL slug:**
```
/implantes-dentales-miami        (Spanish version)
/dental-implants-miami           (English version)
```

Or: add `?lang=es` parameter if the CMS handles bilingual routing.

### Step 11 — Conversion optimization and mobile-first audit
Before finalizing, apply these CRO and mobile checks to the full page copy and structure:

**CRO checklist:**
- [ ] One primary CTA per page — no competing actions
- [ ] CTA appears above the fold (visible on 375px mobile without scrolling)
- [ ] CTA repeated at the bottom
- [ ] Phone number is a `tel:` link for mobile tap-to-call
- [ ] No outbound links that take the visitor off the page (except privacy policy)
- [ ] Hero image direction: compressed, fast-loading, not above copy on mobile
- [ ] Form fields specified: no more fields than necessary to qualify the lead
- [ ] Trust signals above the fold

**Ad traffic compliance (if applicable):**
- [ ] Meta: No negative self-image language; no before/after patient photos; no "guaranteed" outcomes
- [ ] Google: LP content matches the ad's intent — visitor lands where the ad promised
- [ ] Both: Page speed target < 3s load time (note for developer)

**A/B testing recommendations:**
Suggest 2 elements to A/B test at launch:
1. Headline variant A vs. B (different formula)
2. CTA button text variant (e.g., "Book Free Consultation" vs. "Schedule My Visit")

### Step 12 — Compliance review
Before filing as complete:

- [ ] No diagnosis or personalized treatment advice
- [ ] No guaranteed outcomes
- [ ] All trust signals, credentials, stats, and offers in `approved-claims.md`
- [ ] No patient before/after without documented authorization
- [ ] No shame-based or fear-based language
- [ ] Disclaimer present in footer
- [ ] Form: privacy note present; data handling documented
- [ ] Spanish copy flagged for native-speaker review
- [ ] Ad traffic compliance met (Meta / Google as applicable)

**Risk score:**
- Low: Lead magnet download page with no clinical claims
- Medium: Service page with practice credentials and FAQs
- High: Page with specific success rates, cost ranges, or clinical statistics

### Step 13 — Fill the wireframe template and save
Fill `_templates/landing-page-wireframe.md` with all copy and specifications from Steps 1–12.

**Save path:**
```
pages/landing-pages/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_lp-wireframe_v1.md
```

For the full copy document (without wireframe annotations):
```
pages/landing-pages/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_lp-copy_v1.md
```

For Spanish versions:
```
pages/landing-pages/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_lp-wireframe_v1_es.md
pages/landing-pages/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_lp-copy_v1_es.md
```

Every file ends with: `**Status:** Needs review`

Include at the top:
```
<!-- Primary keyword: [keyword] | Monthly volume: [N] | CPC: $[N] -->
<!-- Traffic source: [source] | Ad compliance: [required/not required] -->
<!-- Lead magnet: [title or null] | Risk score: Low/Medium/High -->
```

### Step 14 — Handoff notes
Append to `logs/decisions.md`:
```
## {date} — Landing Page Built: {client-name}
- Page type: [type]
- Primary keyword: [keyword] — [N] monthly searches
- Primary service: [service]
- Target persona: [persona name]
- Lead magnet: [title or null]
- Languages: English / Spanish / Both
- Ad traffic: [Yes — Meta / Google / No]
- Risk score: Low / Medium / High
- Next steps: compliance-review → developer implementation → social-copy (promotion)
- Performance tracking: lp-conversion-rate in campaign-report
```

## Output Files
```
pages/landing-pages/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_lp-wireframe_v1.md    ← filled template
  {YYYY-MM-DD}_{client-slug}_{topic}_lp-copy_v1.md          ← clean copy only
  {YYYY-MM-DD}_{client-slug}_{topic}_lp-wireframe_v1_es.md  ← Spanish wireframe
  {YYYY-MM-DD}_{client-slug}_{topic}_lp-copy_v1_es.md       ← Spanish copy

research/market/{client-slug}/
  lp-benchmark-{YYYY-MM-DD}.md   ← SERP competitor benchmark notes

logs/
  decisions.md   ← handoff notes appended
```

## Quality Checklist
- [ ] Page type defined; traffic source(s) identified
- [ ] Primary keyword selected from keyword-clusters.md Local/Booking intent cluster
- [ ] DataForSEO volume confirmed for primary keyword (100+ monthly searches)
- [ ] SERP competitor benchmark completed; differentiation opportunities noted
- [ ] SEO metadata written: title tag (≤60 chars), meta description (≤155 chars), URL slug, H1
- [ ] Hero section: headline, subheadline, CTA button text, trust signals (all verified)
- [ ] Value section: 3-5 patient-outcome benefits in patient vocabulary
- [ ] Trust section: doctor credentials and practice facts verified in approved-claims.md
- [ ] Process section: 3-5 steps from patient's perspective, plain language
- [ ] Lead magnet block complete with form spec and data handling note (if applicable)
- [ ] FAQ section: 4-8 questions from keyword-clusters.md; each answer under 100 words
- [ ] Final CTA mirrors hero; urgency only if real deadline exists
- [ ] Spanish version produced and flagged for review (required if bilingual: true)
- [ ] CRO checklist complete: one CTA, above-fold, mobile tel: link, no outbound links
- [ ] Ad compliance checked (if applicable): Meta + Google landing page policies
- [ ] A/B test recommendations included
- [ ] Compliance review complete; risk score assigned
- [ ] All files saved to `pages/landing-pages/{client-slug}/`
- [ ] Handoff notes written to `logs/decisions.md`

## Failure Modes
- **No keyword reaches 100 monthly searches** — broaden the keyword (county-level instead of city-level, or "dental [service]" instead of "[service] dentist [city]"); if still below threshold, note that this LP is conversion-focused only, not SEO-optimized, and organic traffic should not be expected
- **SERP benchmark shows 5 well-optimized competitors** — differentiate on: local specificity (neighborhood names), brand personality, specific offer, bilingual content — not on generic claims like "best" or "expert"
- **Form data handling not confirmed by practice** — leave as placeholder in the wireframe; do not launch page until this is documented — collecting leads without knowing where they go is a compliance and operational risk
- **Meta ad policy triggers on LP content** — Meta health and wellness policy restricts before/after patient content and certain health claim language on ads AND destinations; review the LP for "before/after" references, negative self-image language, and guaranteed results; remove any that trigger
- **Practice wants the LP to also serve as a service page on their main website** — note the dual purpose; a service page needs more content (longer, more educational) while an LP is optimized for one action; recommend two separate pages — LP for campaign traffic, service page for organic
- **No lead magnet exists yet** — flag; run `lead-magnet` skill first; a download page cannot be built without content to download

## Dependencies
- DataForSEO (`DATAFORSEO_LOGIN`, `DATAFORSEO_PASSWORD` — for keyword volume + SERP benchmark)
- Firecrawl (primary — competitor LP analysis) or Crawl4AI (fallback)
- `keyword-research` (provides keyword-clusters.md — required for keyword selection)
- `lead-magnet` (if building a download page — content must exist before the LP)
- `campaign-brief` (must be approved)
- `brand-voice-normalizer` (provides brand-voice-guide.md)
- `client-context-ingestion` (provides approved-claims.md, brand-context.md)
- `market-research` (provides patient-personas.md)
- `compliance-review` (mandatory before page goes live)
- `_templates/landing-page-wireframe.md`

**Feeds into:** `compliance-qa-reviewer` agent (copy approval), `social-copy` (ad copy must mirror LP copy), `campaign-report` (LP conversion rate tracking), developer/web team implementation
