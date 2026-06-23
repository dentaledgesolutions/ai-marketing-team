# AI Marketing Team — Dental Practices, Southeast Florida

A Claude Code project that coordinates specialized skills, subagents, research workflows, scraping pipelines, trend detection, creative generation, compliance review, and reporting to produce consistent, local, HIPAA-aware social media marketing assets for private dental practices in Miami-Dade, Broward, and Palm Beach.

## What this system does

- Researches the local dental market, patient demand, and competitors
- Detects and scores social media trends before they go stale
- Generates monthly content calendars, captions, Reel scripts, and creative briefs
- Produces lead magnets and landing page copy for conversion campaigns
- Reviews all content for HIPAA compliance, PHI exposure, and platform policy issues
- Builds a cumulative knowledge base from every campaign and performance cycle

## What this system does NOT do

- Autonomously publish content to social platforms (human approval required at every gate)
- Autonomously change ad spend
- Scrape private, logged-in, paywalled, or restricted content
- Provide diagnosis, treatment planning, or patient-specific medical advice
- Replace legal or compliance counsel

---

## Folder Map

| Folder | Purpose |
|---|---|
| `.claude/` | Agent definitions, custom skills, hooks |
| `_context/` | Durable knowledge: global market context, client profiles, growth-marketing frameworks |
| `_sop/` | Standard operating procedures for every repeatable workflow |
| `_templates/` | Reusable output formats: briefs, calendars, scripts, reports |
| `data/` | Raw scrape exports (Apify, Firecrawl) and processed/normalized data |
| `research/` | NotebookLM digests, market analysis, competitor intel, trend research |
| `social/` | Production social copy: captions, scripts, stories, carousels, reels |
| `creative/` | Visual briefs, image prompts, generated drafts, approved assets |
| `ads/` | Paid social and Google Ads creative tests |
| `pages/` | Landing pages, service pages, lead magnet pages |
| `reports/` | Weekly trend reports, monthly performance reports, client-ready packages |
| `presentations/` | Proposals, strategy decks, reporting decks |
| `seo/` | Keyword research, content briefs, blog support |
| `logs/` | Decisions, changelog, agent handoffs, skill improvement backlog |

---

## How to run the system

### Context loading order

When starting any client task, Claude loads files in this order:

1. `CLAUDE.md`
2. `_context/global/compliance-rules.md`
3. `_context/global/dental-service-taxonomy.md`
4. `_context/clients/{client-name}/brand-context.md`
5. `_context/clients/{client-name}/brand-style-guide.md`
6. `_context/clients/{client-name}/approved-claims.md`
7. Relevant SOP from `_sop/`
8. Relevant template from `_templates/`

### Starting a new client

```
Run the `client-context-ingestion` skill.
Provide: website URL, services list, doctor bios, brand guide, sample posts, competitor names.
Output goes to: _context/clients/{client-name}/
```

### Monthly content cycle

```
1. Competitive Intelligence Analyst → research/competitors/
2. Trend Scout → data/processed/trend-scores/ + reports/weekly-trend-reports/
3. Campaign Strategist → _templates/campaign-brief.md
4. Content Creator → social/captions/, social/scripts/
5. Creative Designer → creative/briefs/, creative/prompts/
6. Compliance Reviewer → social/approvals/
7. Orchestrator → reports/client-ready/
```

### First-session prompt

```
Read CLAUDE.md, _context/global/compliance-rules.md, and _context/global/dental-service-taxonomy.md.
Then tell me: what is this project, who is it for, and what are the compliance rules?
```

---

## Tool dependencies

| Tool | Status | Purpose |
|---|---|---|
| Firecrawl | Active | Website and web research crawling |
| NotebookLM | Active | Deep research synthesis from source packs |
| Apify | Planned | Public social scraping (Instagram, TikTok, Facebook, YouTube) |
| Gemini / Nano Banana | Planned | Draft social image generation |

Credentials go in environment variables — never committed to the repo.

---

## Compliance non-negotiables

- Never reveal or imply patient identity
- Never use patient photos, testimonials, or before-and-after content without explicit written authorization
- Never claim guaranteed results
- Never invent credentials, awards, reviews, prices, or outcomes
- Always run `compliance-review` before anything leaves the project

See `_context/global/compliance-rules.md` for the full policy.
