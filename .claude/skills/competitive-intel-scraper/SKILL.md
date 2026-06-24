# Skill: competitive-intel-scraper

## Description
Collects, normalizes, and synthesizes public competitive intelligence for private dental practices in Southeast Florida. Combines DataForSEO (search rankings, domain metrics, local pack, GBP data), Firecrawl (website content and offers), and Apify (public social profiles) into a structured competitor matrix, a positioning map, and a content gap report. Runs in two modes: **Initial Full Audit** (comprehensive, run once at onboarding) and **Weekly Monitoring Refresh** (lightweight, ongoing signal tracking).

All outputs are for internal strategic use only. Competitive intelligence informs content differentiation but is never used to make comparative claims in published content — ADA advertising guidelines prohibit direct comparisons.

## When to Use
- **Initial Full Audit:** After `client-context-ingestion` and `market-research` complete; before the first `campaign-brief`
- **Weekly Monitoring Refresh:** Every Monday before the weekly trend report cycle
- When a new competitor is identified or a known competitor significantly changes their strategy
- When the Campaign Strategist needs to know which content angles competitors are not covering

## Operating Mode
Declare the mode at the start of each run:

```
MODE: Initial Full Audit
```
or
```
MODE: Weekly Monitoring Refresh
```

The full process below is organized by mode. Steps marked **[FULL]** run in Initial Full Audit only. Steps marked **[BOTH]** run in both modes. Steps marked **[REFRESH]** are lightweight equivalents for the weekly cycle.

## DataForSEO Authentication
```bash
AUTH=$(echo -n "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" | base64)
# Use: -H "Authorization: Basic $AUTH"
```

## Required Inputs

| Input | Priority | Source |
|---|---|---|
| `_context/clients/{client-slug}/competitors.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/brand-context.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/market-context.md` | Required | market-research |
| `_context/clients/{client-slug}/keyword-clusters.md` | Required (Full Audit) | keyword-research |
| `_context/global/dental-service-taxonomy.md` | Required | Global context |
| `_context/global/platform-rules.md` | Required | Global context |
| `_templates/competitor-matrix.csv` | Required | Template |
| Previous competitive intel brief | Required (Refresh only) | `research/competitive-intel/{client-slug}/` |

## ─────────────────────────────────────────
## INITIAL FULL AUDIT
## ─────────────────────────────────────────

### Step 1 [FULL] — Define scope and competitor list
Write a scope statement and save to:
`research/competitive-intel/{client-slug}/audit-scope-{YYYY-MM-DD}.md`

Include:
- Priority services being analyzed (top 3 from `keyword-clusters.md`)
- Starting competitor list (from `competitors.md`) — typically 5–10 practices
- Search queries to use for SERP competitor discovery (Step 2)
- Audit date and next scheduled refresh

**Competitor list rules:**
- Include only private independent practices (no DSO chains like Aspen or Heartland — they operate on different economics and are not the relevant competitive set)
- Include practices within a realistic patient travel radius: 3–5 miles urban (Miami, Brickell), 5–10 miles suburban (Coral Gables, Weston), 10–15 miles rural/exurban (Homestead, Jupiter)
- Aim for 5–10 competitors total

### Step 2 [FULL] — Discover SERP competitors (DataForSEO Labs)
Use the top priority keyword combinations from `keyword-clusters.md` to discover which domains are competing in search results — some of these may not be on the initial competitor list.

```bash
curl -s -X POST "https://api.dataforseo.com/v3/dataforseo_labs/google/serp_competitors/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keywords": [
      "dental implants miami",
      "cosmetic dentist coral gables",
      "family dentist miami"
    ],
    "location_name": "Miami,Florida,United States",
    "language_name": "English",
    "limit": 20
  }]'
```

From the response:
- Extract all domains that appear in the top 10 organic results for these keywords
- Filter for dental practice websites (exclude directories like Healthgrades, Yelp, WebMD)
- Add any new practices not already on the competitor list
- Note average position per domain across all queries — practices with the lowest average position are the most direct search competitors

Save raw response to: `data/raw/dataforseo/{client-slug}/competitive-intel-{YYYY-MM-DD}/serp-competitors.json`

### Step 3 [FULL] — Domain rank overview per competitor (DataForSEO Labs)
For each competitor domain, pull domain-level SEO metrics.

```bash
curl -s -X POST "https://api.dataforseo.com/v3/dataforseo_labs/google/domain_rank_overview/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "target": "competitor-domain.com",
    "location_name": "Miami,Florida,United States",
    "language_name": "English"
  }]'
```

Run separately for each competitor domain. Extract:
- **Rank score** — DataForSEO's domain authority metric (higher = more established in search)
- **Organic keywords count** — total number of keywords the domain ranks for in top 100
- **Organic traffic estimate** — estimated monthly organic visits
- **Paid keywords count** — are they running Google Ads? (>0 = yes)

Run also for the client's own domain to establish a comparison baseline.

Save to: `data/raw/dataforseo/{client-slug}/competitive-intel-{YYYY-MM-DD}/domain-ranks/`

### Step 4 [FULL] — Ranked keywords per competitor (DataForSEO Labs)
For the top 3 competitors by rank score, pull the full list of keywords they rank for to identify keyword gaps.

```bash
curl -s -X POST "https://api.dataforseo.com/v3/dataforseo_labs/google/ranked_keywords/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "target": "competitor-domain.com",
    "location_name": "Miami,Florida,United States",
    "language_name": "English",
    "limit": 100,
    "filters": [["keyword_data.keyword_info.search_volume", ">", 50]]
  }]'
```

Compare each competitor's ranked keywords against the client's `keyword-clusters.md`:
- Keywords the competitor ranks for that the client does not → **gap to close**
- Keywords the client ranks for that the competitor does not → **client advantage to defend**
- High-volume keywords neither ranks for → **shared opportunity**

Save to: `data/raw/dataforseo/{client-slug}/competitive-intel-{YYYY-MM-DD}/ranked-keywords/`

### Step 5 [FULL] — Backlink profile comparison (DataForSEO Backlinks API)
Pull a backlink overview for the client and top 3 competitors. Backlinks are a strong proxy for domain authority and local citation health (critical for local pack rankings).

```bash
curl -s -X POST "https://api.dataforseo.com/v3/backlinks/overview/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "target": "competitor-domain.com",
    "mode": "domain"
  }]'
```

Extract per domain:
- Total backlinks count
- Referring domains count
- Domain rank of top 5 referring domains
- Presence of key local dental citations (Healthgrades, Zocdoc, Yelp, 1-800-Dentist, local chamber of commerce)

A practice with many referring domains from local citation sources is well-positioned for local pack rankings. Missing from key directories is a quick-win recommendation for the client.

Save to: `data/raw/dataforseo/{client-slug}/competitive-intel-{YYYY-MM-DD}/backlinks/`

### Step 6 [BOTH] — Local pack analysis per priority service (DataForSEO SERP Maps)
For each priority service + city combination, determine which practices appear in the local 3-pack.

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

Run for each priority service + the practice's primary city. Record per competitor:
- Local pack position (1, 2, 3, or "not in pack")
- Whether their GBP listing has photos, posts, and Q&A active
- Distance from the search location center

The local pack is the highest-converting search placement for dental practices. A competitor consistently appearing in positions 1–3 for your priority services is a direct threat regardless of their social media presence.

Save to: `data/raw/dataforseo/{client-slug}/competitive-intel-{YYYY-MM-DD}/local-pack/`

### Step 7 [BOTH] — GBP data and review mining (DataForSEO Business Data API)
For each competitor, pull structured Google review data.

```bash
curl -s -X POST "https://api.dataforseo.com/v3/business_data/google/reviews/task_post" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keyword": "Competitor Practice Name Miami FL",
    "location_name": "Miami,Florida,United States",
    "language_name": "English",
    "depth": 100,
    "sort_by": "newest"
  }]'
```

From the response extract per competitor:
- **Rating** — current star rating
- **Review count** — total reviews
- **Review velocity** — calculate approximate reviews/month from the most recent 20 review dates
- **Recurring praise themes** — what patients compliment most (in aggregate, no PHI)
- **Recurring complaint themes** — what patients criticize (opportunity signals for the client)
- **Spanish review presence** — are there Spanish-language reviews? (signals bilingual patient base)
- **Practice response rate** — are they responding to reviews?

**PHI rule:** Extract only vocabulary themes and aggregate patterns. Never copy review text that could identify a patient. Never record reviewer names or identifiable details.

Save to: `data/raw/dataforseo/{client-slug}/competitive-intel-{YYYY-MM-DD}/reviews/`

### Step 8 [FULL] — Website content crawl (Firecrawl)
For each competitor, crawl their website to understand their content positioning and offers.

Extract per competitor:
**Homepage:**
- Tagline and hero copy (what is their #1 positioning message?)
- Primary CTA (what action are they pushing?)
- Featured services on homepage (what do they prioritize?)
- Any offer or promotion visible without clicking

**Service pages:**
- Full list of services offered (cross-reference with `dental-service-taxonomy.md`)
- How they describe each priority service (clinical vs. patient-friendly language?)
- Any pricing signals (ranges, starting from, financing)

**Technology and trust signals:**
- Online booking integration (which platform?)
- Chat widget or text-to-book
- Awards, accreditations, or certifications displayed
- Before/after gallery (with or without clear authorization notice)

**Blog/content:**
- Does the blog exist and is it active?
- Last post date
- Topics covered (educational vs. promotional vs. local)

Save to: `data/raw/firecrawl/{client-slug}/competitive-intel-{YYYY-MM-DD}/`

### Step 9 [BOTH] — Social profile scan (Apify)
Use Apify public scrapers to collect social media signals for each competitor.

**Per platform, collect (public data only):**

| Platform | Data to collect | Apify Actor type |
|---|---|---|
| Instagram | Follower count, total posts, last 12 post types, avg engagement estimate, bio link | Instagram Profile Scraper |
| Facebook | Page likes/followers, last 6 post types, response rate visible | Facebook Pages Scraper |
| TikTok | Follower count, total videos, last 10 video themes | TikTok Profile Scraper |
| YouTube | Subscriber count, total videos, last 5 video titles | YouTube Channel Scraper |

**Content format analysis (from visible posts):**
Classify each competitor's recent content by format and theme:
- Format mix: Reels / carousels / static posts / GBP posts / YouTube Shorts
- Theme mix: Educational / Trust / Service Demand / Local / Team / Promotional
- Hook patterns: What opening lines or visual hooks do they lead with?
- Offer patterns: New patient specials, free consultations, financing CTAs
- Posting frequency: Posts per week per platform

**Note:** Apify Actor IDs vary — use the current Actor for each platform from the Apify Store. Always verify the Actor scrapes public data only. Never use login-required Actors.

Save raw Apify exports to: `data/raw/apify/{client-slug}/competitive-intel-{YYYY-MM-DD}/`

### Step 10 [FULL] — Synthesize competitive positioning matrix
Combine all data from Steps 2–9 into a structured positioning summary per competitor.

**Positioning framework — score each competitor on 5 dimensions:**

| Dimension | Signals | Score (1–5) |
|---|---|---|
| **Search presence** | Domain rank score, organic keywords, local pack position | 1 = weak / 5 = dominant |
| **GBP strength** | Rating, review count, review velocity, response rate | 1 = neglected / 5 = active and high-rated |
| **Content quality** | Format variety, posting frequency, engagement level | 1 = minimal / 5 = high-output |
| **Offer clarity** | Clear CTA, promotions, pricing signals visible | 1 = no offer visible / 5 = compelling offer front-and-center |
| **Audience fit** | Language match, service mix match, segment targeting | 1 = poor fit / 5 = directly competing for same patient |

**Competitive threat level:**
- Total score 20–25 → **High threat** — direct, capable competitor
- Total score 12–19 → **Medium threat** — competitive in some dimensions
- Total score 5–11 → **Low threat** — weak presence; monitor but do not prioritize

### Step 11 [FULL] — Content gap analysis
Based on all collected data, identify where competitors are weak or absent:

**Format gaps:** Content formats no competitor is using effectively
**Topic gaps:** Service areas or patient concerns no competitor addresses in content
**Platform gaps:** Platforms with low competitor activity (opportunity for client to own)
**Language gaps:** Spanish content that's missing or poorly executed (translated, not native)
**Local gaps:** Neighborhoods or community angles no competitor is covering
**Funnel gaps:** Intent stages competitors skip (e.g., everyone posts educational content but nobody runs strong conversion posts)

Format gaps section as:
```markdown
## Content Gap: [Gap name]
**Evidence:** [what was observed across competitors]
**Opportunity:** [what the client can produce to fill this gap]
**Priority:** High / Medium / Low
**Recommended format:** [specific content type]
```

### Step 12 [FULL] — Update competitor matrix and context files
**Update `_templates/competitor-matrix.csv`:**
Add or update one row per competitor with all data collected. Include the new DataForSEO columns: GBP Review Velocity, Local Pack Position, Domain Rank Score, Organic Keywords Count, Backlink Profile.

**Update `_context/clients/{client-slug}/competitors.md`:**
Enrich each competitor entry with:
- Domain rank score and organic keywords count
- Local pack position for primary service
- GBP rating + review velocity
- Positioning summary (one sentence)
- Competitive threat level
- Top content gap this competitor has that the client can exploit

**Write the competitive intelligence brief:**
Save to: `research/competitive-intel/{client-slug}/{client-slug}-competitive-intel-{YYYY-MM-DD}.md`

**Required sections:**
1. Scope and methodology
2. Competitive landscape overview (who's dominant, who's weak)
3. Local pack analysis (who owns the 3-pack for each priority service)
4. Search presence comparison table (all competitors, domain rank, keyword count)
5. GBP health comparison table (rating, reviews, velocity, response rate)
6. Social presence comparison table (platforms active, followers, frequency, formats)
7. Competitor positioning summaries (one per competitor, 3–5 sentences)
8. Content gap analysis (all gaps identified in Step 11)
9. Client recommendations (top 5 strategic opportunities based on gaps found)
10. Sources (all DataForSEO API calls, Firecrawl URLs, Apify exports used)

End with: `**Status:** Needs review`

### Step 13 [FULL] — Handoff notes
Append to `logs/decisions.md`:
```
## {date} — Competitive Intelligence Audit Complete: {client-name}
- Competitors audited: [N], High threat: [N], Medium: [N], Low: [N]
- Local pack winner for primary service: [competitor name + position]
- Biggest content gap identified: [gap name]
- Client's strongest competitive advantage: [one sentence]
- Recommended immediate action: [one specific tactic]
- Next full audit due: {date, 90 days}
- Weekly refresh starts: {next Monday}
```

## ─────────────────────────────────────────
## WEEKLY MONITORING REFRESH
## ─────────────────────────────────────────

### Step R1 [REFRESH] — Load baseline
Load the most recent competitive intelligence brief from:
`research/competitive-intel/{client-slug}/`

Note the date of last audit. The refresh checks for changes since that date.

### Step R2 [REFRESH] — Local pack position check (DataForSEO SERP Maps)
Run Step 6 API calls for all priority service + city combinations.

Compare results to previous audit:
- Did any competitor enter or leave the local 3-pack?
- Did client's position change?
- Are there new practices appearing in results?

Flag any position change as: **Improved** / **Declined** / **New entrant** / **Exited pack**

### Step R3 [REFRESH] — GBP monitoring (DataForSEO Business Data API)
Run Step 7 API calls for all competitors using `sort_by: "newest"` and limit to last 30 days.

Flag significant changes:
- Rating drop of 0.2+ stars → competitor vulnerability
- Sudden review velocity spike → competitor may have run a review campaign
- New negative review themes emerging → note for client's differentiation
- Competitor started responding to reviews (previously wasn't) → they're getting more active

### Step R4 [REFRESH] — Social monitoring (Apify)
Run Step 9 Apify scans for all competitors.

Compare against previous week's data:
- New content formats being tested by high-threat competitors
- New offers or promotions launched
- Significant follower growth or drop
- New platform launched (competitor joined TikTok, started YouTube)

### Step R5 [REFRESH] — Write monitoring delta report
Save to: `research/competitive-intel/{client-slug}/monitoring/monitoring-{YYYY-MM-DD}.md`

Structure:
```markdown
# Competitive Monitoring Report — {client-name} — {date}

## Changes This Week

### Local Pack
[Any position changes]

### GBP Activity
[Any rating, review, or response changes]

### Social Activity
[Any notable new content, formats, or offers from competitors]

## No Significant Changes
[List areas with no notable activity]

## Recommended Actions
[Any tactical responses to competitor activity this week]

**Status:** Needs review
```

Append a one-line summary to `logs/decisions.md`:
```
{date} — Competitive Monitoring: {client-name} — [key change noted or "No significant changes"]
```

## Output Files

**Initial Full Audit:**
```
data/raw/dataforseo/{client-slug}/competitive-intel-{YYYY-MM-DD}/
  serp-competitors.json
  domain-ranks/         ← one file per competitor domain
  ranked-keywords/      ← one file per top 3 competitor domains
  backlinks/            ← one file per competitor domain
  local-pack/           ← one file per service + city query
  reviews/              ← one file per competitor

data/raw/firecrawl/{client-slug}/competitive-intel-{YYYY-MM-DD}/
  {competitor-slug}-website.md    ← one per competitor

data/raw/apify/{client-slug}/competitive-intel-{YYYY-MM-DD}/
  {competitor-slug}-instagram.json
  {competitor-slug}-facebook.json
  {competitor-slug}-tiktok.json   (if active)
  {competitor-slug}-youtube.json  (if active)

_templates/competitor-matrix.csv   ← updated with new data

_context/clients/{client-slug}/
  competitors.md                   ← enriched with DataForSEO metrics

research/competitive-intel/{client-slug}/
  audit-scope-{YYYY-MM-DD}.md
  {client-slug}-competitive-intel-{YYYY-MM-DD}.md

logs/
  decisions.md                     ← handoff notes appended
```

**Weekly Monitoring Refresh:**
```
data/raw/dataforseo/{client-slug}/competitive-intel-{YYYY-MM-DD}/
  local-pack/    ← refreshed
  reviews/       ← refreshed (newest 30 days only)

data/raw/apify/{client-slug}/competitive-intel-{YYYY-MM-DD}/
  (refreshed social scans)

research/competitive-intel/{client-slug}/monitoring/
  monitoring-{YYYY-MM-DD}.md

logs/
  decisions.md   ← one-line weekly summary appended
```

## Quality Checklist

**Initial Full Audit:**
- [ ] Scope file written before any API calls
- [ ] SERP competitors discovered via DataForSEO Labs — new competitors added to list
- [ ] Domain rank overview pulled for all competitors + client (for comparison)
- [ ] Ranked keywords pulled for top 3 competitors by rank score
- [ ] Backlink overview pulled for top 3 competitors + client
- [ ] Local pack analysis run for all priority service + city combinations
- [ ] GBP data pulled for all competitors (rating, review count, velocity, themes)
- [ ] Website crawled for all competitors via Firecrawl
- [ ] Social scan run for all competitors via Apify (public data only)
- [ ] Competitive threat level assigned to every competitor
- [ ] Content gaps documented with priority and recommended format
- [ ] Competitor matrix CSV updated with all new columns
- [ ] `competitors.md` enriched with DataForSEO metrics and positioning summaries
- [ ] Full competitive intelligence brief written and saved
- [ ] PHI check passed — no patient names or identifying details in any output
- [ ] Handoff notes written to `logs/decisions.md`

**Weekly Monitoring Refresh:**
- [ ] Loaded most recent full audit brief as baseline
- [ ] Local pack positions re-checked for all priority services
- [ ] GBP review changes checked for all competitors (last 30 days)
- [ ] Social activity checked for all competitors
- [ ] Delta report written and saved to `monitoring/`
- [ ] `logs/decisions.md` updated with one-line weekly summary

## Failure Modes
- **DataForSEO returns no results for a competitor domain** — domain may be very new or has minimal SEO footprint; note as "minimal search presence" in matrix; this itself is a competitive signal
- **Firecrawl blocked by competitor website** — note as unavailable; use search snippet and GBP listing as proxy for content summary
- **Apify Actor fails for a platform** — note platform as "scan unavailable" in that competitor's row; manually observe what is visible in the browser and document
- **Competitor has no GBP listing** — critical finding; note as major gap in their local strategy; this is a significant client opportunity
- **New DSO competitor found in SERP results** — include in monitoring but flag as DSO; strategy against DSOs is different (differentiate on relationship, not compete on price); flag for Campaign Strategist
- **More than 10 competitors identified** — tier them: audit top 5 by competitive threat score in full detail; monitor remaining via lightweight GBP + local pack checks only

## Compliance Note
Competitive intelligence collected by this skill is for internal strategic use only.

- **Never publish** comparative claims in marketing content ("We have more reviews than X")
- **Never name competitors** in client-facing content unless they are industry references (e.g., "Unlike corporate dental chains…" framing is permitted in general terms; naming a specific practice is not)
- **Never use competitor review text** verbatim in any output
- **ADA advertising guidelines** prohibit comparative claims that disparage other dental practices — all competitive insights must stay internal

## Dependencies
- DataForSEO (`DATAFORSEO_LOGIN`, `DATAFORSEO_PASSWORD` — Labs, SERP Maps, Business Data, Backlinks APIs)
- Firecrawl (primary — competitor website content crawl) or Crawl4AI (fallback: `crwl <url> -o markdown` — preferred for high-volume competitor audits to reduce API cost)
- Apify (`APIFY_API_TOKEN` — public social profile data only; Firecrawl and Crawl4AI cannot replace this)
- `client-context-ingestion` (must run first)
- `market-research` (must run first — provides market-context.md)
- `keyword-research` (required for Full Audit — provides keyword-clusters.md for SERP competitor queries)
- `_context/clients/{client-slug}/competitors.md`
- `_templates/competitor-matrix.csv`

**Feeds into:** `campaign-brief`, `trend-detection-scoring`, `social-copy` (via content gap awareness), `market-researcher` agent
