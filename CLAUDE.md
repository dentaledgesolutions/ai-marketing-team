# AI Marketing Team — Dental Practices, Southeast Florida

An AI Marketing Team that coordinates Claude Code skills, subagents, research workflows, scraping pipelines, trend detection, creative generation, compliance review, and reporting to produce consistent, local, HIPAA-aware social media marketing assets for private dental practices in Southeast Florida.

## Quick Facts

- **Stack**: Claude Code, Apify, Firecrawl, DataForSEO, NotebookLM, Gemini (image generation), Markdown, JSON, Shell
- **Commands**: N/A — Claude Code configuration project
- **Target market**: Private dental practices in Miami-Dade, Broward, and Palm Beach counties
- **Languages**: English and Spanish content support
- **Platforms**: Instagram, Facebook, TikTok, YouTube Shorts, Google Business Profile, practice websites

## Key Directories

- `.claude/` — Agent definitions, custom skills, hooks
- `_context/` — Durable knowledge: global market context, client profiles, growth-marketing frameworks
- `_sop/` — Repeatable standard operating procedures
- `_templates/` — Reusable output formats (briefs, calendars, scripts, reports)
- `data/` — Raw scrape exports (Apify, Firecrawl) and processed/normalized research data
- `research/` — NotebookLM digests, market analysis, competitor intel, trend research
- `social/` — Production social copy: captions, scripts, stories, carousels, reels, approvals
- `creative/` — Visual briefs, image prompts, generated and approved assets
- `ads/` — Paid social and Google Ads creative tests
- `pages/` — Landing pages, service pages, lead magnet pages
- `reports/` — Weekly trend reports, monthly performance reports, client-ready packages
- `presentations/` — Proposals, strategy decks, reporting decks
- `seo/` — Keyword research, content briefs, blog support
- `logs/` — Decisions, changelog, agent handoffs, skill improvement backlog

## Claude's Rules

### Always

- Always run `client-context-ingestion` before any client work begins
- Always run `compliance-review` before anything leaves the project
- Always run `trend-detection-scoring` before trend-based content enters production
- Always store outputs in the correct folder with filenames containing date, client, topic, and version (e.g. `2026-06-23_smile-dental_implants-caption_v1.md`)
- Always include source notes for research-backed claims
- Always include a final status on every output: "Ready for client / Needs review / Rejected"
- Always use `social-creative-designer` only after strategy and creative brief are approved
- Always flag any claim that requires dentist validation before proceeding

### Never

- Never reveal or imply patient identity
- Never use patient photos, testimonials, cases, reviews, or before-and-after content without explicit approval and documentation
- Never provide diagnosis or treatment-specific advice
- Never claim guaranteed results
- Never shame patients or use fear-based medical language
- Never invent credentials, awards, reviews, prices, or treatment outcomes
- Never autonomously publish content without human approval
- Never scrape private, logged-in, paywalled, or restricted content

## Context Loading Order

When starting any client task, read in this order:

1. `CLAUDE.md` (this file)
2. `_context/global/compliance-rules.md`
3. `_context/global/dental-service-taxonomy.md`
4. `_context/clients/{client-name}/brand-context.md`
5. `_context/clients/{client-name}/brand-style-guide.md`
6. `_context/clients/{client-name}/approved-claims.md`
7. Relevant SOP from `_sop/`
8. Relevant template from `_templates/`

## Agent Roster

| Agent | Mission | Owned Skills |
|---|---|---|
| Orchestrator | End-to-end workflow owner; routes tasks, assembles final package | `campaign-brief`, workflow routing |
| Market Researcher | Research local market, audience, competitors, patient demand | `market-research`, `keyword-research`, `notebooklm-research-os` |
| Competitive Intelligence Analyst | Track competitors and summarize public content and positioning | `competitive-intel-scraper`, `social-media-mining` |
| Trend Scout | Detect, score, and prioritize social trends | `trend-detection-scoring`, `social-media-mining` |
| Campaign Strategist | Translate research into monthly content strategy | `campaign-brief`, `branded-deck` |
| Content Creator | Produce captions, scripts, carousels, lead magnets, landing pages | `social-copy`, `short-form-video-script`, `lead-magnet`, `lp-builder` |
| Creative Designer | Produce creative concepts, prompts, and draft AI visual assets | `ad-creative`, `social-creative`, `social-creative-designer` |
| Data Analyst | Analyze performance and recommend next-cycle improvements | `campaign-report`, `data-visualization` |
| Compliance / QA Reviewer | Prevent risky content from reaching the client or public | `compliance-review` |
| Knowledge Librarian | Maintain project memory, NotebookLM exports, SOPs, skill improvements | `notebooklm-research-os`, `skill-discovery` |

## Skill Invocation Rules

- Use `client-context-ingestion` before any work begins for a new client
- Use `trend-detection-scoring` before any trend enters the content calendar
- Use `compliance-review` before any copy, creative, or brief leaves the project
- Use `notebooklm-research-os` for deep research, proposals, technical topics, and reusable knowledge
- Use `social-creative-designer` only after the creative brief has been approved

## Review Gates

Every campaign passes through these gates in order:

1. **Research gate** — sources cited, data fresh, market brief complete
2. **Strategy gate** — campaign brief approved, pillars balanced, offer clear
3. **Copy gate** — captions, scripts, and hooks reviewed for brand voice and clarity
4. **Creative gate** — briefs and image prompts reviewed for brand alignment and platform fit
5. **Compliance gate** — all content approved by Compliance / QA Reviewer
6. **Final package gate** — client-ready folder assembled and status-stamped
7. **Memory update gate** — lessons, decisions, and insights saved to project knowledge base

## Tool Policy

- **DataForSEO** — primary source for keyword intelligence: search volume, keyword ideas, PAA via SERP API, Google Maps local pack, Google Trends velocity, competitor ranked keywords, domain rank overview, and Google Reviews data. Credentials: `DATAFORSEO_LOGIN` and `DATAFORSEO_PASSWORD` env vars. API calls made via curl in the main session — never hardcode credentials. Base URL: `https://api.dataforseo.com/v3/`
- **Firecrawl** — competitor content structure (page titles, H1s, FAQ content), practice websites, blogs, landing pages, public web research. Complements DataForSEO — Firecrawl reads content, DataForSEO provides search intelligence
- **NotebookLM** — source-grounded research synthesis; deep research packets and reusable knowledge
- **Apify** — public social scraping (Instagram, TikTok, Facebook, YouTube); never private or logged-in content
- **Gemini / Nano Banana** — draft social visuals only; never realistic patient before/after; never real patient likeness; assets stay in `creative/generated/` until approved
- **Credentials** — store all API keys in environment variables; never commit to repo

## Output Standards

- Folder: always the correct folder per output type (see Key Directories)
- Filename: `YYYY-MM-DD_{client}_{topic}_{type}_v{n}.{ext}`
- Source notes: include citation or source reference for any research-backed claim
- Status line: every output file ends with `**Status:** Ready for client / Needs review / Rejected`

## Domain Terms

- **PHI**: Protected Health Information — patient data protected under HIPAA; never used in marketing content without explicit written authorization
- **HIPAA**: Health Insurance Portability and Accountability Act — governs use of patient data in dental marketing
- **DSO**: Dental Service Organization — what target clients are NOT; focus is private independent practices only
- **SOP**: Standard Operating Procedure — repeatable workflow stored in `_sop/`
- **CTA**: Call to Action — conversion prompt in social copy (e.g., "Book a free consult")
- **GBP**: Google Business Profile — local search listing; one of the six core platforms
- **CPA**: Cost Per Acquisition — efficiency metric for paid campaigns
- **ROAS**: Return On Ad Spend — revenue divided by ad spend
- **CTR**: Click-Through Rate — clicks divided by impressions
- **CPM**: Cost Per Mille — cost per 1,000 impressions
- **SEO**: Search Engine Optimization — informs keyword research and blog content strategy
- **ADA**: American Dental Association — advertising guidelines referenced in compliance rules
- **SE Florida**: Southeast Florida — Miami-Dade, Broward, and Palm Beach counties; primary target geography
- **SEFL**: Southeast Florida — abbreviated form used in file naming conventions
