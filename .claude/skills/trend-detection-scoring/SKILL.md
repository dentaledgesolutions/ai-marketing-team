# Skill: trend-detection-scoring

## Description
Applies a 10-criterion weighted scoring matrix to social trend candidates and produces a scored weekly report with threshold decisions, hard veto checks, and content recommendations. Only trends scoring 70+ and passing all hard veto rules enter the content calendar. Integrates DataForSEO Google Trends for signal velocity, references competitive intelligence for whitespace scoring, and cross-checks approved claims before recommending production. Maintains a persistent watchlist across weekly cycles.

## When to Use
- Weekly, every Monday before the content calendar planning session
- When a trend candidate is spotted mid-week and needs immediate evaluation
- When the Campaign Strategist needs scored trend inputs before building a campaign brief
- When a client asks whether to participate in a specific trend

Run after `social-media-mining` (which feeds trend candidates). Run before `campaign-brief`.

## Required Inputs

| Input | Priority | Source |
|---|---|---|
| `research/trends/{client-slug}/trend-candidates-{YYYY-MM-DD}.md` | Required | social-media-mining output |
| `_context/clients/{client-slug}/trend-watchlist.md` | Required | Previous trend cycles (created by this skill) |
| `_context/clients/{client-slug}/approved-claims.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/service-offers.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/asset-inventory.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/competitors.md` | Required | competitive-intel-scraper |
| `_context/clients/{client-slug}/keyword-clusters.md` | Preferred | keyword-research |
| `_context/global/compliance-rules.md` | Required | Global context |
| `_context/global/dental-service-taxonomy.md` | Required | Global context |
| `_context/global/content-pillars.md` | Required | Global context |
| `_context/global/southeast-florida-market.md` | Required | Global context |
| Bilingual flag | Required | Check brand-context.md for `bilingual: true` |

## Trend Candidate Intake Format
Each trend candidate entering this skill must have the following fields. Candidates from `social-media-mining` will already use this format. Manual submissions must be formatted before scoring begins.

```markdown
## Candidate: [Trend Name or Short Description]
**Category:** Format trend / Topic trend / Audio trend / Cultural moment / Industry news / Platform feature
**Platforms observed:** [list]
**First signal date:** [YYYY-MM-DD]
**Sources:** [URLs, accounts, or Apify export references]
**Volume estimate:** [views, uses, posts — approximate]
**Spanish signal:** Yes / No / Unknown (is this trend present in Spanish-language content?)
**Entered from:** social-media-mining / competitor observation / manual submission / watchlist carry-forward
```

## Trend Category Definitions
Classifying the trend type routes it correctly through the Campaign Strategist:

| Category | Description | Typical lifespan | Content route |
|---|---|---|---|
| Format trend | A content format gaining traction (e.g., "Get Ready With Me" style) | 2–6 weeks | Reels / TikTok |
| Topic trend | A dental/health topic with rising search interest | 1–3 months | Education pillar, carousels |
| Audio trend | A specific sound or song being widely used in Reels/TikTok | 1–3 weeks | Reels only |
| Cultural moment | A local or national event/awareness month | Fixed window | All platforms |
| Industry news | ADA announcement, new treatment approval, clinical study | Weeks to months | Education pillar |
| Platform feature | New platform feature driving engagement (e.g., new sticker type) | Until novelty fades | Platform-specific |

## Scoring Matrix

Score each criterion 0–5 using the data sources defined below. Multiply by weight. Sum for weighted total out of 100.

| Criterion | Weight | 0 | 3 | 5 | Data source |
|---|---:|---|---|---|---|
| Signal velocity | 15 | No growth | Moderate growth | Rapid growth in last 7–14 days | DataForSEO Google Trends + social-mining Apify data |
| Cross-platform presence | 10 | One weak signal | Present on 2 platforms | Present on 3+ platforms | social-mining Apify exports per platform |
| Dental relevance | 15 | Not dental-relevant | Adaptable to dental | Directly dental / oral-health relevant | dental-service-taxonomy.md |
| Local SE Florida fit | 10 | Not locally relevant | Some local fit | Strong Miami/Broward/Palm Beach fit | southeast-florida-market.md + Spanish signal check |
| Service alignment | 10 | No service connection | Supports a secondary service | Supports a priority service or offer | service-offers.md priority list |
| Patient intent value | 10 | Pure entertainment | Awareness / education | Consultation or lead intent | keyword-clusters.md intent classification |
| Competitive whitespace | 10 | Many local competitors using it | Some usage | Few or no local competitors using it well | competitors.md + competitor-matrix.csv |
| Creative feasibility | 10 | Hard to produce | Can produce with effort | Easy to produce with existing assets | asset-inventory.md |
| Compliance safety | 5 | High risk | Needs edits | Low risk | compliance-rules.md |
| Evidence quality | 5 | Anecdotal only | Some measurable evidence | Strong data from ≥ 2 independent sources | Count verified, independent signal sources |

**Score documentation rule:** Every score must be accompanied by a one-line evidence note. "Score: 4" is not acceptable. "Score: 4 — observed on Instagram and TikTok with 8K+ uses; Google Trends shows 23% WoW growth in Florida" is correct.

## Hard Veto Rules
Reject any trend immediately regardless of total score if:
- Compliance safety score is 0, 1, or 2
- Evidence quality score is 0, 1, or 2
- The trend encourages unsafe dental behavior (DIY braces, DIY veneers, charcoal whitening, oil pulling as treatment, hydrogen peroxide whitening at unsafe concentrations)
- The trend involves self-diagnosis or at-home treatment
- The trend relies on shame-based, fear-based, or negative self-perception messaging
- The trend makes clinical claims that cannot be verified or attributed
- The trend is associated with viral controversy, health misinformation, or a news scandal
- The trend requires patient before/after imagery without documented authorization

## Decision Thresholds

| Score | Decision | Action | Typical content window |
|---|---|---|---|
| 85–100 | **Priority** | Produce content within 7 days | Audio trends: 3–5 days; Format/Topic: 7 days |
| 70–84 | **Meaningful** | Add to next monthly content calendar | Within 30 days |
| 50–69 | **Watchlist** | Monitor; re-score next week; do not produce | Re-evaluate in 7 days |
| 0–49 | **Reject** | Do not produce; log reason | N/A |

## Process

### Step 1 — Load candidates and watchlist
Load two sources:

**A. Fresh candidates from `social-media-mining`:**
Read `research/trends/{client-slug}/trend-candidates-{YYYY-MM-DD}.md`.
Convert any candidates not already in the intake format (see above).

**B. Watchlist carry-forwards:**
Read `_context/clients/{client-slug}/trend-watchlist.md`.
Pull all items with status `Watchlist` from the previous cycle. These are re-scored this week alongside fresh candidates.

If `trend-watchlist.md` does not yet exist (first run), create it now as an empty file with the header:
```markdown
# Trend Watchlist — {Practice Name}
_Managed by trend-detection-scoring. Updated weekly._
```

Compile the full candidate list: fresh candidates + watchlist carry-forwards.

### Step 2 — Google Trends validation (DataForSEO)
For every candidate, query DataForSEO Google Trends to get an objective signal velocity reading. This data informs the Signal Velocity criterion score (weight: 15) and the Evidence Quality criterion score (weight: 5).

```bash
AUTH=$(echo -n "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" | base64)
curl -s -X POST "https://api.dataforseo.com/v3/keywords_data/google_trends/explore/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keywords": ["teeth whitening", "dental veneers"],
    "location_name": "Florida,United States",
    "date_from": "2026-01-01",
    "date_to": "2026-06-24",
    "type": "web"
  }]'
```

**Use the `set date_from` to 6 months ago and `date_to` to today.**
Use `location_name: "Florida,United States"` for all SE Florida clients (state-level granularity is more reliable than county-level).

**For bilingual clients (bilingual: true):** Run the same query with `language_name: "Spanish"` to check Spanish-language search trend velocity separately. A trend rising in Spanish but flat in English is a bilingual content opportunity.

**Signal Velocity score mapping from Google Trends interest values:**
- Rising sharply (≥ 20% week-over-week growth) → score 5
- Moderate upward trend (5–19% WoW growth) → score 3
- Flat (< 5% change) → score 2
- Declining → score 0–1

If DataForSEO credentials are unavailable, score Signal Velocity from social signal volume alone and note the limitation in the report.

Save raw Google Trends responses to: `data/raw/dataforseo/{client-slug}/trend-scoring-{YYYY-MM-DD}/`

### Step 3 — Hard veto check
Before scoring, apply hard veto rules to every candidate. Process is sequential — as soon as one veto triggers, stop and mark the candidate Rejected.

For each vetoed candidate, record:
```
Trend: [name]
Status: Rejected
Veto rule triggered: [specific rule from Hard Veto Rules list]
Notes: [brief context]
```

Add rejected candidates to the Rejected section of the weekly report. Do not add them to the watchlist.

### Step 4 — Score each non-vetoed trend
For each candidate that passed the veto check:

**Score each criterion 0–5 with evidence:**

**Signal Velocity (15pts)**
Use DataForSEO Google Trends output from Step 2. Note WoW growth percentage.

**Cross-platform Presence (10pts)**
Check `social-media-mining` Apify export summaries:
- Instagram: observed in hashtag data?
- TikTok: observed in hashtag data?
- Facebook: observed in competitor page posts?
- YouTube: observed in search results?
Score = number of platforms with confirmed presence (0=none, 3=2 platforms, 5=3+ platforms).

**Dental Relevance (15pts)**
Cross-reference `dental-service-taxonomy.md`:
- Score 5: trend directly references a dental service or oral health topic in the taxonomy
- Score 3: trend is health/beauty-adjacent and can be naturally bridged to a dental service
- Score 0: trend has no credible dental connection

**Local SE Florida Fit (10pts)**
Cross-reference `southeast-florida-market.md`:
- Does the trend reference a SE Florida lifestyle, cultural moment, or geographic context?
- Is the trend active in SE Florida-specific hashtags or accounts?
- **If bilingual: true:** Is the trend present in Spanish-language content? (bonus signal — bump score by 1 if yes, max 5)

**Service Alignment (10pts)**
Cross-reference `service-offers.md` priority services:
- Score 5: trend maps directly to the client's #1 or #2 priority service
- Score 3: trend maps to a secondary service
- Score 0: trend has no service connection

**Patient Intent Value (10pts)**
Cross-reference `keyword-clusters.md` intent levels (if available):
- Score 5: topic has Booking or Emergency intent keywords in the cluster
- Score 3: topic has Informational or Consideration intent keywords
- Score 0: topic is pure entertainment, no path to dental care consideration

**Competitive Whitespace (10pts)**
Cross-reference `competitors.md` and `competitor-matrix.csv`:
- Are any high-threat competitors already using this trend?
- Score 5: no local competitors are using it at all
- Score 3: low-threat competitors using it, but high-threat competitors are not
- Score 0: multiple high-threat competitors already have content on this trend

**Creative Feasibility (10pts)**
Cross-reference `asset-inventory.md`:
- Can this content be produced with assets the practice already has?
- Score 5: existing footage, photos, or team availability is sufficient
- Score 3: requires new footage or scheduling but is achievable within 7 days
- Score 0: requires resources (equipment, talent, location) the practice cannot access quickly

**Compliance Safety (5pts)**
Cross-reference `compliance-rules.md`:
- Score 5: trend participation requires no modifications to be compliant
- Score 3: trend requires edits (remove a claim, change framing) to be compliant
- Score 0–2: trend is inherently high-risk → triggers hard veto (see Step 3)

**Evidence Quality (5pts)**
Count the number of independent, verifiable signal sources:
- Score 5: 3+ independent sources (e.g., DataForSEO Trends data + Apify Instagram data + Apify TikTok data)
- Score 3: 2 sources
- Score 0–2: only 1 source (anecdotal) → triggers hard veto (see Step 3)

**Calculate weighted total:**
```
Total = (Signal Velocity score × 15) + (Cross-platform score × 10) +
        (Dental Relevance score × 15) + (Local Fit score × 10) +
        (Service Alignment score × 10) + (Patient Intent score × 10) +
        (Whitespace score × 10) + (Creative Feasibility score × 10) +
        (Compliance Safety score × 5) + (Evidence Quality score × 5)
```

### Step 5 — Apply threshold decisions
Assign decision based on total:
- 85–100 → Priority
- 70–84 → Meaningful
- 50–69 → Watchlist
- 0–49 → Reject

### Step 6 — Write content recommendations
For Priority and Meaningful trends only, write a structured recommendation:

```markdown
## Recommendation: [Trend Name]
**Score:** [N]/100 | **Decision:** Priority / Meaningful
**Category:** [trend category]
**Estimated production window:** [e.g., "Produce by YYYY-MM-DD — audio trends expire fast" or "Flexible — topic trend, relevant for 4–6 weeks"]

**Content recommendation:**
- Format: [Reel / Carousel / GBP post / Caption / YouTube Short]
- Platform: [primary] + [secondary if applicable]
- Service tie-in: [specific service from taxonomy]
- Content pillar: [pillar name]
- Recommended hook angle: [one specific hook idea in the client's voice]
- Production requirements: [what's needed — doctor on camera? existing footage? new shots?]

**Approved claims check:**
- Does this trend require any service claims? [Yes / No]
- If yes: are those claims in approved-claims.md? [Yes: "[claim]" / No — flag for dentist validation before producing]

**Compliance notes:** [any specific compliance considerations for this trend]

**Performance tracking:** Check engagement on this content by [date 14 days after expected publish] and log in campaign report.
```

### Step 7 — Update trend watchlist
Update `_context/clients/{client-slug}/trend-watchlist.md`:

- **New Watchlist items (score 50–69):** Add with intake format + this week's score + re-score due date (7 days)
- **Escalated items (previously Watchlist, now 70+):** Move to Meaningful or Priority; remove from watchlist
- **Expired items (previously Watchlist, signal declined):** Mark as Rejected; remove from watchlist
- **Stale items (on watchlist for 3+ weeks with no change):** Mark as Rejected — if a trend hasn't accelerated in 3 weeks, it likely won't

**Watchlist file format:**
```markdown
# Trend Watchlist — {Practice Name}
_Managed by trend-detection-scoring. Updated weekly._

## Active Watchlist

### [Trend Name]
- First scored: [date] | Score: [N]/100
- Re-score due: [date]
- Last score: [N]/100 — [brief note on signal change]
- Weeks on watchlist: [N]

## Expired This Week
[Trends removed this cycle with reason]
```

### Step 8 — Write weekly trend report
Use `_templates/trend-report-template.md` as the base structure.

Save to: `reports/weekly-trend-reports/{YYYY-MM-DD}_{client-slug}_trend-report.md`

**Required report sections:**
1. Summary (date, candidates evaluated, Priority: N, Meaningful: N, Watchlist: N, Rejected: N)
2. Priority trends (full score breakdown + content recommendation)
3. Meaningful trends (full score breakdown + content recommendation)
4. Watchlist updates (what changed from last week)
5. Rejected trends (name + veto rule or score)
6. Data sources used this cycle (DataForSEO query dates, Apify export dates, manual observations)

End report with: `**Status:** Needs review`

Save scored data to: `data/processed/trend-scores/{YYYY-MM-DD}_{client-slug}_scores.json`

JSON structure per trend:
```json
{
  "date": "YYYY-MM-DD",
  "client": "client-slug",
  "trend": "trend name",
  "category": "trend category",
  "total_score": 78,
  "decision": "Meaningful",
  "scores": {
    "signal_velocity": {"score": 4, "weighted": 60, "evidence": "..."},
    "cross_platform": {"score": 3, "weighted": 30, "evidence": "..."},
    ...
  },
  "production_window": "Within 30 days",
  "performance_check_due": "YYYY-MM-DD"
}
```

### Step 9 — Handoff notes
Append to `logs/decisions.md`:
```
## {date} — Trend Scoring Complete: {client-name}
- Candidates evaluated: [N] (fresh: N, watchlist: N)
- Priority: [N] — [trend names]
- Meaningful: [N] — [trend names]
- Watchlist: [N carried forward, N new]
- Rejected: [N]
- Key signal this week: [one sentence about the most significant finding]
- Next scoring cycle: [date, 7 days]
```

## Output Files
```
data/raw/dataforseo/{client-slug}/trend-scoring-{YYYY-MM-DD}/
  google-trends-{keyword}.json     ← one per trend candidate queried

data/processed/trend-scores/
  {YYYY-MM-DD}_{client-slug}_scores.json

reports/weekly-trend-reports/
  {YYYY-MM-DD}_{client-slug}_trend-report.md

_context/clients/{client-slug}/
  trend-watchlist.md               ← persistent across weekly cycles

logs/
  decisions.md                     ← handoff notes appended
```

## Quality Checklist
- [ ] Trend candidates loaded from `research/trends/{client-slug}/trend-candidates-{date}.md`
- [ ] Watchlist carry-forwards loaded from `trend-watchlist.md`
- [ ] DataForSEO Google Trends query run for all candidates (English)
- [ ] DataForSEO Google Trends query run in Spanish (required if bilingual: true)
- [ ] Hard veto check applied to every candidate before any scoring
- [ ] Every criterion scored with a one-line evidence note (not just a number)
- [ ] Signal Velocity score grounded in DataForSEO data (or fallback noted)
- [ ] Cross-platform presence checked against Apify export data
- [ ] Service Alignment checked against service-offers.md priority list
- [ ] Competitive Whitespace checked against competitors.md and competitor-matrix.csv
- [ ] Creative Feasibility checked against asset-inventory.md
- [ ] Patient Intent Value checked against keyword-clusters.md (if available)
- [ ] Weighted total calculated correctly for all non-vetoed candidates
- [ ] Threshold decisions applied consistently
- [ ] Content recommendation written for every Priority and Meaningful trend
- [ ] Approved-claims.md cross-checked in every content recommendation
- [ ] Production window and performance check date set for Priority trends
- [ ] Trend watchlist updated (escalations, expirations, new entries)
- [ ] Weekly report written and saved to `reports/weekly-trend-reports/`
- [ ] Scored JSON saved to `data/processed/trend-scores/`
- [ ] Handoff notes written to `logs/decisions.md`

## Failure Modes
- **No trend candidates from social-media-mining** — note data gap; manually check trending dental hashtags on Instagram and TikTok; flag for social-media-mining to run a fresh Apify scan
- **DataForSEO Google Trends returns no data for a candidate** — topic may have too low search volume to trend in Google; score Signal Velocity from social signals only; note in report
- **All candidates score below 50** — report no trends met threshold this week; recommend re-scoring mid-week if signal accelerates; do not force Watchlist placement to fill a quota
- **Watchlist item goes viral overnight** — escalate to Priority immediately outside the weekly cycle; notify Campaign Strategist; set a 3-day production window
- **Trend is ambiguous (borderline compliance)** — score Compliance Safety at 2 (triggers hard veto); escalate to Compliance / QA Reviewer before reconsidering; never produce ambiguous content without expert review
- **Audio trend identified but no audio-ready production capacity** — score Creative Feasibility at 0–1; if total score still hits Watchlist+, note the production gap and set a re-score date once capacity is confirmed

## Dependencies
- DataForSEO (`DATAFORSEO_LOGIN`, `DATAFORSEO_PASSWORD` — Google Trends API)
- `social-media-mining` (primary source of trend candidates)
- `competitive-intel-scraper` (provides competitors.md and competitor-matrix.csv for whitespace scoring)
- `keyword-research` (provides keyword-clusters.md for intent value scoring)
- `_context/global/compliance-rules.md`
- `_context/global/dental-service-taxonomy.md`
- `_context/global/content-pillars.md`
- `_context/global/southeast-florida-market.md`
- `_context/clients/{client-slug}/approved-claims.md`
- `_context/clients/{client-slug}/service-offers.md`
- `_context/clients/{client-slug}/asset-inventory.md`
- `_context/clients/{client-slug}/competitors.md`
- `_context/clients/{client-slug}/trend-watchlist.md` (created on first run)
- `_templates/trend-report-template.md`

**Feeds into:** `campaign-brief`, `social-copy`, `short-form-video-script`, `campaign-report` (via performance check dates)
