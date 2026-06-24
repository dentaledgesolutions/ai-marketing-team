# Skill: client-context-ingestion

## Description
Converts raw client materials — website, intake form, service list, doctor bios, brand guide, sample posts, and competitor names — into structured context files under `_context/clients/{client-name}/`. These files are the foundation every other skill and agent reads when working on client tasks.

## When to Use
- When onboarding a new dental practice client
- Before any research, strategy, or content work begins for a practice
- When client materials are updated (new services, new doctor, new offer, rebrand)
- When the existing client context files are missing or incomplete

Run this skill before any other skill in the client workflow.

## Required Inputs
Gather as many of the following as available. Flag missing items for follow-up.

| Input | Required | Notes |
|---|---|---|
| Practice website URL | Required | Crawl with Firecrawl |
| Services list | Required | May come from website or intake form |
| Practice name and locations | Required | Include all locations if multi-site |
| Doctor name(s) and bios | Required | Full name, credentials, specialty, years in practice |
| Target patient types | Required | Demographics, services they seek |
| Competitors (3–10 names) | Required | For competitor matrix setup |
| Brand guide or style reference | Preferred | Colors, fonts, logo usage |
| Sample social posts (5–10) | Preferred | Recent content for tone analysis |
| Existing offers or promotions | Preferred | Verify before including in approved-claims.md |
| Banned phrases or sensitivities | Optional | Anything the practice has asked to avoid |
| Insurance accepted | Optional | For approved claims and audience targeting |
| Languages spoken | Optional | English, Spanish, other |
| Practice phone and booking URL | Required | For CTAs in all content |

## Process

### Step 1 — Crawl the practice website
Use Firecrawl to extract:
- Homepage content (practice mission, tagline, differentiators)
- Services pages (full list of services offered, descriptions)
- About/team pages (doctor names, credentials, bios, years in practice)
- Contact/locations (address, phone, hours)
- Any existing offer or special pages
- Patient resources or FAQ pages

Save raw crawl output to `research/proposals/{client-slug}/website-crawl.md`.

### Step 2 — Extract and structure brand context
From the crawl and any provided materials, extract:

**Practice identity:**
- Full practice name and any DBA names
- Locations (address, city, county, zip)
- Phone number(s) and booking URL
- Google Business Profile URL
- Social media handles (Instagram, Facebook, TikTok, YouTube)
- Years in practice / founding year
- Number of providers

**Doctor profiles (one per doctor):**
- Full name and credentials (DDS, DMD, specialty board certifications)
- Years in practice
- Dental school and graduation year
- Specialties or advanced training
- Personal note or philosophy (from bio page)
- Languages spoken

**Practice differentiators:**
- What makes this practice different from competitors?
- What do patients say in reviews (themes only — no patient identity)
- Technology or equipment highlights
- Community involvement

### Step 3 — Build service and offer map
Create a structured list of:
- All services offered, organized by category (use `_context/global/dental-service-taxonomy.md`)
- Priority services (highest revenue, most commonly booked, most competitive)
- Current offers and promotions — only include if verified; mark as "unverified" if sourced from website only

### Step 4 — Analyze brand voice
From sample social posts, website copy, and any brand guide:
- Identify tone: formal / conversational / warm / clinical / playful
- Extract recurring vocabulary and phrases the practice uses
- Note anything that sounds off-brand in the samples
- Identify preferred CTA style

### Step 5 — Identify competitors
For each competitor named by the client or found during research:
- Record practice name, city, website, social handles
- Note primary services and positioning
- Create placeholder rows in `_templates/competitor-matrix.csv`
- Full competitive intelligence is handled by the Competitive Intelligence Analyst skill

### Step 6 — Write approved-claims.md
List only verified, documentable claims:
- Claims sourced directly from the website or provided by the client
- Mark each claim as: Verified (source) / Unverified (needs dentist sign-off) / Banned
- Examples of verified claims: "Accepting new patients," "In-house financing available," "Open Saturdays"
- Examples requiring sign-off: "Over 1,000 implants placed," "Voted best dentist in [city]"

### Step 7 — Write banned-claims.md
List anything the practice has explicitly asked not to use:
- Competitor names
- Sensitive topics (pricing they don't want publicized, services they're phasing out)
- Previous brand names or outdated information
- Any legal restrictions on advertising (rare but possible)

### Step 8 — Write output files
Create the following files under `_context/clients/{client-slug}/`:

| File | Contents |
|---|---|
| `brand-context.md` | Practice identity, doctor profiles, differentiators, booking info |
| `brand-style-guide.md` | Colors, fonts, logo usage, visual direction, image style |
| `brand-voice-guide.md` | Tone, vocabulary, approved phrases, banned phrases, CTA style |
| `service-offers.md` | Full service list by category + current offers |
| `approved-claims.md` | Verified claims with sources |
| `banned-claims.md` | Restricted topics and phrases |
| `competitors.md` | Competitor list with handles and basic positioning |
| `asset-inventory.md` | Available photos, videos, logos, and other creative assets |

## Output Files
```
_context/clients/{client-slug}/
  brand-context.md
  brand-style-guide.md
  brand-voice-guide.md
  service-offers.md
  approved-claims.md
  banned-claims.md
  competitors.md
  asset-inventory.md
```

## Quality Checklist
- [ ] Website crawled with Firecrawl; raw output saved to `research/proposals/`
- [ ] All doctor names and credentials verified from official source
- [ ] Service list organized by taxonomy category
- [ ] Brand voice analyzed from actual content samples (not invented)
- [ ] `approved-claims.md` contains only verified or flagged claims — no invented facts
- [ ] `banned-claims.md` populated even if minimal
- [ ] Competitor list has at least 3 entries with handles
- [ ] All files written to `_context/clients/{client-slug}/`
- [ ] `asset-inventory.md` lists what creative materials the practice has available

## Failure Modes
- **Website is down or blocks Firecrawl** — use manual notes from intake form; flag website issue
- **No brand guide provided** — analyze visual style from existing social posts and website; note as "inferred" in brand-style-guide.md
- **No sample posts available** — note voice as "to be established" and flag for first-content review
- **Unverifiable claims on website** — list in approved-claims.md as "Unverified — needs dentist confirmation" rather than including or excluding
- **Multi-location practice** — create one shared brand context file; note location-specific differences where relevant

## Dependencies
- Firecrawl (for website crawl)
- `_context/global/dental-service-taxonomy.md` (for service categorization)
- `_templates/competitor-matrix.csv` (for competitor stub entries)
