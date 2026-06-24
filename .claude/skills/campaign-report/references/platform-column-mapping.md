# Platform Column Mapping Reference

Native export column names per platform and their standard equivalents.
Use this when normalizing CSVs in Step 2.

## Instagram (Meta Business Suite export)

| Native column | Standard name | Notes |
|---|---|---|
| Reach | reach | Unique accounts |
| Impressions | impressions | Total views |
| Likes | likes | |
| Comments | comments | |
| Shares | shares | |
| Saves | saves | High-signal for education content |
| Profile visits | profile_visits | Intent signal |
| Link clicks | link_clicks | Bio link |
| Video views | video_views | Reels only |
| Average watch time | avg_watch_time_sec | Reels only |
| Publication date | published_at | |
| Post type | format | Reel / Carousel / Image / Story |

**Engagement rate formula:** (likes + comments + saves + shares) ÷ reach × 100

---

## Facebook (Meta Business Suite export)

| Native column | Standard name | Notes |
|---|---|---|
| Reach | reach | |
| Impressions | impressions | |
| Reactions | likes | Includes all reaction types |
| Comments | comments | |
| Shares | shares | |
| Link clicks | link_clicks | |
| Post clicks | post_clicks | All click types |
| Video views | video_views | 3-second views |
| 1-Minute video views | video_views_1min | More meaningful than 3s |
| Publication date | published_at | |
| Post type | format | Video / Photo / Link / Story |

**Engagement rate formula:** (reactions + comments + shares) ÷ reach × 100

---

## TikTok (TikTok Analytics export)

| Native column | Standard name | Notes |
|---|---|---|
| Video views | video_views | Primary metric |
| Unique viewers | reach | Not always available |
| Likes | likes | |
| Comments | comments | |
| Shares | shares | |
| Average watch time | avg_watch_time_sec | |
| Watch time | total_watch_time_sec | |
| New followers | new_followers | Per-video attribution |
| Publication date | published_at | |

**Engagement rate formula (when reach unavailable):**
(likes + comments + shares) ÷ video_views × 100

---

## YouTube Shorts (YouTube Studio export)

| Native column | Standard name | Notes |
|---|---|---|
| Views | video_views | |
| Watch time (hours) | total_watch_time_hours | |
| Average view duration | avg_watch_time_sec | Convert from hh:mm:ss |
| Subscribers gained | new_followers | |
| Likes | likes | |
| Comments | comments | |
| Shares | shares | |
| Impressions | impressions | |
| Impressions click-through rate | ctr_pct | |
| Publication date | published_at | |

**Engagement rate formula:** (likes + comments + shares) ÷ views × 100

---

## Google Business Profile (GBP Insights export)

| Native column | Standard name | Notes |
|---|---|---|
| Total searches | gbp_searches | Direct + discovery |
| Direct searches | gbp_direct_searches | Searched by name |
| Discovery searches | gbp_discovery_searches | Found via category/service |
| Website clicks | gbp_website_clicks | |
| Phone calls | gbp_phone_calls | High-intent conversion |
| Direction requests | gbp_directions | High-intent conversion |
| Photo views | gbp_photo_views | |
| Post views | gbp_post_views | Per GBP post |
| New reviews | gbp_new_reviews | Count during period |
| Average rating | gbp_rating | Current, not monthly |

**GBP engagement rate formula:**
(website_clicks + phone_calls + directions) ÷ gbp_searches × 100

---

## SE Florida Dental Engagement Benchmarks

Use these as context when labeling posts High / Average / Low performing.

| Platform | Format | Low | Average | High |
|---|---|---|---|---|
| Instagram | Static post | < 1% | 1–3% | > 3% |
| Instagram | Reel | < 3% | 3–8% | > 8% |
| Instagram | Carousel | < 2% | 2–5% | > 5% |
| Facebook | Any post | < 0.5% | 0.5–1.5% | > 1.5% |
| TikTok | Video | < 3% | 3–10% | > 10% |
| YouTube Shorts | Short | < 2% | 2–6% | > 6% |
| GBP | Monthly | < 2% | 2–5% | > 5% |

Note: New accounts and accounts under 1,000 followers often see higher
engagement rates — factor follower count into context when comparing
across months.
