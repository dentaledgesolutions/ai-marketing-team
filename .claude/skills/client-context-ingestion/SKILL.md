# Skill: client-context-ingestion

## Description
Converts raw client materials — website, intake form, service list, doctor bios, brand guide, sample posts, and competitor names — into 9 structured context files under `_context/clients/{client-slug}/`. These files are the foundation every other skill and agent reads when working on client tasks. Nothing else runs before this.

## When to Use
- When onboarding a new dental practice client
- Before any research, strategy, or content work begins for a practice
- When client materials are updated (new services, new doctor, new offer, rebrand)
- When existing client context files are missing, incomplete, or stale

Run this skill before any other skill or agent in the client workflow.

## Required Inputs
Gather as many of the following as available before starting. Flag missing items for follow-up — do not invent or assume.

| Input | Priority | Notes |
|---|---|---|
| Practice website URL | Required | Crawl with Firecrawl |
| Practice name and all locations | Required | Include all locations if multi-site |
| Doctor name(s) and credentials | Required | Full name, DDS/DMD, board certs, years in practice |
| Services list | Required | May come from website or intake form |
| Target patient types | Required | Demographics, services they seek, primary concerns |
| Practice phone and booking URL | Required | Used in every CTA |
| Competitors (3–10 names or handles) | Required | For competitor matrix setup |
| Google Business Profile URL | Preferred | Reviews, Q&A, posts — rich patient language source |
| Insurance accepted | Preferred | PPO, HMO, Medicaid, self-pay — affects SE Florida audience targeting |
| Languages spoken | Preferred | English, Spanish, Creole — triggers bilingual content flag |
| Brand guide or style reference | Preferred | Colors, fonts, logo usage |
| Sample social posts (5–10) | Preferred | Recent content for first-pass tone analysis |
| Existing offers or promotions | Preferred | Verify before including in approved-claims.md |
| Banned phrases or sensitivities | Optional | Anything the practice has asked to avoid |
| Doctor headshots / team photos | Optional | Log in asset-inventory.md |
| Practice videos or reels | Optional | Log in asset-inventory.md |

## Process

### Step 1 — Crawl the practice website (Firecrawl)
Use Firecrawl to extract content from:
- Homepage (mission, tagline, differentiators)
- Services pages (full list with descriptions)
- About/team pages (doctor names, credentials, bios)
- Contact/locations (address, phone, hours)
- Any offer, special, or promotion pages
- Patient resources or FAQ pages

Save raw crawl output to: `data/raw/firecrawl/{client-slug}/website-crawl-{YYYY-MM-DD}.md`

If Firecrawl is blocked or the site is unavailable, use manual notes from the intake form and flag the website issue in `brand-context.md`.

### Step 2 — Extract Google Business Profile data
If a GBP URL is provided, crawl or manually capture:
- Current star rating and review count
- Representative public review themes (summarize language patterns — never copy patient names or identifying details)
- Any Q&A content visible publicly
- Recent GBP posts if present

Save to: `data/raw/firecrawl/{client-slug}/gbp-extract-{YYYY-MM-DD}.md`

If GBP URL is not provided, note as "GBP not provided — locate and add before market research phase."

### Step 3 — Extract review themes (PHI-safe)
From GBP reviews and any other public review sources (Google, Healthgrades, Zocdoc — public only):
- Identify recurring compliment themes (e.g., "gentle with anxious patients," "fast appointments")
- Identify recurring objection themes (e.g., "hard to reach by phone," "long wait times")
- Summarize in general language — never copy review text verbatim, never include reviewer names or any detail that could identify a patient

These themes feed content angles and trust-building messaging.

### Step 4 — Build practice identity and doctor profiles
From the crawl and provided materials, extract:

**Practice identity:**
- Full legal name and any DBA names
- All locations (address, city, county, zip)
- Phone number(s) formatted for CTAs (e.g., (305) 555-0100)
- Booking URL
- Google Business Profile URL
- Social handles (Instagram, Facebook, TikTok, YouTube)
- Website URL
- Founded year / years in practice
- Number of providers
- Practice type: solo / small group / multi-location
- Insurance: list accepted plans; mark "self-pay friendly" if applicable
- Languages: list all; flag bilingual = true if Spanish confirmed

**Bilingual flag:** If Spanish is confirmed, add `bilingual: true` to `brand-context.md`. All downstream skills check this flag to determine whether Spanish variants are needed.

**Doctor profiles (one block per doctor):**
- Full name and credentials (DDS, DMD, specialty board certifications)
- Years in practice
- Dental school and graduation year
- Specialties or advanced training
- Personal note or philosophy (from bio page)
- Languages spoken
- **Credential note:** Florida dental license status is publicly verifiable at floridadentistry.com. Flag any credential that cannot be independently confirmed as "Unverified — confirm before using in content."

**Practice differentiators:**
- What makes this practice different from competitors (from their own language)
- Technology or equipment highlights
- Community involvement
- Review themes (from Step 3)

### Step 5 — Build service and offer map
Create a structured list of:
- All services offered, organized by category (reference `_context/global/dental-service-taxonomy.md`)
- Priority services: highest revenue, most commonly booked, most competitive — ask the client or infer from website emphasis
- Current offers and promotions: only include if verified on the website or confirmed by the client; mark as "Unverified — needs dentist sign-off" if sourced from website alone

### Step 6 — First-pass brand voice analysis
From sample social posts, website copy, and any brand guide, produce a preliminary voice profile:
- Tone: formal / conversational / warm / clinical / playful
- Recurring vocabulary and phrases the practice actually uses
- Preferred CTA style (e.g., "Call us," "Book online," "Schedule a free consult")
- Anything that sounds off-brand in the samples

**Note:** This is a first-pass profile. Skill #2 (`brand-voice-normalizer`) deepens and formalizes it. Mark the `brand-voice-guide.md` header with `Draft — pending brand-voice-normalizer review` until that skill runs.

### Step 7 — Competitor stub setup
For each competitor named by the client or found during research:
- Record practice name, city, website, social handles
- Note primary services and apparent positioning (from homepage only at this stage)
- Add a row to `_templates/competitor-matrix.csv`

Full competitive intelligence is handled separately by the `competitive-intel-scraper` skill.

### Step 8 — PHI safety check (required before writing output)
Before writing any output file, verify:
- No patient names, initials, or identifiers appear in any draft content
- No case descriptions, diagnoses, treatment details, or clinical outcomes are included
- No before/after patient references — review themes are generalized, not case-specific
- No review text copied verbatim that could identify a reviewer

If any PHI is detected, remove it before proceeding. Do not write output files until this check passes.

### Step 9 — Write output files
Create the following files under `_context/clients/{client-slug}/`:

| File | Contents |
|---|---|
| `brand-context.md` | Practice identity, locations, contact info, differentiators, review themes, bilingual flag |
| `doctor-bios.md` | Full doctor profiles with credentials, training, philosophy, languages |
| `brand-style-guide.md` | Colors, fonts, logo usage, visual direction, image style |
| `brand-voice-guide.md` | First-pass tone, vocabulary, approved phrases, CTA style (Draft — pending brand-voice-normalizer) |
| `service-offers.md` | Full service list by taxonomy category + current verified offers |
| `approved-claims.md` | Verified claims with sources + unverified claims flagged for dentist sign-off |
| `banned-claims.md` | Restricted topics, phrases, competitor names, legal restrictions |
| `competitors.md` | Competitor list with handles, cities, and basic homepage positioning |
| `asset-inventory.md` | Available photos, videos, logos, doctor headshots, and other creative assets |

## Output Files
```
data/raw/firecrawl/{client-slug}/
  website-crawl-{YYYY-MM-DD}.md
  gbp-extract-{YYYY-MM-DD}.md

_context/clients/{client-slug}/
  brand-context.md          ← bilingual flag lives here
  doctor-bios.md            ← separate from brand context for independent access
  brand-style-guide.md
  brand-voice-guide.md      ← Draft until brand-voice-normalizer runs
  service-offers.md
  approved-claims.md
  banned-claims.md
  competitors.md
  asset-inventory.md
```

## Updating Existing Client Context
When updating (not onboarding):
- Do not overwrite — append new information with a dated update block at the top of each file
- Format: `## Update — {YYYY-MM-DD}: {reason for update}`
- Mark outdated information as `~~struck through~~` rather than deleting it
- Update `approved-claims.md` and `banned-claims.md` immediately if offers or restrictions changed
- Re-run `brand-voice-normalizer` if the practice has rebranded or changed tone

## Quality Checklist
- [ ] Website crawled with Firecrawl; raw output saved to `data/raw/firecrawl/{client-slug}/`
- [ ] GBP data captured or flagged as missing
- [ ] Review themes extracted in general language — no patient names or identifying details
- [ ] All doctor credentials verified or flagged as Unverified
- [ ] Service list organized by taxonomy category
- [ ] Priority services identified
- [ ] Bilingual flag set in `brand-context.md` if Spanish confirmed
- [ ] Insurance acceptance documented
- [ ] `approved-claims.md` contains only verified or explicitly flagged claims — no invented facts
- [ ] `banned-claims.md` populated even if minimal
- [ ] Competitor list has at least 3 entries with handles
- [ ] PHI safety check passed — no patient identifiers in any file
- [ ] `doctor-bios.md` created as a separate file
- [ ] `brand-voice-guide.md` marked as Draft
- [ ] `asset-inventory.md` lists all available creative materials
- [ ] All files written to `_context/clients/{client-slug}/`

## Failure Modes
- **Website blocks Firecrawl** — use manual notes from intake form; flag website issue in `brand-context.md`
- **No GBP URL provided** — note in `brand-context.md`; research analyst locates it during market research phase
- **No brand guide provided** — analyze visual style from existing social posts and website; mark as "inferred" in `brand-style-guide.md`
- **No sample posts available** — set voice as "to be established" in `brand-voice-guide.md`; flag for first-content review
- **Unverifiable claims on website** — list in `approved-claims.md` as "Unverified — needs dentist confirmation"
- **Multi-location practice** — one shared `brand-context.md`; note location-specific differences in a dedicated section
- **PHI detected in provided materials** — remove immediately; do not include in any output file; note that materials contained PHI and were sanitized

## Dependencies
- Firecrawl (primary — for website and GBP crawl) or Crawl4AI (fallback: `crwl <url> -o markdown`)
- `_context/global/dental-service-taxonomy.md` (for service categorization)
- `_templates/competitor-matrix.csv` (for competitor stub entries)

**Feeds into:** `brand-voice-normalizer`, `market-research`, `compliance-review`, `social-copy`, and all downstream agents
