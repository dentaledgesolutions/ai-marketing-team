---
name: campaign-report
description: |
  Use when asked to analyze campaign results, create a monthly performance
  report, review social media metrics, assess what content worked or didn't,
  produce a client report, or prepare next-cycle recommendations for a dental
  marketing campaign — even if the user says "let's look at how this month
  went," "write up the report," "what performed best," or "what should we
  change next month." Also use when platform metric CSVs are available and
  the team needs content lessons before starting the next campaign brief.
---

# campaign-report

Analyzes platform metric exports against the approved campaign brief and
produces a filled `_templates/monthly-report-template.md` with intelligent
analysis — not just data transcription. The goal is to understand *why*
content performed as it did and feed those lessons back into future campaigns.

## What you need to start

**Required:**
- Platform metric exports (CSV) saved to `data/raw/platform-exports/{client-slug}/`
- Approved campaign brief: `social/calendars/{client-slug}/`
- Content calendar: `_templates/monthly-content-calendar.csv` (or client copy)
- `_context/clients/{client-slug}/brand-context.md`

**Preferred:**
- Previous month's report (for comparison) from `reports/monthly-performance-reports/`
- GBP performance data (from Google Business Profile dashboard export or DataForSEO)
- Lead/inquiry data from practice (calls logged, forms filled, DMs received)

If platform exports haven't been pulled yet, ask the user to export them from
each platform's native analytics dashboard before proceeding. Each platform
export should cover the full calendar month being reported.

## How to calculate engagement rate

Engagement rate is the single most important metric for comparing posts across
platforms and follower counts. Always use this formula:

**Engagement rate = (likes + comments + saves + shares) ÷ reach × 100**

- Use *reach* (unique accounts), not *impressions* (total views including repeats)
- For TikTok where reach isn't always available: use (likes + comments + shares) ÷ views × 100
- For GBP: engagement = (clicks + calls + directions) ÷ views × 100

Engagement rate lets you compare a post that reached 200 people against one
that reached 2,000 — absolute numbers alone are misleading.

For dental practices in SE Florida, these are rough benchmarks:
- Instagram posts: 1–3% is average; 3%+ is high-performing
- Instagram Reels: 3–8% is average; 8%+ is high-performing
- Facebook posts: 0.5–1.5% is average for organic
- TikTok: 5–10% is average; 10%+ is strong

Context matters: a practice with 500 followers will show different absolute
numbers than one with 5,000. Always compare *rates*, not totals.

## Analysis process

### 1 — Load context
Read the approved campaign brief to understand:
- What the campaign was trying to achieve (the goal)
- Which service was the focus
- What offer or CTA was running
- Which patient persona was targeted
- What content mix was planned (pillar percentages, post count per platform)

This tells you what *should* have happened. The data tells you what *did*.

### 2 — Ingest and normalize platform data
For each platform CSV export:
- Map column names to the standard metrics (each platform uses different labels)
- Calculate engagement rate for every post
- Tag each post with its content pillar (match to campaign brief idea bank or
  content calendar — if a post isn't tagged, infer from the topic and format)
- Note post format: Reel / Carousel / Static / GBP post / Short / Story

Save the normalized data to:
`data/processed/performance-metrics/{client-slug}/{YYYY-MM}_{client-slug}_metrics-normalized.csv`

See `references/platform-column-mapping.md` for the column names each
platform uses in their native exports.

### 3 — Identify top and bottom performers
Rank all posts by engagement rate. The top 5 and bottom 5 go into the report.

For each top performer, answer: *why did this work?*
- Hook type (Question / Bold Statement / Local Angle / etc.)
- Format (Reel outperforming carousels? Educational posts outperforming promotional?)
- Topic (which services generated the most engagement?)
- Timing (day of week, time of day if available)
- Trend tie-in (was a trend involved?)

For each bottom performer, answer: *what can we learn?*
- Was the hook weak? (started with "We" or the practice name?)
- Was it the format? (static image where a Reel would have done better?)
- Was the topic low-interest for this audience?
- Was the CTA confusing or missing?

Avoid attributing underperformance to vague causes ("just didn't resonate").
Be specific — this is what makes the lessons usable next month.

### 4 — Analyze pillar performance
Group all posts by content pillar and calculate:
- Number of posts published per pillar
- Average reach per pillar
- Average engagement rate per pillar

Compare actual pillar distribution to the plan in the campaign brief.
Flag significant deviations — if the plan called for 25% Education but only
15% Education was produced, note why and whether it affected results.

The pillar with the highest average engagement rate tells you what this
audience responds to most. Feed this directly into next month's brief.

### 5 — Pull GBP and search performance (if available)

**From GBP dashboard export or DataForSEO Business Data API:**
Fill the GBP table in the report: searches, clicks, calls, directions, reviews.
Note review velocity (new reviews this month vs. last).

**Optional DataForSEO check for search visibility trends:**
```bash
AUTH=$(echo -n "$DATAFORSEO_LOGIN:$DATAFORSEO_PASSWORD" | base64)
curl -s -X POST "https://api.dataforseo.com/v3/serp/google/maps/live/advanced" \
  -H "Authorization: Basic $AUTH" \
  -H "Content-Type: application/json" \
  -d '[{
    "keyword": "[priority service] [city]",
    "location_name": "Miami,Florida,United States",
    "language_name": "English"
  }]'
```
Note the practice's current local pack position — if it changed vs. last month,
flag it. GBP posting frequency directly affects local pack rankings.

### 6 — Assess campaign verdict
Compare results against the campaign's stated goal from the brief:

- **Acquisition campaign**: Did new patient inquiries or GBP calls increase?
- **Trust campaign**: Did follower growth or profile visits increase?
- **Service campaign**: Did engagement on service-specific content increase?
- **Seasonal campaign**: Did the seasonal content outperform baseline?

Be direct with the verdict: *Exceeded* / *On track* / *Underperformed* —
and give a one-sentence reason grounded in data, not hedging.

### 7 — Write content lessons
The most valuable section of the report. Pull 3 "what worked" and 3
"what underperformed" lessons — each grounded in a specific data point.

Good lesson: "Educational carousels averaged 4.2% engagement vs. 1.8% for
static service posts — the audience saves and shares 'how it works' content
more than offer-focused content."

Bad lesson: "Reels did well this month." (Not actionable.)

### 8 — Write next-month recommendations
Ground every recommendation in this month's data. For each recommendation:
- State what the data showed
- State what to do differently or more of
- Link it to the campaign type for next month

At minimum, recommend:
- Which content pillar to increase (highest engagement rate pillar)
- Which format to prioritize (best-performing format this month)
- 1–2 content experiments to test (specific, testable, measurable)
- Any compliance or PHI flags to carry forward

### 9 — Update knowledge base
This step runs every month, no exceptions. Knowledge compounds — skipping it
wastes the lessons.

- **Winning hooks** → append to `_context/clients/{client-slug}/hook-library.md`
  (mark with `[Confirmed high-signal — {month}]`)
- **Losing patterns** → append to `logs/skill-improvement-backlog.md`
- **Audience insights** → update `_context/clients/{client-slug}/patient-personas.md`
  with any new behavioral signals observed in comments or saves
- **Trend performance** → if a trend-based post was in the top 5, log in
  `data/processed/trend-scores/` with actual outcome vs. predicted score

### 10 — Fill the report template and save
Fill `_templates/monthly-report-template.md` with all analysis from steps 1–9.

Save to:
```
reports/monthly-performance-reports/{client-slug}/{YYYY-MM}_{client-slug}_performance-report_v1.md
```

Every section should be filled — leave nothing as a placeholder except
lead/conversion data if the practice hasn't provided it (mark clearly:
"Practice to provide").

End with: `**Status:** Draft — Needs review`

Append to `logs/decisions.md`:
```
## {date} — Campaign Report Complete: {client-name} — {month}
- Campaign verdict: [Exceeded/On track/Underperformed]
- Top pillar: [pillar name] at [N]% avg engagement rate
- Best post: [brief description] — [N]% engagement rate
- Key lesson: [one sentence]
- Next month priority: [one recommendation]
```

## Output
```
data/processed/performance-metrics/{client-slug}/
  {YYYY-MM}_{client-slug}_metrics-normalized.csv

reports/monthly-performance-reports/{client-slug}/
  {YYYY-MM}_{client-slug}_performance-report_v1.md

logs/
  decisions.md  ← handoff notes appended

_context/clients/{client-slug}/
  hook-library.md  ← winning hooks appended
```

For platform column name mappings, see `references/platform-column-mapping.md`.
