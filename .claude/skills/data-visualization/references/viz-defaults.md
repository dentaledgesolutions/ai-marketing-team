# Visualization Defaults

## Default color palette (when brand colors not available)

| Role | Color | Hex | Use for |
|---|---|---|---|
| Primary | Dental blue | #2B6CB0 | Main metric, primary line |
| Accent | Teal | #38B2AC | Secondary metric |
| Success | Green | #38A169 | Above benchmark |
| Warning | Amber | #D69E2E | At benchmark |
| Underperform | Coral | #E53E3E | Below benchmark |
| Neutral | Light grey | #E2E8F0 | Gridlines, backgrounds |
| Text | Dark grey | #2D3748 | Labels, titles |

Always replace with client brand colors from `brand-style-guide.md` when available.

## Benchmark labels for engagement rate

Applied in tables as HIGH / AVG / LOW. Based on SE Florida dental averages.

| Platform | Format | LOW | AVG | HIGH |
|---|---|---|---|---|
| Instagram | Reel | < 3% | 3–8% | > 8% |
| Instagram | Carousel | < 2% | 2–5% | > 5% |
| Instagram | Static | < 1% | 1–3% | > 3% |
| Facebook | Any | < 0.5% | 0.5–1.5% | > 1.5% |
| TikTok | Video | < 3% | 3–10% | > 10% |
| YouTube Shorts | Short | < 2% | 2–6% | > 6% |
| GBP | Monthly | < 2% | 2–5% | > 5% |

## Sparkline character map

For trend data in tables, use UTF-8 block characters:
▁ = very low | ▂ = low | ▃ = below avg | ▄ = avg | ▅ = above avg | ▆ = high | ▇ = very high | █ = max

Map values to characters: divide range into 8 equal bands.

Example: Follower growth [1240, 1380, 1510, 1690, 1820, 1980]
Range: 1240–1980 (740 units, 92.5 per band)
→ ▁▂▃▅▆█ (displayed in one cell of the table)

## Chart dimensions by output type

| Output destination | Width | Height | Format |
|---|---|---|---|
| Monthly report (screen) | 800px | 450px | PNG |
| Client presentation (16:9 slide) | 1200px | 675px | PNG or SVG |
| Email or social share | 600px | 400px | PNG |
| Print report | 1600px | 900px | SVG preferred |

## Typography defaults (when brand fonts not specified)

- Chart title: Montserrat Bold, 16px
- Axis labels: Montserrat Regular, 12px
- Data labels: Montserrat Medium, 11px
- Source note: Montserrat Regular, 9px, grey

## Source note format

Always include at the bottom of every chart:
`Source: [Platform] analytics | [Month] [Year] | [Practice Name]`
