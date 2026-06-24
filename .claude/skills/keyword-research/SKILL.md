# Skill: keyword-research

## Description
Identifies how SE Florida dental patients search for care and translates that search behavior into actionable content intelligence. Produces topic clusters, a patient FAQ map, a service-intent matrix, and local keyword combinations — all organized by content pillar and intent level. Works without dedicated SEO tools by mining "People also ask" data, competitor page structures, GBP Q&A, and patient review language. Feeds campaign planning, social copy, video scripts, lead magnets, landing pages, GBP posts, and YouTube Shorts titles.

## When to Use
- After `market-research` completes — keyword research builds on the service demand matrix and patient personas already identified
- Before `campaign-brief`, `lead-magnet`, `lp-builder`, and `short-form-video-script` when those skills need search-aligned topics
- When launching a campaign targeting a new service — keyword research defines how patients talk about that service
- When writing GBP posts or YouTube Shorts titles that need search-indexed language
- Quarterly: refresh the keyword clusters alongside the market research cycle

Run after `market-research`. Run before `campaign-brief`, `lead-magnet`, `lp-builder`.

## A Note on Tools
This project does not include a dedicated SEO tool (Ahrefs, SEMrush, Google Keyword Planner). This skill derives keyword intelligence from proxy signals that are available without those tools:

| Proxy Signal | What It Replaces | Source |
|---|---|---|
| "People also ask" density | Search volume proxy | Firecrawl / WebSearch |
| Competitor page title investment | Keyword competition signal | Firecrawl |
| Review language frequency | Patient vocabulary signal | market-research outputs |
| GBP Q&A content | Local intent signal | Firecrawl |
| Content gap analysis | Opportunity signal | Competitor audit |

**If the client provides Google Search Console access:** Extract the top 20 queries by impressions and top 20 by clicks. Incorporate these as high-confidence signals in Steps 2–3. Save the export to `data/raw/platform-exports/{client-slug}/search-console-{YYYY-MM-DD}.csv`.

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
Write a one-paragraph scope statement before any research begins:
- Which services are being researched (use top 3 from service demand matrix)
- Which patient segments are the search target (from patient personas)
- Which outputs are needed by downstream skills and why
- Language(s): English only / English + Spanish

Save to: `seo/keyword-research/{client-slug}/research-scope-{YYYY-MM-DD}.md`

### Step 2 — "People Also Ask" mining (Firecrawl + WebSearch)
For each priority service, run the following search queries and capture the "People also ask" (PAA) boxes:

**Search pattern for each service:**
```
"[patient-facing service name] [city]"
"[patient-facing service name] near me"
"[patient-facing service name] cost [city]"
"how much does [service] cost in [city]"
"best [service] [city]"
"[service] vs [alternative service]"
```

Use patient-facing terms from `dental-service-taxonomy.md`, not clinical terms.
Example: "dental implants Miami" not "osseointegrated implant Miami"

For each query, extract:
- The PAA questions that appear
- The featured snippet answer (if any) — note which competitor owns it
- The top 3 organic results and their page titles

**SE Florida geographic modifiers to test per service:**
Rotate through the practice's primary city plus its county seat and 2–3 nearest major neighborhoods. Example for a Coral Gables practice: "Coral Gables," "Miami," "Miami-Dade," "South Miami," "Coconut Grove."

Save raw PAA output to: `data/raw/firecrawl/{client-slug}/keyword-research-{YYYY-MM-DD}/paa-english/`

### Step 3 — Competitor page title and structure analysis (Firecrawl)
For each of the top 3–5 local competitors identified in `market-context.md`:

Crawl their website and extract:
- Page titles (HTML `<title>` tags) — these reveal which keywords they're targeting
- H1 headings on service pages — their primary content focus per service
- Meta descriptions — how they pitch each service in 155 characters
- Internal link anchor text on homepage and navigation — their service priority hierarchy
- Any FAQ section content — what questions they've chosen to answer

**What to look for:**
- Which services appear most prominently in page titles and H1s → high-competition keywords
- Which services have weak or generic page titles ("Services – ABC Dental") → content gaps
- Which questions appear in competitor FAQs → validated patient concerns
- Which questions no competitor FAQ answers → content whitespace opportunities

Save to: `data/raw/firecrawl/{client-slug}/keyword-research-{YYYY-MM-DD}/competitor-pages/`

### Step 4 — GBP Q&A mining (Firecrawl)
For the client's GBP and each competitor's GBP (from Step 2 of `market-research`):

Extract all visible Q&A entries:
- What questions are patients actually submitting?
- What answers exist (if any)?
- Which questions are unanswered — these represent active patient confusion

These are real, unfiltered patient questions — the highest-quality intent signal available without an SEO tool.

Compile all GBP Q&As into a single list: `data/raw/firecrawl/{client-slug}/keyword-research-{YYYY-MM-DD}/gbp-qa/`

### Step 5 — Review language extraction
From the market-research outputs already saved in `data/raw/firecrawl/{client-slug}/market-research-*/`, re-scan public review content (Google, Yelp, Healthgrades) for:

**Words and phrases patients use to describe services:**
- How do they describe a good experience? ("painless," "gentle," "explained everything")
- How do they describe what they came in for? ("needed a crown," "my implant," "got my teeth whitened")
- How do they describe finding the practice? ("found them on Google," "saw their Instagram," "my neighbor recommended")

**Negative language patterns:**
- Common complaints across competitors that the client can address in content

These are the actual words patients use when thinking about dental care — not marketing language, not clinical language. Content that mirrors this vocabulary converts better.

Do not copy review text verbatim. Extract vocabulary patterns and themes only.

### Step 6 — Spanish keyword research (required if `bilingual: true`; recommended for all Miami-Dade clients)
Repeat Steps 2–4 in Spanish:

**Spanish PAA queries per service:**
```
"[servicio en español] [ciudad]"
"[servicio] dentista [ciudad]"
"cuánto cuesta [servicio] en [ciudad]"
"[servicio] cerca de mí"
"dentista que habla español [ciudad]"
"[servicio] precio [ciudad]"
```

Use natural Latin American Spanish — not word-for-word translations of the English queries.

**Additional Spanish-specific queries:**
- "dentista bilingüe [ciudad]"
- "dentista hispano [ciudad]"
- "[servicio] con seguro [tipo de seguro] [ciudad]" (insurance-qualified searches)

Note: Spanish PAA boxes may not appear for every query in SE Florida — record which queries had PAA results and which did not. Absence of Spanish PAA often signals a content opportunity (no authoritative Spanish content exists yet).

Save to: `data/raw/firecrawl/{client-slug}/keyword-research-{YYYY-MM-DD}/paa-spanish/`

### Step 7 — Classify all findings by intent level
Take all queries, PAA questions, GBP Q&As, and review vocabulary gathered in Steps 2–6 and classify each by intent:

| Intent Level | Patient mindset | Content type it maps to |
|---|---|---|
| **Informational** | "I want to learn" — no immediate booking intent | Education pillar: carousels, Reels, blog |
| **Consideration** | "I'm comparing options or providers" | Service Demand pillar: comparison posts, explainers |
| **Local/Booking** | "I'm ready to find a provider near me" | GBP posts, landing pages, conversion captions |
| **Emergency** | "I need help now" | Emergency content, GBP always-on posts |
| **Anxiety** | "I'm scared and need reassurance" | Trust pillar, sedation content, team personality |

For each item, record:
```
Query/Question: [text]
Language: English / Spanish
Intent: Informational / Consideration / Local-Booking / Emergency / Anxiety
Service: [which service category from taxonomy]
Proxy signal strength: High (GBP Q&A or PAA) / Medium (competitor FAQ) / Low (inferred)
Content gap: Yes (no strong competitor answer exists) / No
```

### Step 8 — Build topic clusters
Organize all classified items into 4–6 topic clusters. Each cluster groups a service or patient concern with all its related search intent layers.

**Required format per cluster:**

```markdown
## Cluster: [Topic Name]
**Service category:** [from dental-service-taxonomy.md]
**Primary patient segment:** [persona name from patient-personas.md]
**Content pillar fit:** [primary pillar]

### Informational keywords / questions
- [question or term] — [proxy signal strength] — [content gap: Y/N]
- ...

### Consideration keywords / questions
- [question or term] — [proxy signal strength] — [content gap: Y/N]
- ...

### Local / Booking keywords
- [term with geographic modifier] — [proxy signal strength] — [content gap: Y/N]
- ...

### Emergency / Urgency keywords (if applicable)
- ...

### Patient vocabulary (from reviews)
Key words patients use: [comma-separated list]

### Top content opportunity
[One sentence: the single best content angle this cluster supports, grounded in a gap found in research]
```

Aim for 4–6 clusters covering the top priority services. Do not create clusters for services not in the practice's service-offers.md.

### Step 9 — Build the patient FAQ map
Compile all PAA questions, GBP Q&As, and competitor FAQ questions into a single prioritized FAQ map.

**Prioritization order:**
1. GBP Q&A questions (highest signal — real patient questions in the practice's market)
2. PAA questions with no strong competitor answer (content gap + validated search intent)
3. PAA questions with a strong competitor answer (competitive — worth a better answer)
4. Competitor FAQ questions not covered by PAA (medium signal)

**Required format per FAQ entry:**

```markdown
**Q: [Question as patients phrase it]**
Intent: [Informational / Consideration / Local-Booking / Emergency / Anxiety]
Service: [service category]
Language: English / Spanish / Both
Best format: [Instagram carousel slide / Reel answer / GBP post / blog section / caption hook]
Content gap: [Yes — no strong competitor answer / No — [competitor name] answers it well]
Priority: High / Medium / Low
```

Aim for 20–30 FAQ entries across all priority services. Include at least 5 Spanish-language FAQ entries if bilingual flag is set.

### Step 10 — Build the service-intent matrix
For each priority service, build a four-column intent progression showing how patients move from unaware to ready to book:

```markdown
## [Service Name] — Intent Matrix

| Stage | Example search or question | What they need | Content that serves them |
|---|---|---|---|
| Awareness | "are dental implants safe?" | Education, reassurance | Reel explainer, carousel |
| Consideration | "implants vs dentures pros and cons" | Honest comparison | Comparison carousel |
| Local search | "dental implants [city]" | Trust signals, location, price range | GBP post, landing page |
| Booking intent | "dental implants free consultation [city]" | Clear CTA, easy booking | Conversion caption, GBP offer |
| Emergency | "lost a tooth [city]" | Immediate contact info | GBP always-on post |
```

Build this matrix for each of the top 3 priority services.

### Step 11 — Build local keyword combinations
Generate a reference list of geographic + service keyword combinations for use in:
- GBP post body copy (search-indexed)
- YouTube Shorts titles (search-indexed)
- Landing page headlines
- Blog meta titles

**Format:**
```
[service] [city/neighborhood]
[service] near [landmark or area name]
[service] [county]
best [service] [city]
[service] dentist [city]
affordable [service] [city]           (only if practice positions on value)
[service] with [insurance type] [city] (if practice accepts that insurance)
dentista [servicio] [ciudad]           (Spanish — if bilingual: true)
```

Generate at minimum 20 English combinations and 10 Spanish combinations (if bilingual).

### Step 12 — Write the keyword research file
Compile all findings into a single structured document.

Save to: `seo/keyword-research/{client-slug}/{client-slug}-keyword-research-{YYYY-MM-DD}.md`

**Required sections:**
1. Research scope and methodology
2. Topic clusters (all 4–6, full format)
3. Patient FAQ map (all 20–30 entries)
4. Service-intent matrix (top 3 services)
5. Local keyword combinations (English + Spanish)
6. Patient vocabulary reference (key words from review language)
7. Google Search Console data (if provided)
8. Content gaps summary — top 5 opportunities where no competitor has strong content
9. Sources (all queries run, all competitor pages crawled, all GBP Q&As extracted)

End file with: `**Status:** Needs review`
**Valid through:** {90 days from research date}

### Step 13 — Write keyword-clusters.md (persistent client context)
Extract the most durable, referenceable intelligence from the research and write it to:
`_context/clients/{client-slug}/keyword-clusters.md`

Include only what downstream skills need to access quickly:
- Top 3 clusters with primary keywords and patient vocabulary per service
- Top 10 FAQ entries by priority (High only)
- Top 5 local keyword combinations per service
- Top 5 content gaps
- Research freshness date

This file is what `campaign-brief`, `social-copy`, `short-form-video-script`, `lp-builder`, and `lead-magnet` load. Keep it under 400 lines.

### Step 14 — Handoff notes
Append to `logs/decisions.md`:
```
## {date} — Keyword Research Complete: {client-name}
- Services researched: [list]
- Top content gaps identified: [top 3]
- Highest-priority FAQ entries: [top 3 questions]
- Best local keyword opportunity: [one term]
- Spanish research: completed / not applicable / incomplete (reason)
- Refresh due: {date}
```

## Output Files
```
data/raw/firecrawl/{client-slug}/keyword-research-{YYYY-MM-DD}/
  paa-english/            ← raw PAA captures
  paa-spanish/            ← raw Spanish PAA captures (if bilingual)
  competitor-pages/       ← competitor page title and structure data
  gbp-qa/                 ← GBP Q&A extracts

seo/keyword-research/{client-slug}/
  research-scope-{YYYY-MM-DD}.md
  {client-slug}-keyword-research-{YYYY-MM-DD}.md   ← full research document

_context/clients/{client-slug}/
  keyword-clusters.md    ← context-ready reference for downstream skills

logs/
  decisions.md           ← handoff notes appended
```

## Quality Checklist
- [ ] Research scope written before any research begins
- [ ] PAA mining completed for all top 3 priority services (English)
- [ ] Spanish PAA mining completed (required if bilingual: true)
- [ ] Competitor page titles and H1s extracted for ≥ 3 competitors
- [ ] GBP Q&A extracted for client and ≥ 3 competitors
- [ ] Review language vocabulary extracted from market-research outputs
- [ ] Google Search Console data incorporated if provided by client
- [ ] All findings classified by intent level (Informational / Consideration / Local-Booking / Emergency / Anxiety)
- [ ] 4–6 topic clusters built with full required format
- [ ] 20–30 FAQ entries in the patient FAQ map with format and priority tags
- [ ] Service-intent matrix built for top 3 services
- [ ] ≥ 20 English + ≥ 10 Spanish (if bilingual) local keyword combinations
- [ ] Top 5 content gaps documented
- [ ] Full keyword research file saved to `seo/keyword-research/{client-slug}/`
- [ ] `keyword-clusters.md` written to `_context/clients/{client-slug}/`
- [ ] Research freshness date and 90-day refresh date documented
- [ ] Handoff notes written to `logs/decisions.md`

## Failure Modes
- **Firecrawl cannot load Google search results** — use WebSearch directly for PAA queries; note that data may be less complete
- **No PAA results for a service** — this itself is a signal: either very low search volume or a content gap; note it and proceed with competitor FAQ and GBP Q&A as primary sources
- **Competitor websites block Firecrawl** — extract from Google search snippets only; note limitation
- **No GBP Q&A content available** — use competitor FAQ pages and PAA as primary patient intent sources; flag GBP Q&A gap in handoff notes
- **No Spanish PAA results** — note as content opportunity: this service has no authoritative Spanish content in the market; the client can own this space
- **Client has no Google Search Console access** — proceed without it; note as a recommendation to set it up
- **Services not in taxonomy** — do not create new taxonomy entries; describe using the closest matching patient-facing term and flag for taxonomy update

## Dependencies
- Firecrawl (for PAA, competitor pages, GBP Q&A)
- `market-research` (must run first — provides service demand matrix and patient personas)
- `_context/global/dental-service-taxonomy.md`
- `_context/global/southeast-florida-market.md`
- `_context/global/content-pillars.md`
- `_context/clients/{client-slug}/market-context.md`
- `_context/clients/{client-slug}/patient-personas.md`

**Feeds into:** `campaign-brief`, `social-copy`, `short-form-video-script`, `lead-magnet`, `lp-builder`, GBP post production, YouTube Shorts title writing, `seo/content-briefs/`
