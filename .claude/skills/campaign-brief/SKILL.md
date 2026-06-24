# Skill: campaign-brief

## Description
Synthesizes all upstream research into a complete monthly campaign brief that directs every downstream content skill and agent. Fills `_templates/campaign-brief.md` and extends it with a content idea bank, content volume plan, trend integration slots, keyword targets, bilingual content plan, and production timeline. The campaign brief is the single source of truth for the month — every caption, script, carousel, and GBP post produced that month traces back to it.

## When to Use
- Once per month, before building the content calendar
- When launching a new service campaign or seasonal push
- When onboarding a new client and producing the first month of content
- After the trend report is scored and before any content production begins

Run after: `market-research`, `keyword-research`, `competitive-intel-scraper`, `social-media-mining`, `trend-detection-scoring`, `brand-voice-normalizer`.
Run before: `social-copy`, `short-form-video-script`, `lead-magnet`, `lp-builder`, content calendar build.

## Required Inputs

| Input | Priority | Source |
|---|---|---|
| `_context/clients/{client-slug}/market-context.md` | Required | market-research |
| `_context/clients/{client-slug}/patient-personas.md` | Required | market-research |
| `_context/clients/{client-slug}/content-angles.md` | Required | market-research |
| `_context/clients/{client-slug}/keyword-clusters.md` | Required | keyword-research |
| `_context/clients/{client-slug}/brand-context.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/brand-voice-guide.md` | Required | brand-voice-normalizer |
| `_context/clients/{client-slug}/service-offers.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/approved-claims.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/banned-claims.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/hook-library.md` | Required | social-media-mining |
| `_context/clients/{client-slug}/hashtag-sets.md` | Required | social-media-mining |
| `_context/clients/{client-slug}/competitors.md` | Required | competitive-intel-scraper |
| `_context/global/content-pillars.md` | Required | Global context |
| `_context/global/platform-rules.md` | Required | Global context |
| `_context/global/southeast-florida-market.md` | Required | Global context |
| `_context/global/compliance-rules.md` | Required | Global context |
| Most recent trend report | Required | `reports/weekly-trend-reports/` |
| Campaign month and year | Required | Provided at invocation |
| Campaign type | Required | See Campaign Types below |

## Campaign Types
Declare the campaign type at invocation. It drives pillar weighting and content volume decisions.

| Type | Primary goal | Pillar emphasis | Typical trigger |
|---|---|---|---|
| **New patient acquisition** | Drive first appointments | Service Demand + Promotional | New client, slow period, new market |
| **High-revenue service** | Fill schedule for a specific high-value treatment | Service Demand + Education | Implants, veneers, Invisalign push |
| **Seasonal** | Ride a local or calendar moment | Local Community + Promotional | Back to school, holiday, tax season, Hispanic Heritage Month |
| **Brand/trust building** | Grow awareness and recognition | Trust + Team + Education | Early client stage, post-rebrand |
| **New service launch** | Introduce a service the practice just added | Education + Service Demand | New technology, new provider, new treatment |
| **Retention** | Re-engage existing patients | Education + Trust + Promotional | Recall campaigns, year-end insurance reminders |

If the campaign type is unclear, default to **New patient acquisition** and note this in the brief.

## Campaign Pillar Mix by Type
These are starting allocations. Adjust based on client preference and active promotions.

| Campaign type | Education | Trust | Service | Local | Team | Promotional |
|---|---|---|---|---|---|---|
| New patient acquisition | 20% | 20% | 25% | 10% | 10% | 15% |
| High-revenue service | 30% | 15% | 30% | 5% | 5% | 15% |
| Seasonal | 15% | 15% | 20% | 25% | 10% | 15% |
| Brand/trust building | 25% | 30% | 15% | 10% | 15% | 5% |
| New service launch | 35% | 15% | 30% | 5% | 5% | 10% |
| Retention | 25% | 20% | 20% | 10% | 10% | 15% |

## Process

### Step 1 — Load all upstream context
Read all required inputs in this order:
1. `brand-context.md` — practice identity, bilingual flag, active platforms
2. `brand-voice-guide.md` — tone, vocabulary, hook starters, CTAs
3. `market-context.md` — service demand matrix, patient personas, seasonal calendar
4. `service-offers.md` — full service list and current verified offers
5. `approved-claims.md` — what can be claimed; `banned-claims.md` — what cannot
6. `keyword-clusters.md` — search topics, FAQ questions, local keyword combinations
7. `content-angles.md` — local angles from market research
8. `hook-library.md` + `hashtag-sets.md` — creative patterns from social mining
9. `competitors.md` — competitive whitespace notes
10. `compliance-rules.md` — global compliance boundaries
11. `content-pillars.md` — pillar definitions and balance guidance
12. `platform-rules.md` — per-platform specs and frequency norms
13. `southeast-florida-market.md` — seasonal calendar, local cultural moments
14. Most recent trend report — Priority and Meaningful trends

From this context, extract before proceeding:
- Top 3 priority services (from market-context.md service demand matrix)
- Bilingual flag (from brand-context.md)
- Active platforms (from brand-context.md)
- Campaign month's seasonal hooks (from southeast-florida-market.md)
- Priority and Meaningful trends (from trend report)

### Step 2 — Define campaign parameters
Establish the five parameters that drive all downstream decisions:

```
Campaign type:     [one of the six types above]
Campaign month:    [Month Year]
Campaign period:   [YYYY-MM-DD → YYYY-MM-DD]
Campaign goal:     [one sentence: what does this campaign need to achieve?]
Success signal:    [what measurable signal indicates the campaign worked?
                    e.g., "10 new patient inquiries", "500 Reel views on implants content"]
```

**Seasonal context check:**
Look up the campaign month in `southeast-florida-market.md` seasonal calendar. Note:
- Which dental services peak this month
- Which local cultural moments are present
- Any SE Florida-specific events or conditions

If there is a strong seasonal fit between the calendar moment and the campaign type, note it as the campaign theme anchor.

### Step 3 — Select service focus and patient segment

**Primary service selection:**
From the service demand matrix in `market-context.md`, select:
- **Primary service:** #1 priority service for this campaign (highest demand score aligned with campaign type)
- **Secondary service (optional):** A complementary service that naturally pairs with the primary

**Patient segment selection:**
From `patient-personas.md`, select the 1–2 personas most likely to be reached by this campaign:
- Match persona's primary dental concern to the primary service
- Match persona's platform preference to the active platforms
- Match persona's language to the bilingual plan (Step 10)

Document the selection rationale in one sentence per choice.

### Step 4 — Define offer and CTA
If the campaign type calls for a promotional element (New patient acquisition, Seasonal, Retention):

**Offer selection:**
1. Check `approved-claims.md` for verified offers available for the campaign period
2. Select the offer most aligned with the primary service and target persona
3. If no verified offer exists, flag "Offer needed — request from client" and proceed without one

**Offer validation:**
Every offer included in the brief must pass this check:
- [ ] Offer text appears verbatim in `approved-claims.md` OR is explicitly marked as pending dentist sign-off
- [ ] Expiration date is real (not invented)
- [ ] Terms and restrictions are documented (insurance eligibility, location, etc.)
- [ ] Offer does not guarantee outcomes ("guaranteed results" is banned)

**CTA selection:**
Pull from `brand-voice-guide.md` approved CTAs. Select the CTA style that matches the campaign type:
- Soft CTA for Brand/Trust campaigns: "Learn more at [URL]" or "Ask us at your next visit"
- Direct CTA for Acquisition/Seasonal: "Book online at [URL]" or "Call [phone]"
- Low-friction CTA for retention: "Schedule before your benefits expire at [URL]"

### Step 5 — Set content pillar mix
Apply the base pillar allocation for the campaign type (see table above). Then adjust for:

**Active promotions:** If there's a time-sensitive offer, bump Promotional by 5% and reduce Education by 5%
**Trend integration:** If a Priority trend aligns with a specific pillar, note that pillar gets additional content slots
**Client preference:** If `brand-voice-guide.md` notes a preference (e.g., "practice prefers less promotional content"), reduce Promotional and redistribute

**Convert percentages to post counts:**
Assume a default of 20 posts per month across all platforms combined.
Multiply each pillar percentage by 20 to get post count per pillar.

```
Example for New Patient Acquisition (20 posts):
Education:    20% → 4 posts
Trust:        20% → 4 posts
Service:      25% → 5 posts
Local:        10% → 2 posts
Team:         10% → 2 posts
Promotional:  15% → 3 posts
Total:                20 posts
```

Adjust total post count up or down based on the client's active platforms and capacity.

### Step 6 — Integrate trend opportunities
From the most recent trend report, pull all Priority and Meaningful trends:

For each trend, assign it to:
- A content pillar (which pillar does this trend serve best?)
- A platform (which platform is the trend most active on?)
- A production window (when must content be produced by, given trend decay rate?)
- A content format (Reel, carousel, GBP post, etc.)

**Trend slot assignment:**
Priority trends get a dedicated slot in the content plan — they do not replace existing pillar content, they add to it (or swap with a lower-priority slot if capacity is constrained).

Meaningful trends get a suggested slot but are optional if production capacity is limited.

If no Priority or Meaningful trends exist this month, note "No trend integration this cycle" and proceed.

### Step 7 — Design channel strategy and content volume
For each active platform in `brand-context.md`, define:

```
Platform:           [name]
Content types:      [Reels / Carousels / Static posts / GBP posts / Shorts]
Posts per week:     [frequency]
Monthly post count: [total]
Primary pillar:     [which pillar gets the most content here]
Key format:         [most important format for this platform this month]
Trend slots:        [which trend(s) are assigned to this platform]
```

Reference `platform-rules.md` for format specs and frequency guidance.

**Platform content priority for this campaign type:**
- New patient acquisition: GBP posts (always-on) + Instagram Reels
- High-revenue service: Instagram + Facebook (higher income demos)
- Seasonal: all platforms, with GBP getting a dedicated offer post
- Brand/trust: Instagram Reels (team) + Facebook (long-form)
- New service launch: all platforms; YouTube Shorts for searchable explainer

### Step 8 — Generate content idea bank
Produce 15–20 specific, actionable content ideas organized by pillar. Each idea must be:
- Specific enough that a writer can start immediately (not "educational content about implants" — instead "3-slide carousel: what happens at your first dental implant consultation — step by step")
- Grounded in the keyword clusters, content angles, or hook library (cite the source)
- Compliant with the banned phrases and approved-claims files

**Required format per idea:**
```
## Idea [N]: [Short descriptive title]
Pillar: [pillar name]
Platform: [primary platform]
Format: [Reel / Carousel / Static / GBP post / Short]
Hook: [opening line or visual hook — pull from hook-library.md or write one aligned with brand voice]
Body summary: [1–2 sentences describing the content]
CTA: [what action does this drive?]
Keyword tie-in: [from keyword-clusters.md — if applicable]
Trend tie-in: [from trend report — if applicable]
Language: English / Spanish / Both
Production notes: [anything special needed: doctor on camera, new footage, authorization required?]
```

Distribute ideas across all pillars according to the monthly mix from Step 5.
Include at least one idea per active platform.
Include at least 2 ideas with Spanish versions if bilingual flag is set.
Include at least 1 GBP post idea per week (4 per month minimum).

### Step 9 — Integrate keyword opportunities
From `keyword-clusters.md`, pull the top 5 keyword opportunities for this campaign month:

For each keyword, identify which content idea from Step 8 it belongs to and note how to use it:
- GBP posts: include keyword naturally in post body (GBP posts are Google-indexed)
- YouTube Shorts titles: use keyword in title for searchability
- Blog/long-form: use as primary topic keyword
- Caption hooks: use the patient-phrased version as a hook opener

```
Keyword: [term]
Monthly volume: [N]
Intent: [Informational / Consideration / Booking / Emergency]
Content idea assigned to: [Idea # from Step 8]
How to use: [specific instruction]
```

### Step 10 — Plan bilingual content (required if `bilingual: true`)
If bilingual flag is set in `brand-context.md`:

**Bilingual allocation:**
- A minimum of 20% of monthly content should have a Spanish version or be Spanish-first
- Spanish-first content performs better in Miami-Dade than translated-English content
- Prioritize Spanish for: Local Community pillar, Trust pillar (team introductions), Service Demand for high-priority services

**For each bilingual content idea, specify:**
```
Idea [N] — Spanish version:
Hook (Spanish): [from hook-library.md Spanish section or brand-voice-guide.md Spanish voice profile]
Caption approach: [Spanish-first (write in Spanish, translate to English) / Bilingual (both in one post) / Separate posts]
Hashtags: [from hashtag-sets.md Spanish sets]
Review note: [flag for native-speaker review before publishing]
```

**Usted vs. tú:**
Use the register documented in `brand-voice-guide.md`. Do not mix registers within the same campaign.

### Step 11 — Set compliance boundaries
Establish the campaign-specific compliance guardrails that `compliance-qa-reviewer` will enforce:

**Claims permitted this campaign:**
List only claims that are in `approved-claims.md` AND are relevant to this campaign's primary service and offer.

**Claims requiring dentist sign-off before production:**
List any claims that are not yet approved but the campaign would benefit from. Flag for client confirmation before the content creator produces copy using them.

**Platform-specific restrictions for this campaign:**
Reference `platform-rules.md` and any campaign-specific notes:
- Is before/after content planned? → Requires HIPAA authorization documentation
- Is the offer being boosted on Meta? → Meta health advertising policy review required
- Are patient testimonials referenced? → Authorization required

**PHI risk areas specific to this campaign:**
Any content type in the idea bank that carries PHI risk (patient photos, testimonials, case studies) must be flagged here.

### Step 12 — Write production timeline
Work backward from the campaign start date to assign production milestones:

```
Campaign period: [start] → [end]

Production timeline:
[Campaign start - 14 days]: Campaign brief approved by client/account lead
[Campaign start - 10 days]: All content ideas finalized; content creator begins
[Campaign start - 7 days]:  First batch of copy drafted and in compliance review
[Campaign start - 5 days]:  Creative briefs written; visual production begins
[Campaign start - 3 days]:  Compliance gate cleared; final package assembled
[Campaign start - 1 day]:   Client review and approval
[Campaign start]:            First content scheduled/posted
```

For Priority trend content: compress timeline — draft within 48 hours of trend identified.

### Step 13 — Assemble the complete campaign brief
Fill `_templates/campaign-brief.md` with all decisions from Steps 2–12. Then append the extended sections below the template.

Save to: `social/calendars/{client-slug}/{YYYY-MM-DD}_{client-slug}_{campaign-month}-campaign-brief_v1.md`

**Extended sections to append after the template:**

```markdown
---

## Extended: Content Idea Bank
[All 15–20 ideas from Step 8 in full format]

---

## Extended: Content Volume Plan

| Platform | Posts/week | Monthly total | Primary format |
|---|---|---|---|
| Instagram | N | N | Reels |
| Facebook | N | N | Long-form posts |
| TikTok | N | N | Short video |
| YouTube Shorts | N | N | Explainer Shorts |
| GBP | N | N | What's new / Offer |
| **Total** | | **N** | |

---

## Extended: Trend Integration Plan

| Trend | Score | Decision | Pillar slot | Platform | Format | Produce by |
|---|---|---|---|---|---|---|
| [name] | [N]/100 | Priority | [pillar] | [platform] | [format] | [date] |

---

## Extended: Keyword Targets
[Top 5 keywords from Step 9 with content idea assignments]

---

## Extended: Bilingual Content Plan
[Only if bilingual: true — all Spanish/bilingual content assignments from Step 10]

---

## Extended: Production Timeline
[Full timeline from Step 12]
```

End the complete file with: `**Status:** Draft — Needs review`

### Step 14 — Handoff notes
Append to `logs/decisions.md`:
```
## {date} — Campaign Brief Complete: {client-name} — {campaign month}
- Campaign type: [type]
- Primary service: [service]
- Primary persona target: [persona name]
- Offer: [offer text or "No offer this cycle"]
- Total content planned: [N posts across all platforms]
- Trend slots: [N Priority + N Meaningful trends integrated]
- Bilingual content: [N pieces with Spanish versions / Not applicable]
- Brief approved by: [pending / approved by name]
- Content production start date: [date]
```

## Output Files
```
social/calendars/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{campaign-month}-campaign-brief_v1.md

logs/
  decisions.md   ← handoff notes appended
```

## Quality Checklist
- [ ] All required inputs loaded before any decisions made
- [ ] Campaign type declared and documented
- [ ] Seasonal context checked against southeast-florida-market.md
- [ ] Primary service selected from service demand matrix (not invented)
- [ ] Patient segment selection tied to specific persona(s) from patient-personas.md
- [ ] Offer verified in approved-claims.md (or flagged as pending sign-off)
- [ ] CTA pulled from brand-voice-guide.md approved CTAs
- [ ] Pillar mix set per campaign type table with any justified adjustments noted
- [ ] Pillar percentages converted to post counts
- [ ] All Priority and Meaningful trends assigned to content slots
- [ ] Channel strategy defined for every active platform
- [ ] 15–20 content ideas in the idea bank with full required format
- [ ] At least 1 GBP post idea per week (4 per month minimum)
- [ ] Top 5 keyword opportunities assigned to specific content ideas
- [ ] Bilingual plan complete (required if bilingual: true)
- [ ] Compliance boundaries documented: permitted claims, sign-off required, PHI risks
- [ ] Production timeline written with all milestone dates
- [ ] Complete brief file saved to `social/calendars/{client-slug}/`
- [ ] Handoff notes written to `logs/decisions.md`

## Failure Modes
- **No verified offers available** — proceed without a promotional offer; shift pillar mix away from Promotional; flag for client to confirm an offer for next cycle
- **Trend report not available** — proceed without trend integration; note in brief; flag trend-detection-scoring to run before content production begins
- **No keyword-clusters.md available** — proceed without keyword integration; content ideas rely on content-angles.md instead; note the gap
- **Primary service demand matrix is tied** — select the service that has the best competitive whitespace score from competitors.md
- **Bilingual flag is true but no Spanish voice profile exists** — flag for brand-voice-normalizer to complete the Spanish profile before brief is finalized; do not produce bilingual content without it
- **Client requests a service that is not in the top 3 demand priorities** — include it but note in the brief that it is client-directed and may underperform against data-driven priorities
- **Production timeline is compressed (< 7 days to campaign start)** — reduce content idea bank to 10 ideas; prioritize Priority trends and highest-demand service; flag compressed timeline in handoff notes

## Dependencies
- `market-research` (must run first — provides market-context, personas, content-angles)
- `keyword-research` (must run first — provides keyword-clusters)
- `brand-voice-normalizer` (must run first — provides brand-voice-guide)
- `competitive-intel-scraper` (must run first — provides competitors.md)
- `social-media-mining` (must run first — provides hook-library, hashtag-sets)
- `trend-detection-scoring` (must run first — provides Priority/Meaningful trends)
- `_context/global/content-pillars.md`
- `_context/global/platform-rules.md`
- `_context/global/southeast-florida-market.md`
- `_context/global/compliance-rules.md`
- `_templates/campaign-brief.md`

**Feeds into:** `social-copy`, `short-form-video-script`, `lead-magnet`, `lp-builder`, `ad-creative`, `social-creative`, `content-creator` agent, `compliance-qa-reviewer` agent, content calendar build
