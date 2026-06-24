# Skill: market-research

## Description
Researches the local dental market conditions, patient demand, audience context, and competitive landscape for a private dental practice in Southeast Florida. Produces a market brief, patient personas, and local content angles using Firecrawl for web research and NotebookLM for synthesis. Feeds the Campaign Strategist and Trend Scout.

## When to Use
- When onboarding a new client practice (run after `client-context-ingestion`)
- At the start of each quarterly content planning cycle
- When building a proposal for a prospective client
- When a campaign is targeting a new service or patient segment the practice hasn't marketed before
- When local market conditions appear to have shifted (new competitor, new DSO entry, local news event)

## Required Inputs
- `_context/clients/{client}/brand-context.md` (practice location, services, target patients)
- `_context/global/southeast-florida-market.md` (market context)
- `_context/global/dental-service-taxonomy.md` (service reference)
- Practice city, county, and surrounding service area
- Primary services to research (from `_context/clients/{client}/service-offers.md`)
- Competitor list (from `_context/clients/{client}/competitors.md`)

## Process

### Step 1 — Define research scope
Before any research begins, write a one-paragraph scope statement:
- What practice and location are we researching?
- What services are the priority for this research?
- What patient segments are we trying to understand?
- What questions does the Campaign Strategist need answered?

Save to `research/proposals/{client-slug}/research-scope.md` (or `research/market/` for ongoing work).

### Step 2 — Local market research with Firecrawl
Use Firecrawl to gather data from:

**Local search landscape:**
- Google search results for "[priority service] [city]" — who ranks, what do they say
- Local dental directories (Healthgrades, Zocdoc, Yelp, 1-800-Dentist listings in the area)
- Google Maps results for the practice's location radius

**Patient behavior signals:**
- "People also ask" questions for key dental services in SE Florida
- Review themes from public Google and Yelp reviews of top local practices (do not copy or attribute — extract themes only)
- Local dental blog or FAQ content that signals common patient questions

**Local context:**
- Local news articles about dental health or access in Miami-Dade, Broward, or Palm Beach
- Community health reports or local health department publications
- Local events, cultural moments, or community needs relevant to dental health

### Step 3 — NotebookLM research packet (when applicable)
For deeper research, create a source pack for NotebookLM:

1. Write a `source-pack.md` in `research/notebooklm/source-packs/{client-slug}-market-research.md`:
   - List all sources to analyze (URLs, uploaded documents)
   - Write 5–10 research questions to explore in NotebookLM
   - Note the output format needed (patient personas, market brief, topic clusters)

2. After NotebookLM synthesis:
   - Export notes to `research/notebooklm/exported-notes/{client-slug}-market-{date}.md`
   - Knowledge Librarian converts to `research/notebooklm/research-digests/{client-slug}-market-digest.md`

### Step 4 — Build patient personas
Create 2–4 patient personas for the practice's target market. Each persona includes:

```
Persona name: (fictional, representative name)
Age range:
Location: (city/neighborhood in SE Florida)
Language preference: English / Spanish / Bilingual
Primary dental concern:
Services most likely to book:
Barriers to booking: (fear, cost, time, trust, language)
How they find a dentist: (Google search, referral, Instagram, GBP)
What they read in a social caption: (what hook would stop their scroll)
Content that builds trust with them:
```

### Step 5 — Identify local content angles
Based on the research, list 10–15 content angles specific to this practice's location and patient base:
- Local cultural moments or seasonal hooks
- Community-specific dental concerns (e.g., high sugar-beverage consumption, sports injuries, language access)
- Local competitor gaps (services or content types no competitor is doing well)
- Local search topics patients are actively researching

### Step 6 — Write the market brief
Using the research, write a structured market brief:

**Sections:**
1. Market overview (geography, competitive density, patient demographics)
2. Patient demand signals (what services are most searched, most reviewed, most competitive)
3. Competitive landscape summary (who the top 3–5 competitors are and how they are positioned)
4. Patient personas (2–4 from Step 4)
5. Local content angles (from Step 5)
6. Recommended service priorities for this market
7. Research gaps and recommended follow-up

Save to `research/market/{client-slug}-market-brief-{date}.md`.

### Step 7 — Update project memory
- Curated insights → `_context/clients/{client}/brand-context.md` (add market section)
- Patient personas → `_context/clients/{client}/patient-personas.md`
- Local content angles → `_context/clients/{client}/content-angles.md`
- Handoff to Campaign Strategist and Trend Scout

## Output Files
- `research/market/{client-slug}-market-brief-{date}.md`
- `_context/clients/{client}/patient-personas.md`
- `_context/clients/{client}/content-angles.md`
- `research/notebooklm/source-packs/{client-slug}-market-research.md` (if NotebookLM used)
- `research/notebooklm/research-digests/{client-slug}-market-digest.md` (if NotebookLM used)

## Quality Checklist
- [ ] Research scope defined before any research begins
- [ ] Firecrawl used for at least 3 source types (search results, directories, reviews)
- [ ] No patient identity extracted from reviews — themes only
- [ ] NotebookLM source pack created if research depth warrants it
- [ ] 2–4 patient personas written with local specificity
- [ ] 10+ local content angles identified
- [ ] Market brief structured and saved to `research/market/`
- [ ] Patient personas and content angles saved to `_context/clients/{client}/`
- [ ] Research gaps flagged for follow-up

## Failure Modes
- **Firecrawl blocked by a site** — note the site as unavailable; use cached search snippets or manual summary
- **Insufficient local review data** — broaden to county level or adjacent services; note data limitation in brief
- **NotebookLM sources are too thin** — add industry reports, dental association publications, or SE Florida health data as sources
- **Market is too saturated to identify gaps** — shift focus to content differentiation (voice, format, language) rather than service gaps
- **Practice is in a niche geography** — research at county level and adjust local angles accordingly

## Dependencies
- Firecrawl (for web research)
- NotebookLM (for deep synthesis — optional but recommended)
- `_context/global/southeast-florida-market.md`
- `_context/global/dental-service-taxonomy.md`
- `_context/clients/{client}/brand-context.md`
- `_context/clients/{client}/service-offers.md`
- `_context/clients/{client}/competitors.md`
