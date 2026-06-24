---
name: data-visualization
description: |
  Use when asked to visualize campaign data, create charts for a report,
  turn metrics into visuals, build a dashboard summary, make data look
  better for a client, prepare a presentation deck, or add charts to a
  monthly report — even if the user says "can you make this more visual,"
  "I need something to show the client," or "can you chart this out."
  Also use after campaign-report completes and visuals are needed for the
  client package. Works without coding — produces markdown tables, ASCII
  previews, and specs for Canva, Google Sheets, or Datawrapper.
---

# data-visualization

Turns campaign performance data into client-ready visuals without writing code.
The output is always immediately usable: markdown tables drop straight into
reports, ASCII charts preview the data in the terminal, and tool specs give
Canva or Google Sheets exactly what they need to build the real chart.

No data visualization skill is complete without the right chart for the right
data. Leading with the wrong chart type (a pie chart for trend data, a line
chart for category comparison) is more confusing to clients than a plain table.
Chart type selection is the most important decision here.

## Inputs

Pull from the campaign-report output in:
`data/processed/performance-metrics/{client-slug}/{YYYY-MM}_{client-slug}_metrics-normalized.csv`

Also reference the monthly report at:
`reports/monthly-performance-reports/{client-slug}/`

From `_context/clients/{client-slug}/brand-style-guide.md`, extract:
- Brand primary and accent colors (hex values for chart styling)
- Typography preference (for chart title and label fonts)

## Chart type selection

The chart type depends on what the data is trying to say. When in doubt,
a clean table beats a confusing chart.

| Data type | Best chart | Avoid |
|---|---|---|
| Trend over time (followers, GBP calls by month) | Line chart | Bar, pie |
| Comparison across categories (platform engagement rates, pillar performance) | Bar chart (vertical) | Line, pie |
| Ranking (top/bottom posts) | Horizontal bar chart | Line, pie |
| Composition (content pillar mix %) | Donut chart | Line, bar |
| Two metrics together (reach vs. engagement rate) | Scatter or dual-axis bar | Pie |
| Single month summary (one number per metric) | Metric card / table | Any chart |

For dental marketing reports, the most useful charts are:
1. **Month-over-month engagement rate by platform** (bar)
2. **Follower growth over time** (line)
3. **Pillar performance comparison** (horizontal bar)
4. **GBP conversion actions over time** (line: calls + directions + clicks)
5. **Top 5 posts ranked by engagement rate** (horizontal bar)

## How to produce each visualization

### Markdown table (universal — always produce this first)

Every visualization starts as a markdown table. This is immediately usable in
the report, renders in GitHub, Notion, and most documentation tools, and serves
as the data source for any chart tool.

Format with alignment, change indicators, and benchmark labels:
```markdown
| Platform | Posts | Avg. Reach | Avg. ER | vs. Last Month | Benchmark |
|---|---:|---:|---:|---:|---|
| Instagram Reels | 8 | 2,340 | 8.3% | +2.1% ▲ | HIGH |
| Instagram Feed | 12 | 890 | 2.8% | -0.4% ▼ | AVG |
| Facebook | 10 | 1,200 | 1.1% | +0.2% ▲ | AVG |
| GBP | 8 | — | 4.2% | +1.8% ▲ | HIGH |
```

Change indicators: ▲ (improved) ▼ (declined) → (flat, ±0.2%)
Benchmark labels: HIGH / AVG / LOW per `references/benchmarks.md`

### ASCII preview chart (terminal-friendly)

After the markdown table, produce an ASCII bar chart for quick visual scanning.
This is especially useful when reviewing in the terminal before the deck is built.

Bar width = (metric value / max value) × 20 characters.

```
Instagram Reels   ████████████████████ 8.3% ▲
Instagram Feed    ███████░░░░░░░░░░░░░ 2.8% ▼
Facebook          ████░░░░░░░░░░░░░░░░ 1.1% ▲
GBP               █████████████░░░░░░░ 4.2% ▲
```

For trend data, show direction with a simple sparkline pattern using
▁▂▃▄▅▆▇█ characters (lowest to highest values across the period).

### Google Sheets chart spec

Google Sheets is the fastest path to a real chart. Provide:
1. The data table formatted for direct paste (tab-separated or CSV)
2. Chart configuration instructions

```
GOOGLE SHEETS SPEC — Pillar Performance Bar Chart

Data to paste (copy from row 1):
Pillar	Posts	Avg ER
Education	5	4.2%
Trust	4	3.8%
Service Demand	5	2.1%
Local Community	2	5.9%
Team Personality	2	3.1%
Promotional	2	1.4%

Chart settings:
- Chart type: Bar chart
- X-axis: Pillar
- Y-axis: Avg ER (format as percentage)
- Sort: Descending by Avg ER (Local Community first)
- Color: [brand primary color hex] for all bars
- Title: "Content Pillar Performance — [Month] [Year]"
- Legend: Off
- Gridlines: Horizontal only, light grey
```

### Canva chart spec

Canva's chart tool accepts pasted data. Provide the data and design direction
in the format Canva expects.

```
CANVA SPEC — Follower Growth Line Chart

Chart type: Line chart
Add to Canva: Charts element → Line chart → Edit data

Data to paste:
Month,Followers
Jan,1240
Feb,1380
Mar,1510
Apr,1690
May,1820
Jun,1980

Design settings:
- Line color: [brand primary hex]
- Background: White or brand light background
- Title: "Follower Growth — [Practice Name]"
- Font: [from brand-style-guide.md] or Montserrat as default
- Remove legend (single line needs no legend)
- Add data point labels if chart allows
```

### Datawrapper spec

Datawrapper produces the most professional-looking charts. Use it for
client-facing presentations and strategy decks.

```
DATAWRAPPER SPEC — GBP Conversion Actions

Chart type: Line chart (multi-line)
URL: datawrapper.de → New chart → Line chart

CSV to upload:
Month,Phone Calls,Direction Requests,Website Clicks
Jan,42,18,95
Feb,51,22,108
Mar,48,19,102
...

Settings:
- Title: "Google Business Profile Conversions — [Client Name]"
- Colors: Phone Calls = [brand primary], Directions = [brand accent], Clicks = grey
- Y-axis label: "Monthly Actions"
- Highlight the most recent month
- Add annotation on highest-converting month
- Export: PNG 1200px wide for report, SVG for deck
```

## Standard visualization set for monthly report

Produce these five visuals for every monthly report. They cover the full
story clients need to understand: how the audience grew, what content worked,
what the platforms delivered, and where GBP stands.

1. **Platform engagement comparison** (bar) — engagement rate by platform
2. **Follower growth trend** (line) — month-over-month if historical data available
3. **Pillar performance ranking** (horizontal bar, sorted high to low)
4. **Top 5 posts** (horizontal bar, by engagement rate)
5. **GBP conversion summary** (table or line if multi-month)

For each: produce markdown table + ASCII preview + the relevant tool spec.

## Output

Save all visualization outputs to:
```
reports/monthly-performance-reports/{client-slug}/
  {YYYY-MM}_{client-slug}_visualizations.md    ← all tables + ASCII + specs

presentations/{client-slug}/
  {YYYY-MM}_{client-slug}_chart-specs.md        ← tool specs only (clean copy for designer)
```

The `visualizations.md` file is the working file — messy, complete, includes all
three formats for each chart. The `chart-specs.md` is the clean handoff to
whoever builds the actual charts in Canva, Google Sheets, or Datawrapper.

For benchmark reference and color palette defaults, see `references/viz-defaults.md`.
