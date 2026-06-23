# Foundation Build Plan — AI Marketing Team for Dental Practices SE Florida

**Date:** June 23, 2026  
**Prepared for:** Erick Sicard  

---

## Context

The repo `/Users/erick/projects/ai-marketing-team` exists with only the planning document.
No `.claude/`, no folder structure, no CLAUDE.md, no commits yet.

Goal: Build the foundation right — establish the operating system the agents and skills will run on
before any client work or content production begins. Firecrawl and NotebookLM are available;
Apify is not yet. A first client is in mind but materials are not ready.

The planning document (`ai_marketing_team_dental_sefl_plan.md`) is the authoritative spec for all
naming conventions, folder rules, and content.

---

## What "Foundation Right" Means

Claude Code needs to be able to:
1. Load the project and know the mission, market, and compliance rules
2. Know where every type of output lives
3. Have reusable templates agents write into
4. Have the 5 most critical skills before any client work runs
5. Understand the agent team structure and handoff lanes

---

## Phase 1 — Repo structure + .gitignore

Create the full folder tree from Section 8 of the plan document.
Key top-level folders: `.claude/`, `_context/`, `_sop/`, `_templates/`, `data/`, `research/`,
`social/`, `creative/`, `ads/`, `pages/`, `reports/`, `presentations/`, `seo/`, `logs/`

Create `.gitignore` to exclude:
- `.env`, secrets, API keys
- `data/raw/` (unedited scrape exports — can be large/sensitive)
- `creative/generated/` (AI image drafts — not committed until approved)
- `research/notebooklm/source-packs/` (may contain client-sensitive materials)
- Standard OS/editor noise (`.DS_Store`, etc.)

**Milestone:** `ls` shows the full operating structure.

---

## Phase 2 — README.md and CLAUDE.md

### README.md
- Project purpose (one paragraph)
- What this system does and does not do
- Folder map (one line per folder)
- How to run the system (context loading order, first prompts)
- Tool dependencies (Firecrawl, NotebookLM; Apify when added)

### CLAUDE.md
Must include everything from Section 9 of the plan:
- Project identity and business objective
- Target market (SE Florida private practices, services list, English + Spanish)
- Non-negotiable compliance rules (the 8 rules from the plan)
- Context-loading order (CLAUDE.md → compliance-rules → taxonomy → client brand → SOP → template)
- Agent roster summary (name, mission, owned skills)
- Skill invocation rules (when each key skill must be called)
- Output standards (folder, filename convention, source notes, status field)
- Review gates (Research → Strategy → Copy → Creative → Compliance → Final → Memory)
- Tool policy (Firecrawl, NotebookLM, Apify when available, Nano Banana/Gemini)

**Milestone:** Claude can load the project and accurately describe the mission, rules, and structure
from CLAUDE.md alone.

---

## Phase 3 — Global context files

Create under `_context/global/`:

1. **`compliance-rules.md`** — The 8 non-negotiables expanded with examples and edge cases.
   Include HIPAA marketing rule summary, Meta health/wellness policy constraints, ADA advertising
   guidelines. This file is read before any content is produced.

2. **`dental-service-taxonomy.md`** — Full service list organized by category (preventive,
   restorative, cosmetic, orthodontic, surgical, emergency, implants). Include patient-facing
   terminology vs. clinical terminology pairs. Used by all content skills.

3. **`southeast-florida-market.md`** — Market context: Miami-Dade, Broward, Palm Beach geography,
   demographic notes, bilingual content needs (English/Spanish), seasonal patterns, local
   cultural angles relevant to dental marketing.

4. **`content-pillars.md`** — The 5–6 content pillars (education, trust/social proof, service demand,
   local community, team personality, promotional). Include what each pillar achieves, example post
   types, and guardrails per pillar.

5. **`platform-rules.md`** — Per-platform constraints: Instagram, Facebook, TikTok, YouTube Shorts,
   Google Business Profile. Format specs, posting cadence norms, what works per platform,
   compliance notes per platform.

**Milestone:** Claude can answer detailed questions about the dental category, region, and compliance
from these files without hallucinating.

---

## Phase 4 — Templates

Create under `_templates/`:

1. `campaign-brief.md` — Campaign name, objective, services, audience, offer, content pillars,
   channel mix, CTA, timeline, compliance notes, sign-off status.

2. `trend-report-template.md` — Trend name, source platforms, signal date, score breakdown
   (each criterion with weight), total score, threshold decision, recommended action, hard veto check.

3. `monthly-content-calendar.csv` — Columns: date, platform, content type, pillar, topic/hook,
   caption draft link, creative brief link, compliance status, approval status.

4. `reel-script-template.md` — Duration, hook (0–3s), body (3–25s), CTA (25–30s), shot list,
   voiceover notes, caption, hashtag suggestions, compliance check.

5. `social-post-template.md` — Platform, pillar, service, hook, body, CTA, hashtags, image brief,
   compliance status, approval status.

6. `creative-brief-template.md` — Post type, platform format, visual concept, brand colors,
   forbidden elements, image prompt, QA notes, approval status.

7. `competitor-matrix.csv` — Competitor name, platforms, posting frequency, top formats,
   top services promoted, offer patterns, engagement observations, content gaps.

8. `monthly-report-template.md` — Period, top 5 posts, bottom 5 posts, engagement patterns,
   content lessons, next-month recommendations, experiments to run.

**Milestone:** Agents have consistent, dated, status-tracked formats to write into.

---

## Phase 5 — First 5 critical skills

Create under `.claude/skills/`. Each skill gets its own folder with `SKILL.md`.
Build in this order — each skill is a prerequisite for the ones that follow:

1. **`compliance-review`** — Most important skill. Checks PHI exposure, testimonial risk,
   clinical claims, before/after imagery, misleading offers, platform policy violations.
   Output: risk score, required edits, approve/reject/redline status.
   Must run before anything leaves the project.

2. **`client-context-ingestion`** — Converts raw client materials (website, intake form, service list,
   bios, offers, sample posts) into structured files under `_context/clients/{client-name}/`.
   Output: `brand-context.md`, `brand-style-guide.md`, `brand-voice-guide.md`,
   `service-offers.md`, `competitors.md`.

3. **`trend-detection-scoring`** — Applies the 10-criterion weighted scoring matrix from the plan.
   Takes trend candidates and produces a scored report with threshold decision and hard veto check.
   Output: `data/processed/trend-scores/` and `reports/weekly-trend-reports/`.

4. **`market-research`** — Researches local dental market conditions, patient demand, and audience
   context using Firecrawl and NotebookLM. Output: market brief, patient personas, local angles.

5. **`social-copy`** — Produces caption variants, Reel hooks, carousel copy, and story copy aligned
   with brand voice and campaign brief. Requires compliance-review before output is finalized.

**Milestone:** Claude can be given a client brief and know how to run intake, research, trend scoring,
and produce first-draft copy with a compliance gate — the core loop.

---

## Phase 6 — Agent definitions (first 4)

Create under `.claude/agents/`. Start with the 4 agents that form the core workflow loop.
Build the remaining 6 after the pilot client's materials arrive.

1. **`orchestrator.md`** — End-to-end workflow owner. Routes tasks, checks dependencies,
   assembles final client package. Does not create content directly.

2. **`market-researcher.md`** — Owns `market-research`, `keyword-research`,
   `notebooklm-research-os`. Sends insight brief to Trend Scout and Strategist.

3. **`compliance-qa-reviewer.md`** — Owns `compliance-review`. Only approves, rejects, or
   redlines. Elevates patient identity, testimonial, and clinical claim issues.

4. **`content-creator.md`** — Owns `social-copy`, `short-form-video-script`, `lead-magnet`.
   Sends drafts to Compliance Reviewer before finalization.

---

## Execution order summary

| Phase | Deliverable | Milestone |
|---|---|---|
| 1 | Folder structure + `.gitignore` | `ls` shows full operating structure |
| 2 | `README.md` + `CLAUDE.md` | Claude describes mission + rules from CLAUDE.md alone |
| 3 | 5 global context files | Claude answers compliance + taxonomy questions accurately |
| 4 | 8 templates | Agents have consistent formats to write into |
| 5 | 5 critical skills | Core intake → research → copy → compliance loop works |
| 6 | 4 core agent definitions | Orchestrator routes correctly through the team |

Commit after each phase. Don't start the next phase until the milestone is verified.

---

## Verification checkpoints

**After Phase 2:** Open a new Claude Code session in this repo. Ask: "What is this project, who is it for,
and what are the compliance rules?" Claude should answer accurately from CLAUDE.md alone.

**After Phase 3:** Ask: "What dental services do private practices in Miami-Dade typically offer?"
and "What are the HIPAA rules around patient testimonials in dental marketing?"

**After Phase 5:** Run the `compliance-review` skill on a sample caption with a borderline claim.
Confirm it flags correctly and produces a risk score + required edits.

**After Phase 6:** Ask the Orchestrator to describe the workflow from client intake to content delivery.
Confirm it routes correctly through Market Researcher → Compliance Reviewer → Content Creator.
