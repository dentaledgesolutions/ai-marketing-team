# Skill: trend-detection-scoring

## Description
Applies the 10-criterion weighted scoring matrix to social trend candidates. Takes raw trend signals from social mining, competitor observation, or manual input and produces a scored report with a threshold decision and hard veto check. Only trends scoring 70+ and passing all hard veto rules enter the content calendar.

## When to Use
- Weekly, before the monthly content calendar is built
- When a trend candidate is spotted during social media mining or competitor monitoring
- When the Campaign Strategist needs scored trend inputs before building a campaign brief
- When a client asks about participating in a specific trend before it has been evaluated

## Required Inputs
- Trend candidates (from social mining, Apify exports, competitor observation, or manual submission)
- `_context/global/compliance-rules.md`
- `_context/global/dental-service-taxonomy.md`
- `_context/global/content-pillars.md`
- `_context/clients/{client}/approved-claims.md` (for service alignment check)
- Previous trend reports from `reports/weekly-trend-reports/` (for watchlist continuity)

## Scoring Matrix

Score each criterion 0–5. Multiply by weight. Sum for weighted total out of 100.

| Criterion | Weight | 0 | 3 | 5 |
|---|---:|---|---|---|
| Signal velocity | 15 | No growth | Moderate growth | Rapid growth in last 7–14 days |
| Cross-platform presence | 10 | One weak signal | Present on 2 platforms | Present on 3+ platforms |
| Dental relevance | 15 | Not dental-relevant | Adaptable to dental | Directly dental / oral-health relevant |
| Local SE Florida fit | 10 | Not locally relevant | Some local fit | Strong Miami/Broward/Palm Beach fit |
| Service alignment | 10 | No service connection | Supports a secondary service | Supports a priority service or offer |
| Patient intent value | 10 | Pure entertainment | Awareness / education | Consultation or lead intent |
| Competitive whitespace | 10 | Many local competitors using it | Some usage | Few or no local competitors using it well |
| Creative feasibility | 10 | Hard to produce | Can produce with effort | Easy to produce with existing assets |
| Compliance safety | 5 | High risk | Needs edits | Low risk |
| Evidence quality | 5 | Anecdotal | Some measurable evidence | Strong data from multiple sources |

## Hard Veto Rules
Reject any trend immediately regardless of total score if:
- Compliance safety score is 0, 1, or 2
- Evidence quality score is 0, 1, or 2
- The trend encourages unsafe dental behavior (DIY braces, DIY veneers, charcoal whitening, oil pulling as treatment)
- The trend involves self-diagnosis or at-home treatment
- The trend relies on shame-based, fear-based, or negative self-perception messaging
- The trend makes clinical claims that cannot be verified or attributed
- The trend is associated with a viral controversy, misinformation, or health scare

## Decision Thresholds

| Score | Decision | Action |
|---|---|---|
| 85–100 | **Priority** | Produce content within 7 days |
| 70–84 | **Meaningful** | Add to next monthly content calendar |
| 50–69 | **Watchlist** | Monitor; re-score next week; do not produce |
| 0–49 | **Reject** | Do not produce; log reason |

## Process

### Step 1 — Collect trend candidates
Sources to check:
- Apify exports from `data/raw/apify/` (Instagram, TikTok, YouTube public data)
- Manual social browsing (trending dental hashtags, competitor posts)
- Industry news and dental trade publications
- Previous watchlist items from `reports/weekly-trend-reports/`
- **DataForSEO Google Trends API** — use to validate signal velocity with search data (see below)

For each candidate, record:
- Trend name or description
- Platforms where signal was observed
- First signal date
- Source accounts or URLs
- Volume estimate (views, uses, posts)

**DataForSEO Google Trends validation (for Signal Velocity criterion):**
When a trend candidate is identified, query Google Trends to confirm whether search interest for the underlying topic is rising, flat, or declining. This grounds the Signal Velocity score in data rather than impression.

```bash
AUTH=$(echo -n "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" | base64)
curl -s -X POST "https://api.dataforseo.com/v3/keywords_data/google_trends/explore/live" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keywords": ["teeth whitening", "dental veneers"],
    "location_name": "Florida,United States",
    "date_from": "2025-12-01",
    "date_to": "2026-06-24",
    "type": "web"
  }]'
```

Use the trend interest-over-time values to inform the Signal Velocity score:
- Rising sharply in last 7–14 days → score 5
- Moderate upward trend → score 3
- Flat or declining → score 0–1

If DataForSEO credentials are unavailable, score Signal Velocity from social signals alone and note the limitation.

### Step 2 — Hard veto check
Before scoring, apply hard veto rules. If any veto is triggered:
- Mark as Rejected
- Record the veto reason
- Do not score further
- Move to the rejected section of the report

### Step 3 — Score each trend
For each non-vetoed trend:
- Score each criterion 0–5 with a brief note
- Calculate weighted score: (score × weight) for each criterion
- Sum all weighted scores for the total out of 100

### Step 4 — Apply threshold decision
Assign the decision based on total score:
- 85–100 → Priority
- 70–84 → Meaningful
- 50–69 → Watchlist
- 0–49 → Reject

### Step 5 — Write content recommendation
For Priority and Meaningful trends only:
- Suggest a specific content format (Reel, carousel, GBP post, etc.)
- Identify the service tie-in
- Note the recommended hook angle
- Flag any production requirements (new footage, doctor on camera, etc.)
- Note any compliance considerations for this specific trend

### Step 6 — Update watchlist
- Carry forward any Watchlist items from the previous week
- Re-score if new data is available
- Escalate to Meaningful or downgrade to Reject if signal has changed

### Step 7 — Write report
Use `_templates/trend-report-template.md`.
Save to `reports/weekly-trend-reports/YYYY-MM-DD_{client}_trend-report.md`.
Save scored data to `data/processed/trend-scores/YYYY-MM-DD_{client}_scores.json`.

## Output Files
- `reports/weekly-trend-reports/YYYY-MM-DD_{client}_trend-report.md`
- `data/processed/trend-scores/YYYY-MM-DD_{client}_scores.json`

## Quality Checklist
- [ ] All trend candidates collected from at least 2 sources
- [ ] Hard veto check applied to every candidate before scoring
- [ ] Every criterion scored with a rationale note
- [ ] Weighted total calculated correctly
- [ ] Threshold decision applied consistently
- [ ] Content recommendation written for every Priority and Meaningful trend
- [ ] Watchlist updated with previous week's carry-forwards
- [ ] Report written using `_templates/trend-report-template.md`
- [ ] Output saved to correct folders

## Failure Modes
- **No trend data available** — note data gap in report; flag for Competitive Intelligence Analyst to run a fresh Apify scan
- **All trends score below 50** — report that no trends met threshold this week; recommend re-checking in 3–5 days
- **Trend is ambiguous** — score conservatively; when in doubt, reduce compliance safety score; prefer Watchlist over Meaningful
- **Watchlist item goes viral overnight** — escalate to Priority immediately; notify Campaign Strategist; do not wait for the weekly cycle

## Dependencies
- DataForSEO (Google Trends API for signal velocity validation — `DATAFORSEO_LOGIN`, `DATAFORSEO_PASSWORD` env vars)
- `_context/global/compliance-rules.md`
- `_context/global/dental-service-taxonomy.md`
- `_context/global/content-pillars.md`
- `_context/clients/{client}/approved-claims.md`
- `_templates/trend-report-template.md`
- `data/raw/apify/` (Apify exports when available)
- Previous `reports/weekly-trend-reports/` (for watchlist continuity)
