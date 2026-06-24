# Skill: social-media-mining

## Description
Mines public social content across Instagram, TikTok, Facebook, and YouTube to extract creative intelligence: hook patterns, format performance signals, audience questions, hashtag data, and trending audio. Produces a reusable hook library and curated hashtag sets that `social-copy`, `short-form-video-script`, and `campaign-brief` consume directly. Run monthly before content calendar planning. Separate from `competitive-intel-scraper` — this skill mines CONTENT PATTERNS across the broader dental social space, not individual competitor strategies.

**Critical rule:** All outputs are patterns and vocabulary — never verbatim captions, never individual comment text, never patient-identifiable content. Mining is for creative inspiration, not copying.

## When to Use
- Monthly, before building the content calendar for the next month
- Before the first campaign brief for a new client (run alongside `market-research`)
- When the hook library or hashtag sets are stale (> 30 days old)
- When the Campaign Strategist requests fresh content angle intelligence
- When `trend-detection-scoring` needs social signal data for candidate evaluation

Run before `campaign-brief`. Run after `competitive-intel-scraper` (reuse its Apify exports where possible).

## Required Inputs

| Input | Priority | Source |
|---|---|---|
| `_context/clients/{client-slug}/brand-context.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/service-offers.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/market-context.md` | Required | market-research |
| `_context/global/content-pillars.md` | Required | Global context |
| `_context/global/platform-rules.md` | Required | Global context |
| `_context/global/dental-service-taxonomy.md` | Required | Global context |
| `_context/global/compliance-rules.md` | Required | Global context |
| Bilingual flag | Required | Check brand-context.md for `bilingual: true` |
| `data/raw/apify/{client-slug}/` | Preferred | Prior Apify exports from competitive-intel-scraper |

## Apify Setup

This skill uses Apify public scrapers. Credentials are stored in environment variables:
- `APIFY_API_TOKEN` — used for all Apify Actor runs via the Apify API

**Actors to use (verify current versions in the Apify Store — do not hardcode Actor IDs):**

| Target | Actor function | Store search term |
|---|---|---|
| Instagram hashtag posts | Hashtag post scraper | "Instagram Hashtag Scraper" |
| Instagram post comments | Comment scraper | "Instagram Comments Scraper" |
| TikTok hashtag videos | Hashtag video scraper | "TikTok Hashtag Scraper" |
| TikTok video comments | Comment scraper | "TikTok Comments Scraper" |
| YouTube search results | Video search scraper | "YouTube Search Scraper" |
| Facebook page posts | Public page post scraper | "Facebook Pages Scraper" |

**Scraping rules (mandatory):**
- Public content only — never logged-in, private, or restricted content
- Never scrape direct messages, stories accessible only to followers, or content behind a login wall
- Respect platform rate limits — run scrapes at Apify's recommended intervals
- Raw exports go to `data/raw/apify/{client-slug}/social-mining-{YYYY-MM-DD}/`

## Mining Scope

### Tier 1 — Local dental content (highest priority for SE Florida relevance)
Hashtags and accounts specific to SE Florida dental practices:
- Local hashtags: `#MiamiDentist` `#CoralGablesDentist` `#BocaRatonDentist` `#FortLauderdaleDentist` `#MiamiBocaDentist` `#SouthFlorida Dentist` `#SEFLDental`
- The client's own competitors (reuse exports from `competitive-intel-scraper`)

### Tier 2 — National high-performing dental content (format and hook inspiration)
High-engagement dental accounts nationally that can provide format and creative inspiration regardless of geography. Look for accounts with:
- Consistent posting (3x+ per week)
- High engagement rate (3%+ on Instagram, 5K+ views on TikTok Reels)
- Content in the client's priority service areas

Do not use these accounts' specific claims, patient content, or branded elements — mine only structure and patterns.

### Tier 3 — Adjacent lifestyle and health content (SE Florida angle)
Trending content in SE Florida lifestyle categories that could be bridged into dental:
- Beach/outdoor lifestyle
- Fitness and wellness
- Hispanic culture and community (if bilingual: true)
- Local food and dining (teeth/oral health connections)

### Tier 4 — Spanish-language dental content (required if `bilingual: true`)
Spanish-language hashtags for SE Florida dental:
- `#DentistaMiami` `#DentistaBocaRaton` `#DentistaHispano` `#SaludDental` `#ImplantesDentales`
- Spanish-language dental accounts with significant SE Florida / Latin American audience

## Process

### Step 1 — Define mining scope
Write a mining scope document before running any Apify jobs:
- Priority services for this mining cycle (top 3 from `market-context.md`)
- Platforms to mine (all active platforms in `brand-context.md`)
- Hashtags and accounts per tier (see above, customize for client's city)
- Languages: English only / English + Spanish
- Previous mining outputs to compare against (date of last run)

Save to: `research/social-mining/{client-slug}/mining-scope-{YYYY-MM-DD}.md`

### Step 2 — Configure and run Apify scrape jobs

**Instagram hashtag scrape (per priority service):**
Run for each priority service hashtag combination. Recommended parameters:
- Hashtags: 3–5 per run (local + service + general)
- Results per hashtag: 50–100 posts
- Filter: public posts only; minimum 1 image or video

Example hashtag sets per service:
```
Implants:   #DentalImplants #ImplantDentist #MiamiDentist
Whitening:  #TeethWhitening #ProfessionalWhitening #MiamiSmile
Invisalign: #Invisalign #ClearAligners #StraightTeeth #MiamiOrtho
General:    #DentalHealth #OralHealth #MiamiDentist #SouthFloridaDentist
```

**TikTok hashtag scrape:**
- Same hashtag logic, TikTok equivalents
- Results per hashtag: 30–50 videos
- Filter: videos with 500+ views only (signal quality threshold)

**YouTube search scrape:**
- Queries: "[service] explained" "[service] what to expect" "dental [service] Miami" "dentist [city]"
- Results: 20–30 videos per query
- Filter: videos published in last 12 months

**Facebook page posts (competitor accounts only):**
- Reuse competitor list from `competitive-intel-scraper`
- Last 30 posts per page
- Public posts only

**Instagram comments scrape (selective):**
- Run only on the top 10 highest-engagement posts from the hashtag scrape
- 50–100 comments per post
- Purpose: question and vocabulary extraction ONLY — no individual comment retention

Save all raw exports to: `data/raw/apify/{client-slug}/social-mining-{YYYY-MM-DD}/`

### Step 3 — Filter for signal quality
Before any analysis, filter the raw Apify exports:

**Engagement thresholds (apply per platform):**

| Platform | Format | Minimum engagement signal |
|---|---|---|
| Instagram | Reel | 500+ views |
| Instagram | Carousel | 50+ likes or 10+ saves |
| Instagram | Static post | 30+ likes |
| TikTok | Video | 1,000+ views |
| Facebook | Post | 20+ reactions |
| YouTube | Short | 500+ views |
| YouTube | Long video | 200+ views (lower bar — search-driven) |

Remove posts that:
- Are from accounts with fewer than 200 followers (too small to be signal-relevant)
- Are older than 90 days (recency matters for hook and format trends)
- Are clearly paid/boosted only (engagement pattern looks artificial)
- Contain any content that would violate the compliance rules in `_context/global/compliance-rules.md`

**Record filtered dataset size** — note how many posts remain after filtering. If fewer than 30 posts per service remain, broaden to county-level hashtags or national accounts.

### Step 4 — Hook pattern extraction
From all filtered posts, extract and classify the opening line or visual hook of each piece of content.

**Hook classification taxonomy** (aligned with what `social-copy` and `content-creator` use):

| Hook type | Pattern | Example |
|---|---|---|
| Question | Opens with a direct question to the reader | "What's the real reason your teeth feel sensitive?" |
| Bold statement | Opens with a provocative or surprising claim | "Most people brush wrong. Here's what to do instead." |
| Surprising fact | Opens with a counter-intuitive statistic or fact | "Your toothbrush is dirtier than you think after 3 months." |
| Relatable scenario | Opens with a situation the reader recognizes | "You know that feeling when you smile in a photo and immediately regret it?" |
| Local angle | Opens with a specific SE Florida reference | "Miami summers are tough on your teeth — here's why." |
| Challenge/myth-bust | Opens by calling out a common misconception | "Stop believing this dental myth." |
| How-to tease | Opens with a promise of actionable information | "Here's exactly what to do if you lose a filling." |
| Emotional/aspiration | Opens with the desired outcome or feeling | "Imagine smiling without covering your mouth." |

For each hook extracted, record:
```
Hook text (paraphrased — not verbatim): [text]
Hook type: [classification]
Service: [service category from taxonomy]
Pillar: [content pillar]
Platform: [Instagram / TikTok / Facebook / YouTube]
Format: [Reel / Carousel / Static / Short / Long video]
Engagement signal: [Low / Med / High per the thresholds above]
Language: English / Spanish
Local reference: Yes / No
```

**Do not record hooks verbatim.** Paraphrase the structure and note the vocabulary pattern. Example: instead of copying "Did you know that your gums can predict your heart health?", record: "Question hook connecting oral health to systemic health (heart disease). Opens with 'Did you know that...' pattern. High-engagement signal."

### Step 5 — Format performance analysis
From the filtered dataset, analyze which content formats are performing above threshold for each service and pillar.

**Analyze per format:**
- What is the average engagement for each format type?
- What video lengths are getting above-threshold views? (15s / 30s / 45s / 60s / 90s)
- What caption lengths correlate with higher engagement? (short ≤ 100 chars / medium 100–300 / long 300+)
- Are carousels or Reels performing better for educational content this month?
- Which formats are competitors using for high-revenue service promotion?

**Record format pattern findings as:**
```markdown
## Format: [Format name]
**Platform:** [platform]
**Service context:** [where it's performing]
**What's working:** [specific observation about structure, length, style]
**What's not working:** [patterns in low-performing versions of this format]
**Recommended use:** [when the client should use this format]
```

### Step 6 — Audience question extraction (PHI-safe)
From comment scrapes on the top-performing posts, extract questions patients are asking.

**PHI safety rules for comment mining:**
- Never record any comment verbatim — extract only the question PATTERN
- Never record commenter names, profile links, or any identifying information
- Never record comments that contain a person's specific dental situation ("My dentist said I need..." = retain pattern, discard personal detail)
- If a comment contains what appears to be PHI (e.g., someone describing a medical situation), skip it entirely

**Question pattern extraction:**
Convert each question comment into a generalized pattern:

Original comment: "Does getting dental implants hurt? I'm terrified of the procedure."
Extracted pattern: `[Anxiety/fear question] — "Does [procedure] hurt?" — Anxiety intent — Implants — Instagram`

Organize extracted questions by:
- Service (which treatment are they asking about?)
- Intent type (Informational / Anxiety / Consideration / Booking)
- Platform (where are these questions appearing?)

Target: 20–40 question patterns total across all mining sources.

These questions are high-value content opportunities — they represent exactly what potential patients want answered before booking.

### Step 7 — Hashtag intelligence analysis
From all scraped posts, analyze hashtag usage patterns.

**Per hashtag, record:**
- Average post engagement for posts using this hashtag
- Frequency of appearance in high-engagement posts
- Language (English / Spanish)
- Type: Local (#MiamiDentist) / Service (#DentalImplants) / General (#OralHealth) / Lifestyle (#MiamiLife) / Trend (currently spiking)

**Build hashtag performance tiers:**
- **Tier 1 — Local authority:** Hashtags with consistent presence in high-engagement local dental posts (use every post)
- **Tier 2 — Service-specific:** High-relevance service hashtags that perform for their target content (use when service matches)
- **Tier 3 — General reach:** Broader hashtags that extend reach but have less targeting value (use sparingly)
- **Tier 4 — Experimental:** Newer or trending hashtags worth testing

**Cap per platform:**
- Instagram: 3–10 total (Tier 1 + 2 + selective Tier 3)
- TikTok: 3–5 total (Tier 1 + 2 only)
- Facebook: 1–3 total (Tier 1 only)

### Step 8 — Trending audio detection (TikTok + Instagram Reels)
From TikTok and Instagram Reel exports, note which audio tracks appear in multiple high-performing dental posts.

For each trending audio track, record:
- Track name / artist (or "Original audio — [description]")
- Times observed in above-threshold dental content
- Content themes it's being paired with
- Tone/mood of the audio
- Whether it's currently spiking or plateauing

**Flag any audio that:**
- Is associated with unsafe, controversial, or health-misinformation content (do not recommend)
- References specific products, brands, or political content (compliance risk)

Trending audio is time-sensitive. Audio that is "trending now" may be stale in 2–3 weeks. Flag each audio with a "use by" estimate based on how early in the trend curve it appears.

### Step 9 — Spanish-language content mining (required if `bilingual: true`)
Repeat Steps 4–8 for Spanish-language content:
- Spanish hashtag scrape (Tier 4 scope from above)
- Separate hook classification for Spanish hooks
- Separate question bank in Spanish
- Spanish-specific hashtag sets
- Note tone and vocabulary differences between English and Spanish dental content

**SE Florida Spanish content notes:**
- Cuban-American Spanish dental content tends to be warmer and more family-focused than general LatAm dental content
- "Habla español" hooks perform well for trust-building in bilingual markets
- Spanish content that reads as machine-translated gets low engagement — flag any patterns that indicate this

### Step 10 — Compliance filter
Before finalizing any output, apply a compliance pass to all extracted patterns:

Remove any hook pattern or format example that:
- Contains before/after patient references without clear authorization language
- Makes guaranteed outcome claims ("perfect teeth," "guaranteed results")
- Uses fear-based or shame-based framing
- References patient testimonials in a way that implies PHI
- Involves DIY dental content (charcoal whitening, DIY braces, DIY veneers) — these are hard veto trend patterns

Flag any hook patterns that are borderline for human review before they enter the library.

### Step 11 — Build the hook library (context file)
Compile all compliance-passed hook patterns into the reusable hook library.

Save to: `_context/clients/{client-slug}/hook-library.md`

**Required structure:**
```markdown
# Hook Library — {Practice Name}
**Last Updated:** {YYYY-MM-DD}
**Mining cycle:** {date range}
**Valid through:** {30 days from last updated}

---

## Hook Type: Question
### Pillar: Education
- **Pattern:** "What's the real reason [dental concern]?" → Opens curiosity about a common patient confusion
  Service: General / Prevention | Signal: High | Language: English
- ...

### Pillar: Service Demand
- **Pattern:** "[Procedure] or [alternative]? Here's how to decide." → Comparison framing for consideration-stage patients
  Service: Implants / Orthodontics | Signal: High | Language: English
- ...

## Hook Type: Bold Statement
### Pillar: Education
- ...

## Hook Type: Surprising Fact
...

## Hook Type: Relatable Scenario
...

## Hook Type: Local Angle
### SE Florida specific
- **Pattern:** "[Local weather/lifestyle reference] + dental connection" → Bridges local identity with oral health
  Example structure: "[Miami season/event] is [impact on teeth]. Here's what to do."
  Signal: High on local accounts | Language: Both
- ...

## Hook Type: Challenge / Myth-Bust
...

## Hook Type: How-To Tease
...

## Hook Type: Emotional / Aspiration
...

---

## Audience Question Bank
[20–40 question patterns organized by service and intent]

## Trending Audio (TikTok + Reels)
[Audio tracks observed, use-by dates, content pairings]

## Format Performance Summary
[Key format findings for this mining cycle]
```

If the file already exists (monthly refresh), replace the content and update the date — do not append old patterns. The hook library reflects the most current mining cycle.

### Step 12 — Build hashtag sets (context file)
Compile tiered hashtag sets into a reference file.

Save to: `_context/clients/{client-slug}/hashtag-sets.md`

**Required structure:**
```markdown
# Hashtag Sets — {Practice Name}
**Last Updated:** {YYYY-MM-DD}

## Instagram

### [Service: Implants]
Tier 1 (always use): #MiamiDentist #DentalImplants #ImplantDentist
Tier 2 (use when relevant): #ToothImplant #MissingTooth #SmileRestoration
Tier 3 (occasional reach): #DentalHealth #OralHealth #Dentist

### [Service: Whitening]
...

## TikTok

### General dental
Tier 1: #DentalTikTok #DentistTikTok #OralHealth
Tier 2: [service-specific]

## Facebook
[1–3 hashtags max per post]

## Spanish Sets (if bilingual: true)

### Instagram — Spanish
Tier 1: #DentistaMiami #SaludDental #DentistaHispano
...
```

### Step 13 — Write research outputs
Compile full analysis into a structured research file.

Save to: `research/social-mining/{client-slug}/{client-slug}-social-mining-{YYYY-MM-DD}.md`

**Required sections:**
1. Mining scope and methodology
2. Datasets collected (Apify jobs run, hashtags scraped, posts processed, posts filtered out)
3. Hook pattern analysis (all classified hooks, with signal ratings)
4. Format performance analysis (all format findings)
5. Audience question bank (all 20–40 questions)
6. Hashtag intelligence (all tiers, with performance rationale)
7. Trending audio (all observed tracks)
8. Spanish-language findings (if bilingual: true)
9. Compliance filter log (anything removed and why)
10. Trend candidates for scoring (see Step 14)
11. Sources (all Apify Actor runs, hashtags scraped, accounts included)

End file with: `**Status:** Needs review`
**Valid through:** {30 days — social content trends move fast}

### Step 14 — Flag trend candidates for scoring
From the mining data, identify any content patterns that appear to be gaining velocity — not just performing well, but accelerating. These become trend candidates for `trend-detection-scoring`.

For each trend candidate, write:
```markdown
**Trend Candidate:** [name/description]
**Evidence:** [what was observed — how many posts, what platforms, engagement signals]
**Estimated velocity:** Rising fast / Steady growth / Plateauing
**Dental relevance:** [how a dental practice could participate]
**Compliance concern:** None / Review needed / Hard veto risk
**Recommended action:** Submit to trend-detection-scoring / Monitor / Reject
```

Save to: `research/trends/{client-slug}/trend-candidates-{YYYY-MM-DD}.md`

### Step 15 — Handoff notes
Append to `logs/decisions.md`:
```
## {date} — Social Media Mining Complete: {client-name}
- Posts processed: [N after filtering]
- Hook patterns extracted: [N]
- Audience questions extracted: [N]
- Hashtag sets built: [N platforms]
- Trend candidates flagged: [N] — submitted to trend-detection-scoring
- Spanish mining: completed / not applicable / incomplete
- Next mining cycle due: {30 days}
- Key creative insight this cycle: [one sentence]
```

## Output Files
```
data/raw/apify/{client-slug}/social-mining-{YYYY-MM-DD}/
  instagram-{hashtag}-posts.json    ← one per hashtag scraped
  instagram-{post-id}-comments.json ← top 10 performing posts only
  tiktok-{hashtag}-videos.json
  youtube-{query}-results.json
  facebook-{account}-posts.json

research/social-mining/{client-slug}/
  mining-scope-{YYYY-MM-DD}.md
  {client-slug}-social-mining-{YYYY-MM-DD}.md   ← full analysis

research/trends/{client-slug}/
  trend-candidates-{YYYY-MM-DD}.md   ← feeds trend-detection-scoring

_context/clients/{client-slug}/
  hook-library.md        ← consumed by social-copy and content-creator
  hashtag-sets.md        ← consumed by social-copy and content-creator

logs/
  decisions.md           ← handoff notes appended
```

## Quality Checklist
- [ ] Mining scope defined before any Apify jobs run
- [ ] Apify jobs configured for all active platforms in brand-context.md
- [ ] Raw exports saved to `data/raw/apify/{client-slug}/social-mining-{YYYY-MM-DD}/`
- [ ] Engagement threshold filtering applied — filtered dataset size recorded
- [ ] Hook patterns extracted and classified by type, pillar, service, platform
- [ ] No verbatim hook text recorded — all hooks are paraphrased patterns
- [ ] Format performance analysis complete for all platforms
- [ ] Audience questions extracted (20–40 patterns) — no individual comment text retained
- [ ] PHI check passed — no patient names, identifying details, or specific case descriptions
- [ ] Hashtag intelligence complete with tiering per platform
- [ ] Trending audio noted with use-by estimates
- [ ] Spanish-language mining complete (required if bilingual: true)
- [ ] Compliance filter applied — all problematic patterns removed
- [ ] Hook library written to `_context/clients/{client-slug}/hook-library.md`
- [ ] Hashtag sets written to `_context/clients/{client-slug}/hashtag-sets.md`
- [ ] Full research file saved to `research/social-mining/{client-slug}/`
- [ ] Trend candidates written to `research/trends/{client-slug}/`
- [ ] Handoff notes written to `logs/decisions.md`

## Failure Modes
- **Apify scrape returns fewer than 30 posts after filtering** — broaden hashtag scope to county level or national dental accounts; note limitation in research file
- **No above-threshold content found for a priority service** — this is itself a finding: either low competitor content activity (opportunity) or a very saturated space; document both interpretations
- **Apify Actor unavailable or deprecated** — check Apify Store for current alternative; note in scope file; do not proceed without verifying the Actor scrapes public data only
- **Comments are too sparse for question extraction** — use GBP Q&A data from `keyword-research` outputs as supplement; note the source swap
- **Spanish-language scrape yields very few results** — note as content opportunity; first-mover advantage for the client in Spanish dental content for their city
- **Trending audio changes rapidly between mining and production** — always verify audio is still trending before recommending it in a script; audio shelf life is typically 1–3 weeks

## Dependencies
- Apify (`APIFY_API_TOKEN` env var — irreplaceable for social platform data: Instagram, TikTok, Facebook, YouTube. Firecrawl and Crawl4AI cannot extract structured social metrics from these platforms)
- Firecrawl (primary — for any web content portions: GBP Q&A, dental blog content, public web pages) or Crawl4AI (fallback: `crwl <url> -o markdown`)
- `client-context-ingestion` (must run first)
- `market-research` (must run first — provides service priorities)
- `competitive-intel-scraper` (run before if possible — reuse its Apify exports)
- `_context/global/content-pillars.md`
- `_context/global/platform-rules.md`
- `_context/global/compliance-rules.md`
- `_context/global/dental-service-taxonomy.md`

**Feeds into:** `campaign-brief`, `social-copy`, `short-form-video-script`, `content-creator` agent, `trend-detection-scoring` (via trend candidates)
