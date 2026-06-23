# AI Marketing Team for Private Dental Practices in Southeast Florida

**Project type:** Claude Code AI Marketing Team 
**Primary focus:** Social media marketing for private dental practices in Southeast Florida  
**Prepared for:** Erick Sicard  
**Date:** June 23, 2026

---

## Source Notes

This file consolidates the full plan generated in the previous response and adds source references for the tool, compliance, and integration assumptions. Key source categories include Anthropic Claude Code skills documentation, Anthropic's public skills repository, Google NotebookLM Help, Apify, Firecrawl, Gemini image-generation documentation, HHS HIPAA marketing guidance, and Meta advertising policies.

---

## 1. Project Goal

Create an **AI Marketing Team for private dental practices in Southeast Florida**: a Claude Code project that coordinates specialized skills, subagents, research workflows, scraping pipelines, trend detection, creative generation, compliance review, and reporting to produce consistent, local, HIPAA-aware social media marketing assets for dental practices.

The business objective is to help private dental practices in Miami-Dade, Broward, and Palm Beach generate more visibility, trust, consultations, and patient leads through research-backed social media content, local competitive intelligence, and repeatable campaign execution.

---

## 2. Assumptions and Scope

### Assumptions

- Target clients are **private dental practices**, not DSOs, hospitals, or national dental chains.
- Primary geography is **Southeast Florida**, especially Miami-Dade, Broward, and Palm Beach.
- Core platforms are **Instagram, Facebook, TikTok, YouTube Shorts, Google Business Profile posts/reviews, and practice websites**.
- Content should support services such as general dentistry, implants, orthodontics, cosmetic dentistry, emergency dentistry, oral surgery, veneers, whitening, crowns, clear aligners, and preventive care.
- The system should assist with marketing execution but should **not autonomously publish content without human approval**.
- All patient photos, testimonials, case stories, reviews, and before-and-after content require strict review because HIPAA marketing rules can require authorization for use or disclosure of protected health information.
- Dental practices should maintain a written social media and marketing-approval policy.

### In scope

- Social media research.
- Dental trend detection.
- Local competitor tracking.
- Content strategy.
- Caption writing.
- Reel/TikTok/Shorts scripting.
- Creative briefs.
- AI-assisted social graphics.
- Lead magnets.
- Landing page outlines.
- Campaign briefs.
- Monthly reporting.
- NotebookLM-assisted research.
- Claude Code skills, agents, hooks, and project memory.

### Out of scope for Version 1

- Autonomous posting to social platforms.
- Autonomous ad spend changes.
- Scraping private, logged-in, paywalled, or restricted content.
- Diagnosis, treatment planning, or patient-specific medical advice.
- Replacing legal/compliance review.
- Direct CRM automation unless added later through GoHighLevel or another approved system.

---

## 3. Success Criteria

A successful first version should accomplish the following:

- **Research intake:** Ingest one practice’s brand guide, services, location, competitors, offers, past content, and preferred tone.
- **Competitor intelligence:** Track 10–25 local competitors per practice across public social profiles, websites, Google reviews, and public video content.
- **Trend detection:** Produce a weekly trend report with scored opportunities and clear “use / monitor / reject” decisions.
- **Content calendar:** Generate a 30-day social calendar with at least 12–20 usable post ideas, including short-form video concepts, educational posts, trust-building posts, service posts, and local community posts.
- **Creative pipeline:** Generate social copy, creative briefs, design prompts, and image-generation instructions aligned with the Brand Style Guide.
- **Compliance gate:** Flag risky claims, PHI exposure, unverifiable before-and-after claims, review-response issues, and unsafe dental advice before approval.
- **Reporting:** Produce a monthly performance report summarizing top posts, weak posts, engagement patterns, content lessons, next-month recommendations, and new content tests.
- **Team operation:** Agents hand off work through files, not chat memory alone.
- **Project memory:** Every campaign, trend decision, competitor insight, content experiment, and performance lesson is saved into the project knowledge base.

---

## 4. Marketing Task Map

| Core function | Objective | Inputs | Outputs | Success criteria |
|---|---|---|---|---|
| Client intake and brand context | Build a reliable client profile before any content is created. | Website, services, offers, doctor bios, brand guide, photos, location, target patients, past posts. | `client-profile.md`, `brand-summary.md`, `service-priority-map.md`. | Claude can accurately describe the practice, audience, offers, tone, and restrictions. |
| Local market research | Understand the patient market in Southeast Florida. | City, service area, demographics, languages, patient concerns, search behavior, competitors. | Market brief, patient personas, local content angles. | Content ideas feel local, culturally relevant, and service-specific. |
| Competitive intelligence | Identify what nearby dental practices are posting, promoting, and ranking for socially. | Competitor list, public social profiles, websites, reviews, YouTube channels. | Competitor content matrix, offer map, posting frequency, engagement observations. | Finds repeatable competitor patterns and content gaps. |
| Social media mining | Extract reusable social insights from public social content. | Captions, hashtags, formats, hooks, comments, views, likes, shares, posting dates. | Hook library, FAQ list, objection list, engagement triggers. | Produces content ideas grounded in observed audience behavior. |
| Trend detection | Detect meaningful dental and local social trends before they become stale. | TikTok/Instagram/YouTube signals, competitor posts, patient questions, industry news. | Weekly trend report with score, rationale, recommended action. | Only trends above the threshold enter production. |
| Content strategy | Translate research into a monthly campaign plan. | Market brief, competitor gaps, service priorities, trend scores, offers. | Monthly content calendar, campaign themes, channel mix. | Balanced mix of education, trust, service demand, local relevance, and conversion. |
| Copywriting | Produce captions, scripts, hooks, CTAs, and post text. | Campaign brief, service page, brand voice, trend report, compliance rules. | Captions, Reel scripts, carousel copy, story copy, post variants. | Copy is clear, local, dental-specific, non-alarming, and brand-aligned. |
| Creative direction | Turn strategy into visual concepts. | Brand guide, creative brief, winning content patterns, asset library. | Design brief, image prompt, layout direction, visual checklist. | Creative is consistent with the practice and platform-native. |
| AI social design | Generate or prepare visual assets. | Design brief, brand style guide, approved prompt, image model instructions. | Draft images, thumbnails, concept boards, asset variants. | Outputs match brand style and avoid patient/privacy risk. |
| Lead magnet creation | Create educational assets that convert attention into leads. | Service priority, patient objections, FAQs, offer, landing page goal. | PDF/checklist/guide outline, copy, CTA, design brief. | Asset supports a clear conversion path. |
| Landing page support | Create conversion-focused landing page content for social campaigns. | Offer, service, audience, lead magnet, objections, proof elements. | LP wireframe, headline options, CTA copy, FAQ block. | Page aligns with social promise and avoids unsupported claims. |
| Reputation and review mining | Convert public review themes into content insights without exposing PHI. | Public reviews, recurring compliments, complaints, objections. | Trust themes, content angles, service improvement notes. | Insights are generalized and never identify patients. |
| Reporting and optimization | Learn from performance and improve the next content cycle. | Platform metrics, published content, campaign plan, lead data if available. | Monthly report, next experiments, winning hooks, losing patterns. | Clear recommendations for next month. |
| Compliance and QA | Prevent risky content before it reaches the client or public. | Draft content, creative, claims, patient references, reviews, offers. | Approval checklist, redlines, reject/approve decision. | No PHI, no implied diagnosis, no misleading claims, no unsafe dental advice. |

---

## 5. Skill Architecture

Claude Code skills are the right unit for repeatable marketing procedures because skills are self-contained folders with `SKILL.md` instructions, optional scripts, and resources that Claude loads only when relevant.

### Core custom skills

| Skill | Purpose | Inputs | Outputs | Dependencies | Trigger / use case |
|---|---|---|---|---|---|
| `client-context-ingestion` | Convert raw client materials into structured context. | Website, intake form, service list, photos, offers, bios. | Client profile, tone notes, service map. | Brand context folder. | First setup for every practice. |
| `brand-voice-normalizer` | Standardize tone, vocabulary, and visual constraints. | Brand guide, sample posts, doctor tone, target audience. | Voice guide, approved phrases, banned phrases. | `brand-guidelines` skill. | Before copy or creative production. |
| `market-research` | Research local dental market conditions and patient demand. | City, service area, treatments, competitors. | Market brief, patient personas, local insights. | Web research, NotebookLM. | New client, quarterly refresh, proposal. |
| `keyword-research` | Identify search-informed social topics and patient intent. | Service priorities, locations, Google Ads/SEO data. | Topic clusters, FAQ map, service-intent matrix. | SEO tools or manual research. | Campaign planning and lead magnet creation. |
| `competitive-intel-scraper` | Collect public competitor data. | Competitor URLs, social handles, Google profiles. | Raw JSON/CSV, normalized competitor matrix. | Apify, Firecrawl. | Weekly competitor update. |
| `social-media-mining` | Mine posts, captions, hooks, hashtags, comments, and formats. | Public social datasets, competitor posts, hashtags. | Hook library, format patterns, audience questions. | Apify exports. | Before monthly content planning. |
| `trend-detection-scoring` | Score social trends using a repeatable matrix. | Trend candidates, platform metrics, competitor patterns. | Trend score, threshold decision, recommendation. | Social mining, market research. | Weekly trend report. |
| `campaign-brief` | Convert insights into a campaign plan. | Client profile, trends, competitors, offer, services. | Campaign brief, channel strategy, content pillars. | Market research, trend scoring. | Monthly planning. |
| `social-copy` | Write captions, hooks, scripts, story copy, and CTAs. | Campaign brief, brand voice, service notes. | Caption variants, Reel scripts, carousel text. | Compliance rules. | Content production. |
| `short-form-video-script` | Create Reels, TikToks, and Shorts scripts. | Trend report, service topic, CTA, practice personality. | 15s/30s/45s scripts, shot list, voiceover. | Social copy, creative brief. | Video-first content. |
| `blog-writer` | Produce supporting educational blog content. | Topic cluster, service page, research packet. | Blog draft, meta title, excerpt, FAQ. | Market research, SEO. | When social campaigns need deeper content. |
| `lead-magnet` | Create educational lead magnets. | Patient objection, service focus, offer. | Checklist, PDF outline, guide copy, CTA. | Doc skills, brand guide. | Implant, orthodontic, cosmetic, emergency campaigns. |
| `lp-builder` | Draft landing page content and wireframes. | Offer, lead magnet, campaign brief. | LP outline, hero copy, FAQ, CTA copy. | Web artifacts, brand guide. | Conversion campaign setup. |
| `ad-creative` | Build paid social creative concepts. | Offer, audience, platform, constraints. | Static/video ad concepts, hooks, visual prompts. | Campaign brief, compliance review. | Paid social expansion. |
| `social-creative` | Build organic social creative concepts. | Calendar topic, format, brand style. | Design brief, graphic prompt, layout notes. | Brand guide, Nano Banana MCP. | Organic content production. |
| `social-creative-designer` | Generate social image prompts and draft assets with Nano Banana. | Research-backed design brief, brand guide, platform format. | Image prompts, image variants, QA notes. | `.mcp.json`, Gemini image model, brand assets. | When visual assets are needed. |
| `campaign-report` | Analyze performance and explain what changed. | Post metrics, content calendar, campaign goals. | Monthly report, content lessons, next tests. | Analytics export. | Monthly reporting. |
| `data-visualization` | Turn performance data into charts and tables. | CSV/XLSX metrics, campaign data. | Charts, tables, report visuals. | Spreadsheet/report tools. | Reports and presentations. |
| `branded-deck` | Create client-facing strategy/report decks. | Campaign brief, report, brand guide. | Slide outline, deck content, visual structure. | `brand-guidelines`, `doc-coauthoring`, deck tools. | Proposals, reviews, strategy presentations. |
| `notebooklm-research-os` | Package, export, and reuse NotebookLM research inside Claude Code. | Source pack, research question, client/prospect context. | Research digest, source map, insight log, memory updates. | NotebookLM, Google Docs/Drive export or manual source export. | Deep research, proposals, technical research, trend analysis. |
| `skill-discovery` | Identify new skills worth building from repeated tasks. | Logs, repeated prompts, agent failures, SOP gaps. | Skill candidate brief, priority score, draft `SKILL.md`. | Skill creator, project logs. | Weekly project improvement review. |
| `compliance-review` | Review dental marketing content before approval. | Draft copy, creative, claims, reviews, patient references. | Risk score, required edits, approval status. | HIPAA/ADA policy notes, client rules. | Before client delivery or publishing. |

### Trend Signal Scoring Matrix

Use a **0–5 score** for each criterion. Convert to a weighted 100-point total.

| Criterion | Weight | 0 score | 3 score | 5 score |
|---|---:|---|---|---|
| Signal velocity | 15 | No growth | Moderate growth | Rapid growth in last 7–14 days |
| Cross-platform presence | 10 | One weak signal | Present on 2 platforms | Present on 3+ platforms |
| Dental relevance | 15 | Not dental-relevant | Adaptable to dental | Directly dental/oral-health relevant |
| Local Southeast Florida fit | 10 | Not locally relevant | Some local fit | Strong Miami/Broward/Palm Beach fit |
| Service alignment | 10 | No service connection | Supports a secondary service | Supports priority service/offer |
| Patient intent value | 10 | Pure entertainment | Awareness/education | Consultation or lead intent |
| Competitive whitespace | 10 | Many local competitors already using it | Some usage | Few/no local competitors using it well |
| Creative feasibility | 10 | Hard to produce | Can produce with effort | Easy to produce with existing assets |
| Compliance safety | 5 | High risk | Needs edits | Low risk |
| Evidence quality | 5 | Anecdotal | Some measurable evidence | Strong data from multiple sources |

### Thresholds

- **85–100:** Priority trend. Use in next 7 days.
- **70–84:** Meaningful trend. Add to monthly content calendar.
- **50–69:** Watchlist. Monitor but do not produce yet.
- **0–49:** Reject.
- **Hard veto:** Reject any trend if compliance safety is **0–2**, evidence quality is **0–2**, or the trend encourages unsafe dental behavior, DIY treatment, diagnosis, shame-based messaging, or unverifiable claims.

---

## 6. Agent Architecture

Claude Code subagents should group related skills into bounded responsibilities. The goal is not to create many overlapping “creative” agents, but to create clear handoff lanes.

| Agent | Mission | Owned skills | Boundaries | Handoffs | Quality checks |
|---|---|---|---|---|---|
| **Orchestrator / Marketing Lead** | Own the end-to-end campaign workflow and decide which agent works next. | `campaign-brief`, workflow routing, final synthesis. | Does not scrape, write all copy, or approve compliance alone. | Receives all outputs; sends tasks to each agent. | Confirms brief completeness, dependencies, and final package. |
| **Market Researcher** | Understand market, audience, competitors, services, and patient demand. | `market-research`, `keyword-research`, `notebooklm-research-os`. | Does not create final posts or designs. | Sends insight brief to Strategist and Trend Scout. | Sources must be cited or stored in research files. |
| **Competitive Intelligence Analyst** | Track competitors and summarize what they are doing. | `competitive-intel-scraper`, `social-media-mining`. | Does not decide campaign strategy alone. | Sends competitor matrix to Trend Scout and Strategist. | Validates data freshness, source type, and public-access status. |
| **Trend Scout** | Detect, score, and prioritize trends. | `trend-detection-scoring`, `social-media-mining`. | Does not produce final content. | Sends scored trend list to Strategist. | Applies scoring matrix and hard veto rules. |
| **Campaign Strategist** | Translate research into monthly content strategy. | `campaign-brief`, `branded-deck`. | Does not generate final assets. | Sends campaign brief to Content Creator and Creative Designer. | Checks service mix, funnel balance, local relevance, and offer clarity. |
| **Content Creator** | Produce written content and scripts. | `social-copy`, `short-form-video-script`, `blog-writer`, `lead-magnet`, `lp-builder`. | Does not approve clinical claims or design final visuals. | Sends drafts to Compliance Reviewer and Creative Designer. | Checks readability, CTA clarity, brand voice, and content format. |
| **Creative Designer** | Produce creative concepts, prompts, layouts, and AI image drafts. | `ad-creative`, `social-creative`, `social-creative-designer`. | Does not invent claims, patient stories, or medical results. | Sends visuals to Compliance Reviewer and Orchestrator. | Checks brand alignment, platform format, and visual consistency. |
| **Data Analyst** | Analyze performance and recommend iterations. | `campaign-report`, `data-visualization`. | Does not rewrite strategy without Strategist approval. | Sends insights to Orchestrator, Strategist, and Knowledge Librarian. | Checks data integrity and separates vanity metrics from conversion indicators. |
| **Compliance / QA Reviewer** | Prevent risky dental, health, privacy, and platform-policy issues. | `compliance-review`. | Does not create campaigns; only approves, rejects, or redlines. | Sends required edits to Content Creator, Designer, or Strategist. | Checks PHI, testimonials, claims, before/after, offers, and tone. |
| **Knowledge Librarian** | Maintain project memory, NotebookLM exports, SOPs, and skill improvements. | `notebooklm-research-os`, `skill-discovery`, template maintenance. | Does not make marketing decisions. | Updates `_context`, `research`, `_sop`, and lessons learned. | Ensures insights are dated, sourced, and retrievable. |

---

## 7. Team Workflow

### End-to-end workflow

1. **Client intake**
   - Orchestrator invokes `client-context-ingestion`.
   - Output goes to `_context/clients/{client-name}/`.

2. **Research phase**
   - Market Researcher builds a market and audience brief.
   - NotebookLM workflow is used for deep research packets: market research, competitor research, clinical-topic research, proposal research, and skill-building research.
   - Knowledge Librarian saves final research summaries to `research/` and curated insights to `_context/`.

3. **Competitive intelligence phase**
   - Competitive Intelligence Analyst runs scraping workflows.
   - Raw exports go to `data/raw/`.
   - Normalized insights go to `research/competitive-intel/`.

4. **Trend detection phase**
   - Trend Scout extracts candidate trends.
   - Each trend is scored with the Trend Signal Scoring Matrix.
   - Only trends scoring **70+** and passing hard veto rules move forward.

5. **Campaign strategy phase**
   - Campaign Strategist creates the monthly campaign brief.
   - Brief includes content pillars, channel mix, campaign objectives, service priorities, and CTAs.

6. **Content production phase**
   - Content Creator drafts captions, scripts, carousel copy, stories, blog support, lead magnets, and landing page copy.

7. **Creative production phase**
   - Creative Designer uses research-backed creative briefs and brand guidelines to generate image prompts or draft assets.
   - Nano Banana should be used only after the creative brief is approved.

8. **Compliance review**
   - Compliance Reviewer checks all copy, images, claims, patient references, offers, and before/after concepts.
   - Anything involving patient identity, testimonials, treatment outcomes, or reviews gets elevated.

9. **Client-ready package**
   - Orchestrator assembles the calendar, captions, scripts, creative briefs, image files, lead magnets, and recommendations.

10. **Performance reporting**
    - Data Analyst reviews platform metrics.
    - Campaign Strategist converts lessons into next-month priorities.
    - Knowledge Librarian updates memory and reusable templates.

### Escalation paths

- **Compliance risk:** Escalate to Compliance Reviewer; reject until rewritten.
- **Clinical claim uncertainty:** Escalate to dentist/client for validation.
- **Low data confidence:** Escalate to Market Researcher or Competitive Intelligence Analyst.
- **Scraping failure:** Use Firecrawl for websites, Apify alternative actor, manual capture, or mark the source unavailable.
- **Creative off-brand:** Return to Creative Designer with Brand Style Guide reference.
- **Trend conflict:** Trend Scout and Strategist decide whether it is a content opportunity, watchlist item, or rejection.

### Quality control gates

- Gate 1: Brand/context completeness.
- Gate 2: Research source quality.
- Gate 3: Trend score threshold.
- Gate 4: Campaign brief approval.
- Gate 5: Copy and creative QA.
- Gate 6: Compliance approval.
- Gate 7: Final client package review.
- Gate 8: Performance learning update.

---

## 8. Project Folder Structure

Recommended project root:

```text
ai-marketing-team-dental-sefl/
  CLAUDE.md
  README.md
  .mcp.json
  .gitignore

  .claude/
    agents/
      orchestrator.md
      market-researcher.md
      competitive-intel-analyst.md
      trend-scout.md
      campaign-strategist.md
      content-creator.md
      creative-designer.md
      data-analyst.md
      compliance-qa-reviewer.md
      knowledge-librarian.md

    skills/
      client-context-ingestion/
        SKILL.md
      brand-voice-normalizer/
        SKILL.md
      market-research/
        SKILL.md
      keyword-research/
        SKILL.md
      competitive-intel-scraper/
        SKILL.md
      social-media-mining/
        SKILL.md
      trend-detection-scoring/
        SKILL.md
      campaign-brief/
        SKILL.md
      social-copy/
        SKILL.md
      short-form-video-script/
        SKILL.md
      social-creative-designer/
        SKILL.md
      lead-magnet/
        SKILL.md
      lp-builder/
        SKILL.md
      campaign-report/
        SKILL.md
      data-visualization/
        SKILL.md
      notebooklm-research-os/
        SKILL.md
        scripts/
        templates/
        references/
      compliance-review/
        SKILL.md
      skill-discovery/
        SKILL.md

    hooks/
      pre-edit-compliance-check.sh
      secret-scan.sh
      output-location-check.sh
      client-data-warning.sh

  _context/
    global/
      southeast-florida-market.md
      dental-service-taxonomy.md
      compliance-rules.md
      platform-rules.md
      content-pillars.md

    clients/
      client-name/
        brand-context.md
        brand-style-guide.md
        brand-voice-guide.md
        doctor-bios.md
        service-offers.md
        approved-claims.md
        banned-claims.md
        competitors.md
        asset-inventory.md

    growth-marketing/
      funnel-model.md
      content-strategy-principles.md
      social-platform-playbook.md
      lead-magnet-playbook.md

  _sop/
    campaign-performance-reporting.md
    social-content-production.md
    competitor-intelligence.md
    trend-detection.md
    notebooklm-research-workflow.md
    compliance-review.md
    client-approval-workflow.md

  _templates/
    campaign-brief.md
    monthly-content-calendar.csv
    social-post-template.md
    reel-script-template.md
    creative-brief-template.md
    trend-report-template.md
    competitor-matrix.csv
    monthly-report-template.md
    lead-magnet-outline.md
    landing-page-wireframe.md
    proposal-research-brief.md

  data/
    raw/
      apify/
      firecrawl/
      platform-exports/
    processed/
      competitor-matrices/
      trend-scores/
      performance-metrics/
    schemas/
      competitor-post.schema.json
      trend-signal.schema.json
      campaign-report.schema.json

  research/
    notebooklm/
      source-packs/
      exported-notes/
      research-digests/
    market/
    competitors/
    trends/
    academic-technical/
    proposals/
    skill-research/

  social/
    calendars/
    captions/
    scripts/
    stories/
    carousels/
    reels/
    approvals/

  creative/
    briefs/
    prompts/
    generated/
    approved/
    rejected/

  ads/
    social-ads/
    google-ads/
    creative-tests/

  pages/
    landing-pages/
    service-pages/
    lead-magnet-pages/

  reports/
    weekly-trend-reports/
    monthly-performance-reports/
    competitor-reports/
    client-ready/

  presentations/
    proposals/
    strategy-decks/
    reporting-decks/

  seo/
    keyword-research/
    content-briefs/
    blog-support/

  logs/
    decisions.md
    changelog.md
    agent-handoffs.md
    skill-improvement-backlog.md
```

### Folder rules

- `_context/` contains durable knowledge.
- `_sop/` contains repeatable operating procedures.
- `_templates/` contains reusable output formats.
- `data/raw/` contains unedited exports.
- `data/processed/` contains cleaned, normalized files.
- `research/` contains analysis and source-backed insight.
- `social/` contains production copy.
- `creative/` contains visual briefs, prompts, and assets.
- `reports/` contains client-facing and internal reporting.
- `logs/` contains project memory, decisions, and improvement backlog.

---

## 9. Claude.md Specification

The project-wide `CLAUDE.md` should contain at minimum:

### Project identity

- Name: **AI Marketing Team for Dental Practices in Southeast Florida**
- Purpose: Social media research, strategy, creative production, trend detection, and reporting for private dental practices.
- Business objective: Help private practices improve visibility, trust, patient education, consultation demand, and lead generation.

### Target market

- Private dental practices.
- Southeast Florida.
- English and Spanish content support.
- Services: implants, orthodontics, cosmetic dentistry, emergency dentistry, preventive care, restorative dentistry, oral surgery, clear aligners, veneers, crowns, whitening.

### Non-negotiable compliance rules

- Never reveal or imply patient identity.
- Never use patient photos, testimonials, cases, reviews, or before-and-after content without explicit approval and documentation.
- Never provide diagnosis or treatment-specific advice.
- Never claim guaranteed results.
- Never shame patients.
- Never use fear-based medical language.
- Never invent credentials, awards, reviews, prices, or treatment outcomes.
- Flag any claim requiring dentist approval.

### Context-loading rules

Claude Code should read, in this order:

1. `CLAUDE.md`
2. `_context/global/compliance-rules.md`
3. `_context/global/dental-service-taxonomy.md`
4. `_context/clients/{client}/brand-context.md`
5. `_context/clients/{client}/brand-style-guide.md`
6. `_context/clients/{client}/approved-claims.md`
7. Relevant SOP
8. Relevant template

### Agent roster

Define each subagent, its owned skills, boundaries, and allowed outputs.

### Skill invocation rules

- Use `client-context-ingestion` before client work.
- Use `trend-detection-scoring` before trend-based content.
- Use `compliance-review` before anything leaves the project.
- Use `notebooklm-research-os` for deep research, proposals, technical research, and reusable knowledge.
- Use `social-creative-designer` only after strategy and creative brief are approved.

### Output standards

- Always store outputs in the correct folder.
- Use filenames with date, client, topic, and version.
- Include source notes for research-backed claims.
- Include a final “Ready for client / Needs review / Rejected” status.

### Review gates

- Research gate.
- Strategy gate.
- Copy gate.
- Creative gate.
- Compliance gate.
- Final package gate.
- Memory update gate.

### Tool policy

- Apify for public social data extraction.
- Firecrawl for websites, pages, and structured web extraction.
- NotebookLM for source-grounded research synthesis.
- Nano Banana/Gemini image generation for draft social visuals only after brand and compliance constraints are loaded.
- Hooks for deterministic safety checks.

---

## 10. Tooling and Integrations

### Anthropic Marketplace / official skills

Install the official Anthropic skills first as foundation skills. Anthropic’s public skills repository lists examples across creative, design, development, communication, and document workflows, and includes folders such as `algorithmic-art`, `brand-guidelines`, `canvas-design`, `doc-coauthoring`, `internal-comms`, `mcp-builder`, `skill-creator`, `theme-factory`, and `web-artifacts-builder`.

Use them this way:

- `skill-creator`: Create and evaluate custom project skills.
- `brand-guidelines`: Enforce brand voice, style, logo, color, and typography rules.
- `canvas-design` / Canva-design use case: Support visual layout thinking and brand-compliant design direction.
- `doc-coauthoring`: Build proposals, reports, lead magnets, and strategy documents.
- `internal-comms`: Standardize client updates, approval messages, and internal notes.
- `mcp-builder`: Build or configure MCP integrations.
- `theme-factory`: Create design systems and visual themes for dental brands.
- `web-artifacts-builder`: Create landing page mockups, dashboards, and web-based previews.
- `algorithmic-art`: Optional; useful for abstract backgrounds or visual systems, not core dental marketing.

### NotebookLM integration

NotebookLM should function as the **Research Powerhouse**, while Claude Code remains the **builder, orchestrator, and memory maintainer**.

NotebookLM supports source-based workflows such as adding/discovering sources, creating notes, mind maps, audio overviews, video overviews, infographics, flashcards/quizzes, and slide decks. This makes it useful for synthesizing large source packs before Claude Code turns the findings into repeatable project artifacts.

Recommended workflow:

1. Claude Code creates a `source-pack.md` for a research question.
2. User or automation adds sources to NotebookLM.
3. NotebookLM is used to explore and synthesize the research.
4. Claude Code imports the exported notes or summaries into `research/notebooklm/exported-notes/`.
5. Knowledge Librarian converts the notes into:
   - `research-digest.md`
   - `insight-log.md`
   - `source-map.md`
   - `skill-improvement-candidates.md`
6. Only curated, verified insights move into `_context/`.

Use NotebookLM for:

- Social media mining synthesis.
- Market research.
- Competitive intelligence.
- Trend analysis.
- Client/prospect research.
- Proposal research.
- Academic and technical research.
- Identifying new skills to build.
- Improving project memory and knowledge retrieval.

### Apify

Use Apify for public social scraping and structured social data collection. Apify provides a marketplace of Actors for scraping websites, automating the web, and feeding AI systems with web data.

Recommended scraping workflows:

| Workflow | Target sources | Frequency | Intended outputs |
|---|---|---:|---|
| Local competitor profile scan | Instagram, TikTok, Facebook public pages, YouTube channels, Google Business Profiles where accessible | 2x weekly | Posting frequency, format mix, hooks, engagement, offers |
| Short-form video scan | TikTok public search, Instagram Reels public accounts, YouTube Shorts | Weekly | Trending hooks, topics, audio/style notes, video formats |
| YouTube dental scan | Competitor channels, dental education channels, local dental searches | Weekly/monthly | Video titles, views, comments, transcripts/subtitles where available |
| Review mining | Public review content from approved sources | Weekly | Patient language, objections, praise themes, service issues |
| Proposal competitor scan | Prospect’s top 5–10 competitors | Per proposal | Competitive positioning brief |
| Trend watchlist | Dental hashtags, local hashtags, treatment-related terms | Daily weekday or 3x weekly | Candidate trends for scoring |

### Firecrawl

Use Firecrawl for websites, competitor service pages, blogs, location pages, landing pages, and public web research. Firecrawl can support scraping, crawling, searching, markdown extraction, and structured extraction. It also provides an MCP path for Claude Code-style workflows.

Recommended Firecrawl workflows:

| Workflow | Target sources | Frequency | Intended outputs |
|---|---|---:|---|
| Competitor website crawl | Service pages, blog, offer pages, landing pages | Monthly | Service positioning, offers, proof claims, content gaps |
| Prospect research | Prospect website, competitors, local market pages | Per proposal | Proposal research packet |
| Landing page analysis | Client and competitor campaign pages | Per campaign | LP copy gaps, CTA issues, offer structure |
| Blog/topic research | Dental service topics and local content | Monthly | Blog/social support briefs |
| Skill research | Tool docs, platform docs, API docs | As needed | New skill or integration brief |

### Nano Banana / Gemini image generation

Use Nano Banana/Gemini image generation for **draft social visuals, concept variations, thumbnails, stylized educational graphics, and brand-aligned image ideas**.

Implementation constraint:

- Configure through `.mcp.json` or a dedicated MCP/server wrapper.
- Inputs must include brand guide, creative brief, platform size, forbidden imagery, and compliance constraints.
- Never generate realistic patient before-and-after images.
- Never imply an actual patient result.
- Never use a real person’s likeness without permission.
- Keep generated assets in `creative/generated/` until approved.

### Platform and compliance constraints

Meta’s health and wellness advertising policy includes restrictions around health-related and cosmetic procedure advertising, so social creative and paid-social assets must avoid shame-based, negative self-perception, or inappropriate personal-attribute framing.

---

## 11. Implementation Roadmap

### Phase 1 — Foundation setup

1. Create the repository: `ai-marketing-team-dental-sefl`.
2. Add the folder structure from Section 8.
3. Add `.gitignore` rules for secrets, raw exports, generated assets, and client-sensitive data.
4. Create `README.md` with project purpose and operating rules.
5. Create initial `CLAUDE.md`.

**Milestone:** Claude Code can load the project and understand the mission, folder rules, and safety constraints.

### Phase 2 — Install official skills

1. Add Anthropic skills marketplace.
2. Install:
   - `skill-creator`
   - `algorithmic-art`
   - `brand-guidelines`
   - `canvas-design`
   - `doc-coauthoring`
   - `internal-comms`
   - `mcp-builder`
   - `theme-factory`
   - `web-artifacts-builder`
3. Verify each skill is discoverable.
4. Create a skill inventory file in `_context/global/installed-skills.md`.

**Milestone:** Claude Code has base document, design, brand, MCP, and skill-building capabilities.

### Phase 3 — Build global context

1. Create `_context/global/dental-service-taxonomy.md`.
2. Create `_context/global/southeast-florida-market.md`.
3. Create `_context/global/compliance-rules.md`.
4. Create `_context/global/content-pillars.md`.
5. Create `_context/growth-marketing/social-platform-playbook.md`.

**Milestone:** The system understands the dental category, region, content pillars, and compliance rules.

### Phase 4 — Create templates

Build the reusable templates:

- Campaign brief.
- Competitor matrix.
- Trend report.
- Monthly content calendar.
- Reel script.
- Social caption.
- Creative brief.
- Lead magnet.
- Landing page wireframe.
- Monthly report.
- Proposal research brief.

**Milestone:** Agents can write into consistent formats.

### Phase 5 — Build custom skills

Build custom skills in this order:

1. `client-context-ingestion`
2. `brand-voice-normalizer`
3. `market-research`
4. `keyword-research`
5. `competitive-intel-scraper`
6. `social-media-mining`
7. `trend-detection-scoring`
8. `campaign-brief`
9. `social-copy`
10. `short-form-video-script`
11. `social-creative-designer`
12. `lead-magnet`
13. `lp-builder`
14. `campaign-report`
15. `data-visualization`
16. `notebooklm-research-os`
17. `compliance-review`
18. `skill-discovery`

**Milestone:** Every repetitive workflow is represented as a skill.

### Phase 6 — Build subagents

Create agents in `.claude/agents/`:

1. Orchestrator / Marketing Lead
2. Market Researcher
3. Competitive Intelligence Analyst
4. Trend Scout
5. Campaign Strategist
6. Content Creator
7. Creative Designer
8. Data Analyst
9. Compliance / QA Reviewer
10. Knowledge Librarian

**Milestone:** Claude Code can delegate tasks to bounded roles.

### Phase 7 — Configure integrations

1. Add Firecrawl MCP for web scraping and website research.
2. Add Apify API or MCP-style integration for public social scraping.
3. Add Nano Banana/Gemini image generation wrapper.
4. Add any analytics import method: CSV first, APIs later.
5. Store credentials in environment variables, never in repo.

**Milestone:** Research, scraping, and creative tools are available from Claude Code.

### Phase 8 — Build data schemas

Create schemas for:

- Competitor profile.
- Competitor post.
- Trend signal.
- Trend score.
- Social content item.
- Creative brief.
- Campaign report.
- Compliance review.

**Milestone:** Agents can exchange structured data reliably.

### Phase 9 — Build scraping workflows

Start with three workflows:

1. Competitor website crawl with Firecrawl.
2. YouTube/Shorts data extraction with Apify.
3. Instagram/TikTok/Facebook public profile scan with Apify or approved actors.

**Milestone:** The system can produce a competitor intelligence brief.

### Phase 10 — Build trend detection engine

1. Collect candidate signals.
2. Normalize signals.
3. Apply Trend Signal Scoring Matrix.
4. Save results to `data/processed/trend-scores/`.
5. Generate `reports/weekly-trend-reports/`.

**Milestone:** The system can decide which trends are meaningful.

### Phase 11 — Build NotebookLM research workflow

1. Create `research/notebooklm/source-packs/`.
2. Build source-pack template.
3. Build export/import SOP.
4. Build `notebooklm-research-os` skill.
5. Build Knowledge Librarian workflow for memory updates.

**Milestone:** NotebookLM research becomes reusable Claude Code memory.

### Phase 12 — Build content production workflow

1. Generate monthly campaign brief.
2. Produce content calendar.
3. Produce captions and scripts.
4. Produce creative briefs.
5. Run compliance review.
6. Assemble client-ready package.

**Milestone:** The system can produce one full month of social content.

### Phase 13 — Build reporting workflow

1. Import platform metrics.
2. Normalize performance data.
3. Identify winners and losers.
4. Generate monthly report.
5. Update content lessons.

**Milestone:** The system improves from performance data.

### Phase 14 — Add hooks and safety gates

Add hooks for:

- Secret scanning.
- Preventing edits to protected context files without approval.
- Warning on PHI-like content.
- Enforcing output folder rules.
- Running compliance review before files enter `client-ready/`.

**Milestone:** The system has deterministic safety checks.

### Phase 15 — Pilot with one practice

Use one dental practice as the pilot.

Deliver:

- Client context profile.
- Competitor matrix.
- Trend report.
- 30-day content calendar.
- 10 captions.
- 5 Reel scripts.
- 5 creative briefs.
- 2 lead magnet ideas.
- 1 landing page outline.
- 1 monthly report template.

**Milestone:** First complete operating version.

### Phase 16 — Improve and systematize

1. Log every failure.
2. Identify repeated prompts.
3. Turn repeated procedures into skills.
4. Update templates.
5. Refine agents.
6. Add evals for quality, compliance, and output structure.

**Milestone:** The project becomes a repeatable AI marketing operating system.

---

## 12. Risks and Gaps

| Risk / gap | Why it matters | Mitigation |
|---|---|---|
| PHI exposure | Dental social content can accidentally reveal patient identity. | Mandatory compliance review, approved-claims file, no patient content without authorization. |
| Unsafe dental trends | Some viral dental trends encourage harmful DIY behavior. | Trend hard veto for unsafe behavior, clinical review for health claims. |
| Scraping limits or platform changes | Social platforms change access rules and data availability. | Use public data only, approved actors, Firecrawl fallback, manual capture option. |
| Weak trend data | Viral signals can be noisy or irrelevant. | Use weighted scoring, cross-platform validation, evidence-quality threshold. |
| Over-agent complexity | Too many agents can slow execution. | Keep agents bounded; Orchestrator controls workflow; skills handle repeatable tasks. |
| Brand inconsistency | AI-generated content may drift from the practice’s tone. | Brand voice guide, banned phrases, brand QA, creative review. |
| Generic dental content | Content may feel like any dental office anywhere. | Require local angle, practice-specific services, doctor personality, Southeast Florida context. |
| Hallucinated claims | AI may invent awards, credentials, pricing, results, or patient outcomes. | Approved-claims file; claims not in source context are rejected. |
| AI image misuse | Generated visuals may imply real patients or real outcomes. | No realistic before/after generation; no patient likeness; store generated assets as drafts only. |
| Poor performance attribution | Social metrics alone may not prove patient acquisition. | Track content metrics, lead metrics, landing page behavior, calls/forms when available. |
| Compliance policy drift | Platform and healthcare advertising rules change. | Quarterly compliance update SOP; maintain `platform-rules.md`. |
| NotebookLM output overtrust | NotebookLM summaries still need verification. | Keep source maps; import only curated insights into project memory. |
| Client approval bottleneck | Human review can delay publishing. | Create approval packets with clear status: approve, edit, reject. |
| Missing analytics access | Some practices may not provide platform data. | Start with manual CSV exports; add APIs later. |
| Local cultural mismatch | Southeast Florida audiences vary by city and language. | Maintain local personas and bilingual content variants for each client. |

---

## Implementation Notes for Claude Code

### Suggested first Claude Code prompt

```text
You are working inside the repository ai-marketing-team-dental-sefl.

First, read CLAUDE.md, README.md, _context/global/compliance-rules.md, _context/global/dental-service-taxonomy.md, and the folder structure.

Your task is to help me implement the AI Marketing Team project for private dental practices in Southeast Florida.

Start by verifying the folder structure. Then create or update the foundation files in this order:
1. README.md
2. CLAUDE.md
3. _context/global/dental-service-taxonomy.md
4. _context/global/southeast-florida-market.md
5. _context/global/compliance-rules.md
6. _context/global/content-pillars.md
7. _sop/social-content-production.md
8. _sop/trend-detection.md
9. _sop/competitor-intelligence.md
10. _templates/campaign-brief.md
11. _templates/trend-report-template.md
12. _templates/monthly-content-calendar.csv

Do not create client-specific content yet. Focus only on the reusable operating system foundation.
After each file is created or updated, summarize what changed and why.
```

### Suggested second Claude Code prompt

```text
Now create the initial custom skills for this project.

Use the official Anthropic skill structure. Each skill must have its own folder under .claude/skills/ and a clear SKILL.md.

Create these first five skills:
1. client-context-ingestion
2. market-research
3. competitive-intel-scraper
4. trend-detection-scoring
5. compliance-review

For each skill, include:
- Name
- Description
- When to use
- Required inputs
- Step-by-step process
- Output files
- Quality checklist
- Failure modes
- Dependencies

Do not create scripts yet unless strictly necessary. Prioritize clear, reliable SKILL.md instructions.
```

### Suggested third Claude Code prompt

```text
Now create the first version of the agent team.

Create these files under .claude/agents/:
1. orchestrator.md
2. market-researcher.md
3. competitive-intel-analyst.md
4. trend-scout.md
5. campaign-strategist.md
6. content-creator.md
7. creative-designer.md
8. data-analyst.md
9. compliance-qa-reviewer.md
10. knowledge-librarian.md

Each agent file must define:
- Mission
- Owned skills
- Inputs it expects
- Outputs it produces
- What it must not do
- Handoff points
- Quality checks
- Escalation rules

Make sure agent responsibilities do not overlap unnecessarily.
```

---

## Recommended Source References

- Claude Code Skills Documentation: https://code.claude.com/docs/en/skills
- Anthropic Public Skills Repository: https://github.com/anthropics/skills
- Claude Code Hooks Guide: https://code.claude.com/docs/en/hooks-guide
- Google NotebookLM Help: https://support.google.com/notebooklm/?hl=en
- NotebookLM Official Product Page: https://notebooklm.google/
- Apify Platform: https://apify.com/
- Apify Social Media Scrapers Category: https://apify.com/store/categories/social-media-scrapers
- Firecrawl Official Site: https://www.firecrawl.dev/
- Firecrawl API Documentation: https://docs.firecrawl.dev/
- Gemini API Documentation: https://ai.google.dev/gemini-api/docs
- Gemini Native Image Generation: https://ai.google.dev/gemini-api/docs/image-generation
- HHS HIPAA Marketing Guidance: https://www.hhs.gov/hipaa/for-professionals/privacy/guidance/marketing/index.html
- Meta Health and Wellness Advertising Policy: https://www.facebook.com/business/help/2489235377779939
- Meta Advertising Standards: https://transparency.meta.com/policies/ad-standards/
