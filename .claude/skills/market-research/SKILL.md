# Skill: market-research

## Description
Researches the local dental market conditions, patient demand, competitive landscape, and audience context for a private dental practice in Southeast Florida. Produces a market brief, a ranked service demand matrix, 2–4 enriched patient personas, 10–15 pillar-tagged content angles, and a dedicated `market-context.md` file that all downstream skills and agents load before strategy or content work. Uses Firecrawl for web research and NotebookLM for deep synthesis when warranted.

## When to Use
- After `client-context-ingestion` and `brand-voice-normalizer` complete — before any campaign strategy or content production
- At the start of each quarterly content planning cycle
- When building a proposal for a prospective client
- When a campaign targets a new service or patient segment the practice hasn't marketed before
- When local market conditions shift (new competitor opens, DSO enters the market, local news event affects patient behavior)

Run after `client-context-ingestion`. Run before `competitive-intel-scraper`, `trend-detection-scoring`, and `campaign-brief`.

## Required Inputs

| Input | Priority | Source |
|---|---|---|
| `_context/clients/{client-slug}/brand-context.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/service-offers.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/competitors.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/doctor-bios.md` | Required | client-context-ingestion |
| `_context/global/southeast-florida-market.md` | Required | Global context |
| `_context/global/dental-service-taxonomy.md` | Required | Global context |
| `_context/global/content-pillars.md` | Required | Global context |
| Practice city, county, and service area radius | Required | From brand-context.md |
| Primary services to prioritize for this research | Required | From service-offers.md |
| Bilingual flag | Required | Check brand-context.md for `bilingual: true` |

## NotebookLM Trigger Conditions
Use NotebookLM (Step 4) when any of the following are true:
- This is a proposal for a prospective client (high-stakes research)
- The campaign targets a clinically complex service (implants, oral surgery, orthodontics)
- The competitive landscape has more than 10 local competitors within 5 miles
- The practice serves a specialized patient segment (pediatric, geriatric, fear-based)
- Research depth from Firecrawl alone is insufficient to answer the Campaign Strategist's questions

Skip NotebookLM and use Firecrawl only for: routine quarterly refreshes on established clients with well-understood markets.

## Process

### Step 1 — Define research scope
Before any research begins, write a research scope statement and save it to:
`research/market/{client-slug}/research-scope-{YYYY-MM-DD}.md`

Include:
- Practice name, location(s), and service area radius
- Services prioritized for this research cycle and why
- Patient segments to understand
- Specific questions the Campaign Strategist needs answered
- Research freshness: date this scope was written + recommended refresh date (default: 90 days)
- NotebookLM: yes / no, with reason

This file stays in `research/market/{client-slug}/` — never in `research/proposals/`.

### Step 2 — English-language search landscape (Firecrawl)
Use Firecrawl to gather data from:

**Local search results:**
- "[priority service] [city]" — who ranks on page 1, what do their titles and descriptions say
- "[priority service] near me [county]" — for near-me intent signals
- "[practice name] reviews" — aggregate themes from public review snippets (themes only, no patient identity)
- Local dental directories: Healthgrades, Zocdoc, Yelp, 1-800-Dentist listings for the practice's service area

**Patient intent signals:**
- "People also ask" questions for each priority service in SE Florida
- FAQ and blog content from top-ranking local dental sites — what questions are patients asking?
- Review themes from the top 3–5 local competitors on Google and Yelp (extract themes only — no patient names or identifying details)

**Local context signals:**
- Local news mentioning dental health, access to care, or relevant community health topics in the county
- Local health department or community health report snapshots (public access only)

Save raw Firecrawl outputs to: `data/raw/firecrawl/{client-slug}/market-research-{YYYY-MM-DD}/`

### Step 3 — Spanish-language search landscape (Firecrawl)
**Required when `bilingual: true` in brand-context.md. Strongly recommended for all Miami-Dade clients.**

Run the same search queries in Spanish:
- "[servicio] dentista [ciudad]" (e.g., "implantes dentales Miami")
- "[servicio] cerca de mí [condado]" 
- "[nombre de práctica] opiniones" / "[nombre de práctica] reseñas"
- Spanish-language dental directories or listings active in SE Florida

Note which competitors appear in Spanish results that did not appear in English results — these represent a distinct competitive set the client may not be aware of.

Note any Spanish-language content gaps: services or patient concerns that no local practice addresses well in Spanish.

Save to: `data/raw/firecrawl/{client-slug}/market-research-{YYYY-MM-DD}/spanish-search/`

### Step 4 — NotebookLM deep synthesis (conditional — see trigger conditions above)
If triggered:

1. Write a source pack to `research/notebooklm/source-packs/{client-slug}-market-{YYYY-MM-DD}.md`:
   - List all sources (Firecrawl URLs, uploaded documents, relevant publications)
   - Write 8–12 research questions: patient pain points, service demand signals, competitive gaps, cultural considerations, seasonal opportunities
   - Specify output format needed: market brief sections, persona sketches, content angle seeds

2. After NotebookLM synthesis:
   - Export notes to `research/notebooklm/exported-notes/{client-slug}-market-{YYYY-MM-DD}.md`
   - Summarize key findings for Knowledge Librarian to convert into `research/notebooklm/research-digests/{client-slug}-market-digest.md`

### Step 5 — GBP competitive scan
For each competitor in `_context/clients/{client-slug}/competitors.md`, use the DataForSEO Business Data API to pull structured Google review data, then supplement with Firecrawl for GBP post and Q&A content.

**DataForSEO Business Data API — Google Reviews:**
```bash
AUTH=$(echo -n "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" | base64)
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

Run for the client practice and each competitor. From the structured response, extract:
- Star rating and total review count
- Review velocity: timestamps of the 10 most recent reviews
- Recurring language themes in review text (tone, experience descriptors — no PHI, no verbatim copying)
- Any attributes returned (Spanish-speaking staff, wheelchair accessible, etc.)

**Also use DataForSEO SERP Maps API to confirm local pack presence:**
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

Run for each priority service + city combination. Note which competitors appear in the local 3-pack — this is the highest-visibility position for local dental searches.

Supplement with Firecrawl for GBP post recency and Q&A content (not available via DataForSEO).

Summarize all findings into a GBP competitive snapshot table. Note which competitors are winning on GBP and which are neglecting it — neglect is an opportunity.

If DataForSEO credentials are unavailable, fall back to Firecrawl and manual GBP observation.

### Step 6 — Build service demand matrix
For each priority service, score demand across three dimensions (1–5 scale):

| Service | Search Demand | Review Frequency | Competitive Density | Priority Score |
|---|---|---|---|---|
| [Service A] | 1–5 | 1–5 | 1–5 (5 = least competitive) | avg |
| [Service B] | ... | ... | ... | ... |

**Search demand:** How many competitors rank for this term? How much "People also ask" activity exists?
**Review frequency:** How often is this service mentioned in public reviews across local practices?
**Competitive density:** How many local competitors actively market this service? (5 = few competitors = opportunity)

Rank all priority services by Priority Score. The top 3 become the recommended service focus for the campaign brief.

### Step 7 — Build patient personas
Create 2–4 patient personas grounded in the research — not invented. Base each on patterns observed in: review themes, FAQ questions, local demographics from `southeast-florida-market.md`, and the practice's existing patient context.

Each persona uses this template:

```
## Persona: [Fictional first name]

**Demographics:**
- Age range:
- Neighborhood / city in SE Florida:
- Occupation or life stage:
- Household income tier: Budget-conscious / Middle / Premium (affects treatment acceptance)
- Primary language: English / Spanish / Bilingual

**Dental profile:**
- Primary concern or trigger for seeking care:
- Services most likely to book:
- Time since last dental visit: Recent (< 1 year) / Lapsed (1–3 years) / Long-lapsed (3+ years)
- Insurance status: PPO / Medicaid / Self-pay / Uninsured
- Dental anxiety level: Low / Moderate / High

**Discovery and decision-making:**
- How they find a dentist: Google search / GBP reviews / Instagram / Friend referral / Insurance directory
- Decision-making style: Self-researcher (reads everything) / Social proof (needs reviews) / Convenience (books the first available) / Referred (trust is pre-established)
- Primary booking barrier: Cost / Fear / Time / Language / Trust / Past bad experience

**Content that resonates:**
- Hook that stops their scroll:
- Content pillar(s) that build trust with them:
- CTA style that converts them:
- Platform they're most likely to discover the practice on:
```

### Step 8 — Seasonal demand integration
From `_context/global/southeast-florida-market.md`, extract the seasonal demand calendar and match it to the priority services and patient personas identified in Steps 6–7.

For each quarter, note:
- Which services naturally peak in demand
- Which patient segments are most active
- Any SE Florida-specific seasonal hooks (snowbird season, hurricane prep, back to school, Hispanic Heritage Month, year-end insurance deadline)

Add a Seasonal Recommendations section to the market brief.

### Step 9 — Identify local content angles
Based on all research above, generate 10–15 local content angles. Each angle must follow this format:

```
[#] [Content Pillar] — [Specific angle title]
Why it works: [One sentence grounded in research — not invented]
Best format: [Post type: carousel / Reel / caption / GBP post / etc.]
Language: [English / Spanish / Both]
```

Example:
```
[1] Education — "What Miami's water quality means for your teeth"
Why it works: SE Florida water hardness and taste drive bottled-water reliance, reducing fluoride exposure — a real patient question surfaced in FAQ mining.
Best format: Instagram carousel (3–5 slides)
Language: Both
```

Avoid generic angles ("5 tips for healthy teeth") unless made specific to the local context. Every angle should feel like it could only belong to this practice in this market.

### Step 10 — Write the market brief
Compile all research into a structured market brief.

Save to: `research/market/{client-slug}/{client-slug}-market-brief-{YYYY-MM-DD}.md`

**Required sections:**

```markdown
# Market Brief — {Practice Name}
**Date:** {YYYY-MM-DD}
**Valid through:** {YYYY-MM-DD} (90 days default)
**Research scope:** {link to research-scope file}

## 1. Market Overview
Geography, competitive density, patient demographics summary. Cite sources.

## 2. English-Language Search Landscape
Who ranks, what they say, patient intent signals, FAQ themes. Source each claim.

## 3. Spanish-Language Search Landscape
(Include if bilingual: true or Miami-Dade client)
Spanish search competitors, gaps, patient language patterns.

## 4. GBP Competitive Snapshot
Table: competitor name, rating, review count, GBP activity level, notable attributes.

## 5. Service Demand Matrix
Table from Step 6. Highlight top 3 recommended service priorities.

## 6. Patient Personas
Full 2–4 personas from Step 7.

## 7. Seasonal Recommendations
Quarterly demand calendar matched to priority services and personas.

## 8. Local Content Angles
All 10–15 angles in the structured format from Step 9.

## 9. Research Gaps and Recommended Follow-Up
What data was unavailable, what questions remain, what the competitive-intel-scraper should prioritize.

## 10. Sources
All Firecrawl URLs, search queries used, review sources, and any publications cited.
```

End file with: `**Status:** Needs review`

### Step 11 — Write market-context.md (persistent client context)
Extract the most durable, referenceable insights from the market brief and write them to:
`_context/clients/{client-slug}/market-context.md`

This file is what downstream skills load — it stays lean and current. Include:
- Service demand ranking (top 3 priorities with one-line rationale)
- 2–4 persona summaries (name + one-sentence description)
- Top 5 content angles (most evergreen)
- Seasonal calendar snapshot
- Spanish market notes (if bilingual: true)
- Research freshness date and next refresh date

Do NOT write market insights into `brand-context.md` — that file is owned by `client-context-ingestion`.

### Step 12 — Write patient personas and content angles to context
- Full personas → `_context/clients/{client-slug}/patient-personas.md`
- All content angles → `_context/clients/{client-slug}/content-angles.md`

If these files already exist (quarterly refresh), append new data as a dated update block at the top. Do not overwrite.

### Step 13 — Handoff notes
Append to `logs/decisions.md`:
```
## {date} — Market Research Complete: {client-name}
- Top service priorities: [list top 3 from demand matrix]
- Key persona insights: [one sentence per persona]
- Most promising content angles: [top 3]
- Recommended competitive-intel-scraper focus: [specific competitors to prioritize]
- Research refresh due: {date}
```

## Output Files
```
data/raw/firecrawl/{client-slug}/market-research-{YYYY-MM-DD}/
  (raw Firecrawl outputs — English and Spanish)

research/market/{client-slug}/
  research-scope-{YYYY-MM-DD}.md
  {client-slug}-market-brief-{YYYY-MM-DD}.md

research/notebooklm/
  source-packs/{client-slug}-market-{YYYY-MM-DD}.md       (if NotebookLM used)
  exported-notes/{client-slug}-market-{YYYY-MM-DD}.md      (if NotebookLM used)
  research-digests/{client-slug}-market-digest.md           (if NotebookLM used)

_context/clients/{client-slug}/
  market-context.md      ← new dedicated file; do not write into brand-context.md
  patient-personas.md
  content-angles.md

logs/
  decisions.md           ← handoff notes appended
```

## Quality Checklist
- [ ] Research scope written and saved before any research begins
- [ ] NotebookLM trigger condition evaluated; decision documented in scope file
- [ ] English-language search landscape researched via Firecrawl (≥ 3 source types)
- [ ] Spanish-language search landscape researched (required for bilingual: true clients; recommended for all Miami-Dade clients)
- [ ] GBP competitive snapshot table complete with ≥ 3 competitors
- [ ] Service demand matrix built and top 3 priorities ranked
- [ ] 2–4 patient personas written using full template (no fields left blank)
- [ ] Seasonal demand calendar integrated into brief
- [ ] 10–15 content angles in structured format (pillar-tagged, format-tagged, language-tagged)
- [ ] All research claims have source citations
- [ ] No patient identity extracted from reviews — themes only
- [ ] Market brief saved to `research/market/{client-slug}/`
- [ ] `market-context.md` written to `_context/clients/{client-slug}/` (not into brand-context.md)
- [ ] `patient-personas.md` and `content-angles.md` written or updated
- [ ] Research freshness date and refresh date documented
- [ ] Handoff notes written to `logs/decisions.md`

## Failure Modes
- **Firecrawl blocked by a site** — note as unavailable; use cached search snippets or manual summary; cite limitation in brief
- **Insufficient local review data** — broaden to county level or adjacent services; note data limitation in brief
- **No Spanish search data available** — note gap; recommend client provide Spanish-speaking staff input; flag for follow-up
- **GBP data unavailable for competitors** — manually note which competitors could not be assessed; check if they have a GBP listing at all
- **Market is saturated (high competitive density across all services)** — shift service demand matrix to content differentiation: voice, format, language, platform — where is the whitespace?
- **Persona research yields insufficient local data** — use `southeast-florida-market.md` demographic profiles as baseline; mark personas as "Market-inferred — validate with client"
- **Research goes stale mid-campaign** — re-run Steps 2–3 only; update market-context.md with a dated refresh block; do not rewrite the full brief

## Dependencies
- DataForSEO (for Google Reviews + local pack — `DATAFORSEO_LOGIN`, `DATAFORSEO_PASSWORD` env vars)
- Firecrawl (primary — website content, GBP posts/Q&A, web research) or Crawl4AI (fallback: `crwl <url> -o markdown`)
- NotebookLM (for deep synthesis — conditional)
- `client-context-ingestion` (must run first)
- `_context/global/southeast-florida-market.md`
- `_context/global/dental-service-taxonomy.md`
- `_context/global/content-pillars.md`

**Feeds into:** `competitive-intel-scraper`, `trend-detection-scoring`, `campaign-brief`, `keyword-research`, `market-researcher` agent
