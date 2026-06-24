# Skill: keyword-research

## Description
Identifies how SE Florida dental patients search for care using DataForSEO as the primary intelligence source, and translates that data into actionable content strategy. Produces topic clusters, a patient FAQ map, a service-intent matrix, local keyword combinations, and seasonal demand data — all grounded in real search volume, PAA results, and competitor ranking signals. Feeds campaign planning, social copy, video scripts, lead magnets, landing pages, GBP posts, and YouTube Shorts titles.

## When to Use
- After `market-research` completes — keyword research builds on the service demand matrix and patient personas already identified
- Before `campaign-brief`, `lead-magnet`, `lp-builder`, and `short-form-video-script` when those skills need search-aligned topics
- When launching a campaign targeting a new service — keyword research defines how patients phrase that need
- When writing GBP posts or YouTube Shorts titles that need search-indexed language
- Quarterly: refresh alongside the market research cycle

Run after `market-research`. Run before `campaign-brief`, `lead-magnet`, `lp-builder`.

## DataForSEO Authentication
All API calls use HTTP Basic auth. Credentials are stored in environment variables — never hardcoded.

```bash
AUTH=$(echo -n "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" | base64)
# Use in every curl call: -H "Authorization: Basic $AUTH"
```

Base URL: `https://api.dataforseo.com/v3/`

If `DATAFORSEO_LOGIN` or `DATAFORSEO_PASSWORD` are not set, see the Fallback section at the end of this skill before proceeding.

## Required Inputs

| Input | Priority | Source |
|---|---|---|
| `_context/clients/{client-slug}/market-context.md` | Required | market-research output |
| `_context/clients/{client-slug}/service-offers.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/brand-context.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/patient-personas.md` | Required | market-research output |
| `_context/global/dental-service-taxonomy.md` | Required | Global context |
| `_context/global/southeast-florida-market.md` | Required | Global context |
| `_context/global/content-pillars.md` | Required | Global context |
| Service demand matrix (top 3 priorities) | Required | From market-context.md |
| Practice city, county, service area | Required | From brand-context.md |
| Bilingual flag | Required | Check brand-context.md for `bilingual: true` |
| Google Search Console export | Optional | From client — incorporate if provided |

## Process

### Step 1 — Define research scope
Write a one-paragraph scope statement before any API calls:
- Which services are being researched (top 3 from service demand matrix)
- Which patient segments are the search target (from patient personas)
- Which outputs are needed by downstream skills
- Language(s): English only / English + Spanish
- Location targeting: practice city + county (used in all API calls)

Save to: `seo/keyword-research/{client-slug}/research-scope-{YYYY-MM-DD}.md`

### Step 2 — Seed keyword expansion (DataForSEO: Keywords For Keywords)
For each priority service, build a seed keyword list using patient-facing terms from `dental-service-taxonomy.md`. Then expand each seed into related terms.

```bash
curl -s -X POST "https://api.dataforseo.com/v3/keywords_data/google_ads/keywords_for_keywords/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keywords": ["dental implants", "tooth implant", "implant dentist"],
    "location_name": "Miami,Florida,United States",
    "language_name": "English",
    "date_from": "2025-06-01",
    "date_to": "2026-06-01"
  }]'
```

Run for each priority service. Use the practice's primary city as `location_name`.
Save raw JSON responses to: `data/raw/dataforseo/{client-slug}/keyword-research-{YYYY-MM-DD}/seeds/`

Collect all returned keywords. Remove irrelevant terms (non-dental, non-local, competitors' brand names). The resulting list becomes the input for Step 3.

### Step 3 — Search volume and competition data (DataForSEO: Search Volume)
Pull search volume, CPC, and competition index for all keywords collected in Step 2, plus geographic modifier combinations (service + city/neighborhood).

```bash
curl -s -X POST "https://api.dataforseo.com/v3/keywords_data/google_ads/search_volume/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keywords": [
      "dental implants miami",
      "dental implants coral gables",
      "implant dentist near me",
      "tooth implant cost miami",
      "best dental implants miami"
    ],
    "location_name": "Miami,Florida,United States",
    "language_name": "English"
  }]'
```

**Geographic modifiers to test per service:**
Rotate through: practice city, county name, nearest 2–3 major neighborhoods, and "near me" variants. For a Coral Gables practice: "Coral Gables," "Miami," "South Miami," "Coconut Grove," "Miami-Dade."

For each keyword, record:
- Monthly search volume
- Competition index (0–1, where 1 = highest competition)
- CPC (indicates commercial intent — higher CPC = higher booking intent)

Save to: `data/raw/dataforseo/{client-slug}/keyword-research-{YYYY-MM-DD}/volumes/`

### Step 4 — SERP analysis with PAA (DataForSEO: SERP Organic)
For the top 10 highest-volume keywords per service, pull full SERP data including People Also Ask boxes, featured snippets, and organic results.

```bash
curl -s -X POST "https://api.dataforseo.com/v3/serp/google/organic/live/advanced" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keyword": "dental implants miami",
    "location_name": "Miami,Florida,United States",
    "language_name": "English",
    "depth": 10,
    "se_domain": "google.com"
  }]'
```

From each SERP result, extract:
- **PAA questions** — every "People also ask" question returned (these are real patient questions)
- **Featured snippet owner** — which competitor holds the featured snippet (if any)
- **Top 10 organic URLs** — which domains rank and what their page titles are
- **Knowledge panel / local pack presence** — is there a local pack for this query?

Save raw SERP data to: `data/raw/dataforseo/{client-slug}/keyword-research-{YYYY-MM-DD}/serp/`

### Step 5 — Local pack analysis (DataForSEO: Google Maps SERP)
For each priority service + location combination, pull the Google Maps local pack to see which practices appear in the 3-pack.

```bash
curl -s -X POST "https://api.dataforseo.com/v3/serp/google/maps/live/advanced" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keyword": "dental implants miami",
    "location_name": "Miami,Florida,United States",
    "language_name": "English"
  }]'
```

For each Maps result, record:
- Practice name and address
- Star rating and review count
- Whether the client practice appears (and at what position)
- Which competitors consistently appear in the local pack across multiple queries

Local pack presence is the single highest-leverage SEO signal for dental practices. Practices in the 3-pack for "[service] [city]" capture the majority of local booking intent.

Save to: `data/raw/dataforseo/{client-slug}/keyword-research-{YYYY-MM-DD}/maps/`

### Step 6 — Keyword ideas and related terms (DataForSEO Labs)
Expand topic coverage using DataForSEO Labs to find keyword clusters the seed expansion may have missed.

```bash
# Related Keywords
curl -s -X POST "https://api.dataforseo.com/v3/dataforseo_labs/google/related_keywords/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keyword": "dental implants",
    "location_name": "Miami,Florida,United States",
    "language_name": "English",
    "depth": 2,
    "limit": 50
  }]'

# Keyword Ideas
curl -s -X POST "https://api.dataforseo.com/v3/dataforseo_labs/google/keyword_ideas/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keywords": ["dental implants", "implant dentist miami"],
    "location_name": "Miami,Florida,United States",
    "language_name": "English",
    "limit": 100
  }]'
```

Run for each priority service. Filter results for dental relevance and minimum monthly search volume (drop anything under 10 searches/month for a local market — likely zero local demand).

Save to: `data/raw/dataforseo/{client-slug}/keyword-research-{YYYY-MM-DD}/labs/`

### Step 7 — Historical and seasonal search data (DataForSEO Labs + Google Trends)
Pull historical search volume to identify seasonal demand patterns for each priority service.

```bash
# Historical Search Volume (DataForSEO Labs)
curl -s -X POST "https://api.dataforseo.com/v3/dataforseo_labs/google/historical_search_volume/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keywords": ["dental implants miami", "teeth whitening miami", "invisalign miami"],
    "location_name": "Miami,Florida,United States",
    "language_name": "English"
  }]'

# Google Trends (12-month trend for the region)
curl -s -X POST "https://api.dataforseo.com/v3/keywords_data/google_trends/explore/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keywords": ["dental implants", "veneers", "teeth whitening"],
    "location_name": "Florida,United States",
    "date_from": "2025-06-01",
    "date_to": "2026-06-01",
    "type": "web"
  }]'
```

Cross-reference results with the seasonal calendar in `_context/global/southeast-florida-market.md`. Note where DataForSEO confirms or contradicts the expected seasonal patterns — data trumps assumptions.

Save to: `data/raw/dataforseo/{client-slug}/keyword-research-{YYYY-MM-DD}/trends/`

### Step 8 — Google Reviews language mining (DataForSEO: Business Data API)
For the client practice and top 3–5 competitors, pull Google Reviews to extract the vocabulary patients actually use.

```bash
curl -s -X POST "https://api.dataforseo.com/v3/business_data/google/reviews/task_post" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keyword": "Brickell Smiles Miami FL",
    "location_name": "Miami,Florida,United States",
    "language_name": "English",
    "depth": 100
  }]'
```

From review text, extract vocabulary patterns — not individual reviews, not patient identities:
- Words patients use to describe services they received ("my implant," "the whitening," "braces")
- Words patients use to describe the experience ("painless," "gentle," "explained everything")
- Words patients use to describe finding the practice ("found them on Google," "saw their Instagram")

These patient-vocabulary signals inform hook writing more than any keyword tool can.

Save to: `data/raw/dataforseo/{client-slug}/keyword-research-{YYYY-MM-DD}/reviews/`

### Step 9 — Competitor content structure (Firecrawl)
DataForSEO tells you what keywords competitors rank for. Firecrawl tells you how they structure their content to rank for them. Both are needed.

For each of the top 3–5 local competitors identified in `market-context.md`, use Firecrawl to crawl their website and extract:
- Page titles (`<title>` tags) — which keywords they optimized each page for
- H1 headings on service pages — their primary topic focus per service
- FAQ section content — what patient questions they've already answered
- Meta descriptions — how they pitch each service in 155 characters
- Internal link anchor text — their service hierarchy signals

**What to flag:**
- Services with generic or missing page titles → content gap (no competitor has optimized this)
- FAQ questions no competitor answers → patient need with no good answer in the market
- Services with heavily optimized, specific page titles → high-competition keywords

Save to: `data/raw/firecrawl/{client-slug}/keyword-research-{YYYY-MM-DD}/competitor-pages/`

### Step 10 — Spanish keyword research (DataForSEO — required if `bilingual: true`)
Repeat Steps 2–3 in Spanish. Use `language_name: "Spanish"` in all API calls. Do not translate English keywords word-for-word — use natural Latin American Spanish dental terms.

```bash
curl -s -X POST "https://api.dataforseo.com/v3/keywords_data/google_ads/search_volume/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keywords": [
      "implantes dentales miami",
      "dentista implantes miami",
      "cuánto cuestan implantes dentales miami",
      "dentista que habla español miami",
      "implantes dentales precio miami"
    ],
    "location_name": "Miami,Florida,United States",
    "language_name": "Spanish"
  }]'
```

Also run Google Trends in Spanish to compare trend velocity between English and Spanish searches for the same service — divergence can indicate untapped Spanish-market opportunity.

**Zero-volume Spanish keywords are not dead ends.** No monthly search volume for a Spanish dental keyword in SE Florida often means no Spanish content exists for that query — the client can own that position with the first quality piece.

Save to: `data/raw/dataforseo/{client-slug}/keyword-research-{YYYY-MM-DD}/spanish/`

### Step 11 — Google Search Console integration (if provided)
If the client provides Google Search Console access:
1. Export: Performance → Queries — filter to last 90 days, sort by Impressions descending
2. Export: Performance → Queries — filter to last 90 days, sort by Clicks descending
3. Save to `data/raw/platform-exports/{client-slug}/search-console-{YYYY-MM-DD}.csv`

GSC data is the highest-confidence signal in this entire skill because it shows actual queries driving real traffic to the client's own site. Treat any GSC keyword as confirmed demand — do not require DataForSEO volume to validate it.

### Step 12 — Classify all findings by intent level
Consolidate all keywords, PAA questions, and review vocabulary from Steps 2–10. Classify each item:

| Intent Level | Patient mindset | Volume/CPC signal | Content type |
|---|---|---|---|
| **Informational** | "I want to learn" | Higher volume, lower CPC | Education pillar: carousels, Reels, blog |
| **Consideration** | "I'm comparing options" | Medium volume, medium CPC | Service Demand: comparison posts, explainers |
| **Local/Booking** | "I'm ready to find a provider" | Lower volume, higher CPC | GBP posts, landing pages, conversion captions |
| **Emergency** | "I need help now" | Lower volume, very high CPC | Always-on GBP posts, emergency content |
| **Anxiety** | "I'm scared, need reassurance" | Medium volume, low CPC | Trust pillar, sedation content, team personality |

For each item, record:
```
Keyword/Question: [text]
Language: English / Spanish
Monthly volume: [N] (or "< 10" or "not available")
CPC: $[N] (or "not available")
Competition: [0.0–1.0]
Intent: Informational / Consideration / Local-Booking / Emergency / Anxiety
Service: [from taxonomy]
Source: DataForSEO volume / DataForSEO PAA / DataForSEO Labs / Firecrawl FAQ / GSC / Review language
Content gap: Yes / No (and if No: competitor who answers it well)
```

### Step 13 — Build topic clusters
Organize classified items into 4–6 topic clusters — one per priority service or patient concern.

**Required format per cluster:**

```markdown
## Cluster: [Topic Name]
**Service category:** [from dental-service-taxonomy.md]
**Primary patient segment:** [persona name from patient-personas.md]
**Content pillar fit:** [primary pillar]
**Seasonal peak:** [month(s) from historical data]

### Informational (volume → content opportunity)
| Keyword / Question | Volume | CPC | Gap |
|---|---|---|---|
| "what is a dental implant" | 1,900/mo | $3.20 | No – Wikipedia ranks |
| "are dental implants safe" | 880/mo | $4.10 | Yes – no local dental content |

### Consideration
| Keyword / Question | Volume | CPC | Gap |
|---|---|---|---|
| "dental implants vs dentures" | 590/mo | $6.80 | Yes |

### Local / Booking
| Keyword / Question | Volume | CPC | Gap |
|---|---|---|---|
| "dental implants miami" | 1,300/mo | $12.40 | No – 4 competitors rank |
| "dental implants coral gables" | 210/mo | $14.20 | Yes |

### Emergency / Urgency (if applicable)
...

### Patient vocabulary (from reviews)
Key words patients use: [comma-separated]

### Top content opportunity
[One sentence: the single best angle this cluster supports, grounded in a data gap]
```

### Step 14 — Build the patient FAQ map
Compile all PAA questions from Step 4, Firecrawl FAQ content from Step 9, and review vocabulary from Step 8 into a prioritized FAQ map.

**Priority order:**
1. PAA questions with high volume + content gap (no competitor answers it well) — highest priority
2. PAA questions with high volume + competitor answer (worth a better answer)
3. Firecrawl FAQ content no competitor covers
4. Review vocabulary questions (patients asking in their own words)

**Required format per entry:**
```markdown
**Q: [Question as patients phrase it]**
Volume signal: [search volume or "PAA — volume not direct" or "Review language"]
CPC: $[N] (intent proxy)
Intent: [Informational / Consideration / Local-Booking / Emergency / Anxiety]
Service: [service category]
Language: English / Spanish / Both
Best format: [Instagram carousel slide / Reel answer / GBP post / blog section / caption hook]
Content gap: Yes — [reason] / No — [competitor who answers it]
Priority: High / Medium / Low
```

Target 20–30 FAQ entries. Include at least 5 Spanish-language entries if bilingual flag is set.

### Step 15 — Build the service-intent matrix
For each of the top 3 priority services, map the full patient search journey with real volume data:

```markdown
## [Service Name] — Intent Matrix

| Stage | Example keyword | Volume | CPC | Best content response |
|---|---|---|---|---|
| Awareness | "are dental implants painful" | 1,300/mo | $2.10 | Reel: honest answer |
| Consideration | "dental implants vs dentures pros cons" | 590/mo | $6.80 | Comparison carousel |
| Local search | "dental implants miami" | 1,300/mo | $12.40 | GBP post + landing page |
| Booking intent | "free dental implant consultation miami" | 320/mo | $18.50 | Conversion caption + GBP offer |
| Emergency | "lost tooth dentist miami" | 140/mo | $22.00 | GBP always-on post |
```

High CPC at the Local/Booking and Emergency stages confirms these terms drive actual patient decisions — prioritize them in GBP posts and landing pages.

### Step 16 — Build local keyword combinations
Generate a reference list of geographic + service combinations for GBP posts, YouTube Shorts titles, and landing page copy.

**English combinations (minimum 20):**
```
[service] [city]
[service] [neighborhood]
[service] near [landmark or area]
[service] [county]
best [service] [city]
affordable [service] [city]          (only if practice positions on value)
[service] dentist [city]
[service] with [insurance] [city]    (for each insurance type accepted)
[service] open saturday [city]       (if applicable)
emergency [service] [city]           (for emergency services)
```

**Spanish combinations (minimum 10, if bilingual: true):**
```
dentista [servicio] [ciudad]
[servicio] en español [ciudad]
[servicio] dentista hispano [ciudad]
dentista bilingüe [ciudad]
[servicio] con seguro [tipo] [ciudad]
```

Validate all combinations against DataForSEO volume data from Step 3. Flag any combination with 0 monthly volume — it may not be worth producing content around.

### Step 17 — Write the keyword research file
Compile all findings into a single structured document.

Save to: `seo/keyword-research/{client-slug}/{client-slug}-keyword-research-{YYYY-MM-DD}.md`

**Required sections:**
1. Research scope and methodology
2. Data sources used (DataForSEO endpoints called, Firecrawl pages crawled, GSC if provided)
3. Topic clusters (all 4–6, full format with volume data)
4. Patient FAQ map (all 20–30 entries)
5. Service-intent matrix (top 3 services, with real volume + CPC)
6. Local keyword combinations (English + Spanish, volume-validated)
7. Patient vocabulary reference (key words from review language)
8. Google Search Console highlights (if data was provided)
9. Top 5 content gaps (highest-opportunity keywords with no strong competitor answer)
10. Seasonal demand summary (from historical volume data + Google Trends)
11. All API calls made (endpoint + parameters, for reproducibility)

End file with: `**Status:** Needs review`
**Valid through:** {90 days from research date}

### Step 18 — Write keyword-clusters.md (persistent client context)
Extract the most durable findings and write to:
`_context/clients/{client-slug}/keyword-clusters.md`

Keep this file under 400 lines. Include:
- Top 3 clusters with primary keywords, volumes, and patient vocabulary
- Top 10 FAQ entries (High priority only)
- Top 5 local keyword combinations per service (with volumes)
- Top 5 content gaps
- Seasonal demand peaks per service
- Research freshness date and refresh date

This is what `campaign-brief`, `social-copy`, `short-form-video-script`, `lp-builder`, and `lead-magnet` load. Keep it lean and scannable.

### Step 19 — Handoff notes
Append to `logs/decisions.md`:
```
## {date} — Keyword Research Complete: {client-name}
- Services researched: [list]
- Highest-volume opportunity: [keyword + volume]
- Top content gaps: [top 3]
- Best local keyword: [term + volume + CPC]
- Spanish research: completed / not applicable / incomplete (reason)
- Local pack position for priority service: [position or "not ranking"]
- Refresh due: {date}
```

## Output Files
```
data/raw/dataforseo/{client-slug}/keyword-research-{YYYY-MM-DD}/
  seeds/          ← Keywords For Keywords raw responses
  volumes/        ← Search Volume raw responses
  serp/           ← SERP Organic raw responses (PAA, top 10)
  maps/           ← Google Maps local pack raw responses
  labs/           ← DataForSEO Labs raw responses
  trends/         ← Historical volume + Google Trends raw responses
  reviews/        ← Business Data Google Reviews raw responses
  spanish/        ← Spanish keyword data raw responses

data/raw/firecrawl/{client-slug}/keyword-research-{YYYY-MM-DD}/
  competitor-pages/   ← Competitor content structure (Firecrawl)

data/raw/platform-exports/{client-slug}/
  search-console-{YYYY-MM-DD}.csv   ← GSC export (if provided)

seo/keyword-research/{client-slug}/
  research-scope-{YYYY-MM-DD}.md
  {client-slug}-keyword-research-{YYYY-MM-DD}.md

_context/clients/{client-slug}/
  keyword-clusters.md    ← context-ready reference for downstream skills

logs/
  decisions.md           ← handoff notes appended
```

## Quality Checklist
- [ ] Research scope written before any API calls
- [ ] DataForSEO credentials confirmed available before starting
- [ ] Seed keywords expanded via Keywords For Keywords for all top 3 services
- [ ] Search volume pulled for all priority keywords + geographic modifier combinations
- [ ] SERP data pulled for top 10 keywords per service (PAA extracted)
- [ ] Google Maps local pack pulled for all priority service + city combinations
- [ ] DataForSEO Labs related keywords and keyword ideas run for all services
- [ ] Historical search volume + Google Trends pulled for seasonal analysis
- [ ] Google Reviews pulled for client + ≥ 3 competitors (patient vocabulary extracted)
- [ ] Firecrawl competitor content structure analyzed for ≥ 3 competitors
- [ ] Spanish keyword research completed (required if bilingual: true)
- [ ] GSC data incorporated if provided by client
- [ ] All keywords classified by intent level with volume + CPC recorded
- [ ] 4–6 topic clusters built with volume data in required format
- [ ] 20–30 FAQ entries in patient FAQ map with priority tags
- [ ] Service-intent matrix built for top 3 services with real volume + CPC
- [ ] ≥ 20 English + ≥ 10 Spanish (if bilingual) local keyword combinations, volume-validated
- [ ] Top 5 content gaps identified with supporting evidence
- [ ] Seasonal demand peaks documented per service
- [ ] Full keyword research file saved to `seo/keyword-research/{client-slug}/`
- [ ] `keyword-clusters.md` written to `_context/clients/{client-slug}/`
- [ ] Research freshness date and 90-day refresh date documented
- [ ] Handoff notes written to `logs/decisions.md`

## Fallback: DataForSEO Unavailable
If `DATAFORSEO_LOGIN` or `DATAFORSEO_PASSWORD` env vars are not set:
1. Note the gap in the research scope file
2. Replace Steps 2–8 with proxy signal methods:
   - **Search volume proxy:** Count how many top-10 competitors have optimized pages for a keyword → more competitors = more demand
   - **PAA proxy:** Use Firecrawl to scrape Google search result pages and extract PAA boxes
   - **Local pack proxy:** Manually note which competitors appear in Maps results
   - **Review language:** Use Firecrawl to crawl public review pages
3. Mark all topic cluster volumes as "estimated — DataForSEO not available"
4. Flag in `logs/decisions.md` that keyword research used proxy methods and volume data is approximate
5. Recommend setting up DataForSEO credentials before the next refresh

## Failure Modes
- **DataForSEO rate limit hit** — space out API calls; use Task-based endpoints instead of Live for large batches
- **Zero volume for a geographic modifier** — the city modifier may be too narrow; try county-level or nearest major city
- **No Spanish PAA results** — content opportunity signal; note in clusters as "Spanish content gap"
- **Competitor websites block Firecrawl** — extract from DataForSEO SERP snippets (page titles + descriptions from organic results)
- **Client has no GSC access** — proceed without it; add to onboarding recommendations
- **DataForSEO returns no results for a dental keyword** — likely very low local volume; reprioritize toward higher-volume services

## Dependencies
- DataForSEO (primary — `DATAFORSEO_LOGIN`, `DATAFORSEO_PASSWORD` env vars)
- Firecrawl (competitor content structure — supplementary)
- `market-research` (must run first — provides service demand matrix and patient personas)
- `_context/global/dental-service-taxonomy.md`
- `_context/global/southeast-florida-market.md`
- `_context/global/content-pillars.md`
- `_context/clients/{client-slug}/market-context.md`
- `_context/clients/{client-slug}/patient-personas.md`

**Feeds into:** `campaign-brief`, `social-copy`, `short-form-video-script`, `lead-magnet`, `lp-builder`, GBP post production, YouTube Shorts titles, `seo/content-briefs/`
