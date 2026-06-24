# Skill: social-creative-designer

## Description
Translates approved campaign briefs and social copy into visual creative briefs and AI image generation prompts for Gemini / Nano Banana. Produces one completed `_templates/creative-brief-template.md` per visual asset needed, with a structured Gemini prompt, negative prompt, QA criteria, and asset workflow instructions. Manages the creative pipeline: `creative/generated/` → QA review → `creative/approved/` or `creative/rejected/`. Never generates realistic patient before/after imagery. Never uses real patient likeness.

**Only runs after the campaign brief AND the corresponding social copy or script have been approved.** Visual assets are never produced before the copy strategy is locked.

## When to Use
- After `campaign-brief` is approved AND `social-copy` or `short-form-video-script` for the specific piece is reviewed
- When building a carousel cover, educational graphic, quote card, GBP post image, or Story graphic
- When the campaign plan calls for AI-generated imagery (Gemini/Nano Banana)
- When directing a practice photography session (produces a shot brief, not AI images)

Do NOT run before the creative brief has been approved by the account lead. Do NOT use `social-creative-designer` for before/after patient content without documented HIPAA authorization confirmed by the practice.

## Required Inputs

| Input | Priority | Source |
|---|---|---|
| Approved campaign brief | Required | `social/calendars/{client-slug}/` |
| Approved social copy or script for this piece | Required | `social/captions/` or `social/scripts/` |
| `_context/clients/{client-slug}/brand-style-guide.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/brand-context.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/asset-inventory.md` | Required | client-context-ingestion |
| `_context/global/compliance-rules.md` | Required | Global context |
| `_context/global/platform-rules.md` | Required | Global context |
| `_templates/creative-brief-template.md` | Required | Template |

## Visual Asset Types
Select the type that matches the content piece being supported:

| Type | Description | AI-generated? | Platform fit |
|---|---|---|---|
| **Service concept graphic** | Abstract, illustration-style visual representing a dental service | Yes | Instagram, Facebook, GBP |
| **Educational infographic** | Clean diagram or data visualization of a dental concept | Yes | Instagram carousel, Facebook |
| **Quote / text card** | Brand-colored background with a quote, stat, or tip as the hero element | Yes | Instagram, Facebook, GBP, Story |
| **Carousel cover** | Eye-catching cover slide for a swipeable carousel | Yes | Instagram |
| **Story graphic** | Vertical format branded graphic for 24-hour content | Yes | Instagram Story, Facebook Story |
| **Lifestyle image** | SE Florida lifestyle imagery bridged to oral health | Yes | Instagram, Facebook |
| **Practice/team concept** | Warm office or team-like imagery (when real photos aren't available) | Yes (with caveats) | Instagram, Facebook |
| **GBP post image** | Professional, locally-relevant image for Google Business Profile | Yes | GBP |
| **Video thumbnail** | Static frame optimized to drive Reel/Short click-through | Yes | Instagram, YouTube |
| **Photography shot brief** | Direction for a real practice photo session — not AI-generated | No (human) | All platforms |

## AI Generation Constraints (Non-Negotiable)
These rules apply to every prompt produced by this skill:

**Never generate:**
- Realistic before/after imagery implying real patient results
- Identifiable human faces in clinical or procedural contexts
- Images that could be interpreted as depicting a specific patient
- Dental procedures being performed on a person (hands in mouth, etc.)
- X-rays or clinical records (real or realistic-looking)
- Anything that could shame, alarm, or stigmatize dental conditions

**Always include in negative prompts:**
- `realistic dental procedures`, `before and after patient photos`, `identifiable patients`
- `clinical close-ups of mouths`, `graphic medical imagery`, `scary dental tools`
- `stock photo feel`, `generic corporate imagery`

**Caveats for team/practice imagery:**
AI-generated "people" faces should look diverse and realistic in style but should NOT closely resemble real, identifiable individuals. Note in creative brief: "AI-generated team imagery is for concept ideation only — replace with real practice photos when available."

## Platform Visual Specifications
Reference these when setting brief dimensions:

| Platform | Format | Dimensions | Notes |
|---|---|---|---|
| Instagram | Square feed | 1080 × 1080 px | Safe for all feed contexts |
| Instagram | Portrait feed | 1080 × 1350 px | More screen real estate |
| Instagram | Reel / Story | 1080 × 1920 px (9:16) | Vertical only |
| Instagram | Carousel slide | 1080 × 1080 px | Each slide same dimensions |
| Facebook | Feed post | 1200 × 630 px | Landscape dominant |
| Facebook | Story | 1080 × 1920 px | Vertical |
| TikTok | Video frame | 1080 × 1920 px | Vertical only |
| GBP | Post image | 1200 × 900 px | Landscape recommended |
| YouTube Shorts | Thumbnail | 1280 × 720 px | 16:9 |

## Process

### Step 1 — Load context and identify visual requirements
Read all required inputs. From the approved campaign brief, identify every content piece that requires a visual asset:
- Which ideas have `Format: Carousel`, `Format: Reel thumbnail`, `Format: Story`, etc.
- Which GBP posts need a supporting image
- Which YouTube Shorts need a thumbnail

From `asset-inventory.md`, note what real photos and videos already exist:
- If real photos are available and appropriate, note "use existing asset" in the brief rather than generating
- Only generate AI imagery when real assets are absent or insufficient

From `brand-style-guide.md`, extract before writing any prompt:
- Brand colors (hex or descriptive — note both)
- Typography style (font family/weight — described for prompt use, e.g., "clean modern sans-serif")
- Visual tone (bright/clinical vs. warm/approachable vs. luxury/polished)
- Any explicit visual restrictions (e.g., "no blue-and-white — competitor uses that palette")

### Step 2 — Determine asset approach for each piece
For each visual requirement, decide the approach before writing a brief:

```
Content piece: [campaign brief idea #N]
Visual type: [from type table above]
Asset approach: □ Use existing asset (from asset-inventory.md)
               □ AI generation (Gemini/Nano Banana)
               □ Photography shot brief (for upcoming shoot)
               □ Graphic design directive (text + brand elements, no imagery)
```

Only proceed to Steps 3+ for AI generation or photography briefs. For existing assets, note the file path from `asset-inventory.md` and skip to the compliance check.

### Step 3 — Write the visual concept
For each AI generation brief, define the visual concept before writing the prompt:

**Big idea (one sentence):** What should a viewer feel in the first half-second of seeing this image?

**Mood/aesthetic (select one):**
- Bright and welcoming — natural light, white spaces, warm tones
- Bold and confident — strong color blocks, clean typography, modern
- Soft and aspirational — blurred backgrounds, light, dreamy quality
- Luxury and polished — dark tones, gold accents, pristine detail
- Community and warmth — diverse faces, candid moments, SE Florida lifestyle

**Content pillar visual approach:**

| Pillar | Visual direction |
|---|---|
| Education | Clean illustration, diagram, or infographic — information-forward, minimal clutter |
| Trust & Social Proof | Warm, human, real-feeling — team moments, office warmth, welcoming spaces |
| Service Demand | Aspirational outcome — confident smile (stock), clean result concept (illustration) |
| Local Community | SE Florida lifestyle — palm trees, diverse community, local landmarks (illustrated) |
| Team Personality | Human warmth — team-like AI imagery OR direction for real team photo session |
| Promotional | High-contrast, bold — offer text is the hero, image is a supporting backdrop |

### Step 4 — Write the Gemini / Nano Banana image prompt
A well-structured dental marketing prompt has six components. Write each component explicitly:

**Prompt anatomy:**
```
[Style descriptor] — [Subject] — [Composition] — [Lighting and color] — [Mood] — [Technical spec]
```

**Style descriptors that work for dental marketing:**
- "Professional lifestyle photography style" — avoids clinical feel
- "Clean modern illustration, flat design" — for educational graphics
- "Warm editorial photography" — for practice/team concepts
- "Bold graphic design, text-forward" — for quote cards
- "Aerial view, abstract" — for interesting non-literal compositions
- "Soft-focus bokeh background" — for aspirational imagery

**Subject guidance by visual type:**

| Visual type | Subject approach |
|---|---|
| Service concept | Abstract representation — illustrated tooth, illustrated smile graphic, concept art. Never a procedure on a person. |
| Educational infographic | Clean diagram or icon set — tooth anatomy illustration, brushing technique arrows, whiteness scale |
| Quote / text card | Minimal abstract background — soft color wash, subtle dental tool silhouette, brand pattern |
| Carousel cover | Bold hook text on brand-colored background — image is supporting, text is hero |
| Lifestyle image | Diverse person smiling in an SE Florida setting — beach, urban Miami, palm trees. Smiling person (stock-style, not clinical). |
| Practice/team concept | Warm modern dental office interior, diverse healthcare team in professional attire, bright natural lighting |
| GBP post image | Professional dental office exterior or bright reception area — local, welcoming |
| Video thumbnail | Bold text overlay on practice-branded background — high contrast, legible at small size |

**Prompt template:**
```
[Style]: [photography/illustration/graphic design style]
[Setting]: [location or background]
[Subject]: [what is in the image — describe without clinical specifics]
[Demographic]: [if people appear: diverse, professional, SE Florida demographic]
[Lighting]: [bright natural / warm studio / soft bokeh]
[Color]: [brand primary color name] and [brand accent color name] tones
[Mood]: [welcoming / confident / aspirational / warm]
[Format]: [aspect ratio or crop]
```

**Example prompt (Service Demand — Implants):**
```
Professional lifestyle photography style. A bright modern dental office 
reception area with clean white walls and [brand color] accents. A confident, 
diverse adult in their 40s smiling naturally in a welcoming waiting room. 
Soft natural lighting through floor-to-ceiling windows. South Florida 
atmosphere — subtle palm fronds visible outside. Warm, aspirational mood. 
Square format 1:1.
```

**Example prompt (Education — Infographic):**
```
Clean flat design illustration. A cross-section diagram of a dental implant 
showing the three components with simple labels: implant post, abutment, crown. 
Medical illustration style but friendly and approachable — not clinical. 
[Brand primary color] as the dominant color with white and light grey accents. 
Minimal background, white or near-white. Square format 1:1.
```

**Negative prompt (always include):**
```
No realistic dental procedures being performed, no identifiable patients, 
no clinical close-ups of mouths or teeth in a medical context, 
no before/after patient imagery, no graphic medical tools, 
no generic stock photo feel, no low-quality clipart, 
no competitor brand colors, no scary or alarming dental imagery.
```

### Step 5 — Define QA criteria
Before running generation, specify what "approved" looks like for this specific image. QA criteria prevent subjective back-and-forth after generation.

**Standard QA checklist (expand per brief):**
- [ ] Brand colors are present and accurate (not shifted or off-palette)
- [ ] No realistic patient faces or patient-implied imagery
- [ ] No clinical procedures visible on a person
- [ ] Demographic representation feels appropriate for SE Florida
- [ ] Mood and lighting match the brief's aesthetic direction
- [ ] Composition works at the intended dimensions (no important elements cropped)
- [ ] Text elements (if any) are legible at thumbnail size
- [ ] Image does not trigger concern under Meta health/wellness policy (if content will be boosted)
- [ ] No watermarks, artifacts, or obvious AI tells (blurred text, extra fingers, etc.)

**Visual-verbal check:** Does this image pair correctly with the approved copy? An education-pillar image must not look promotional. A trust-pillar image must not look clinical.

### Step 6 — Fill the creative brief template
Fill `_templates/creative-brief-template.md` for each asset. Save to:
```
creative/briefs/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_{format}_brief_v1.md
```

Include:
- Context section: goal, target viewer, platform, CTA action
- Visual concept section: big idea, mood, imagery direction, colors, typography, text on image
- Technical specs: dimensions from platform table, file format (PNG for static, JPG for photos), safe zones
- Brand constraints: must include / must not include / brand voice in visual
- Image Generation Instructions: filled prompt, negative prompt, style reference, output path
- Compliance review checklist
- QA criteria (from Step 5)

### Step 7 — Generate with Gemini / Nano Banana
**Run only after the creative brief is approved by the account lead.**

**Using Gemini image generation:**
```bash
# Gemini image generation via API
# Credentials: GEMINI_API_KEY env var
curl -s -X POST "https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash-exp-image-generation:generateContent" \
  -H "x-goog-api-key: $GEMINI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "contents": [{
      "parts": [
        {"text": "[PASTE FULL PROMPT FROM BRIEF]"},
        {"text": "Negative prompt: [PASTE NEGATIVE PROMPT]"}
      ]
    }],
    "generationConfig": {"responseModalities": ["IMAGE", "TEXT"]}
  }'
```

**Using Nano Banana (if configured in `.mcp.json`):**
Follow the Nano Banana MCP tool instructions in the project `.mcp.json` configuration. Pass the prompt and negative prompt as specified in the brief.

**Generate 2–3 variants per brief.** Never present only one option — variation allows the account lead to choose the best fit without a re-generation cycle.

Save all generated variants to:
```
creative/generated/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_{format}_gen_v1_[a/b/c].png
```

Do not move to `creative/approved/` until QA passes.

### Step 8 — QA review of generated assets
Apply the QA checklist from Step 5 to each generated variant.

**For each generated image, record:**
```
Variant: [a / b / c]
QA result: Pass / Fail
Failures: [list any QA check failures]
Recommended action: Approve / Regenerate with modified prompt / Reject
```

**If QA fails:**
- Modify the prompt to address the failure (more specific subject, adjusted negative prompt, different style descriptor)
- Regenerate up to 2 more rounds
- If 3 rounds fail to produce a passing image, flag for escalation: "AI generation not producing compliant output — recommend practice photo or licensed stock image instead"

**If all variants pass QA:**
Select the best variant. Note the reason for selection in the brief.

### Step 9 — Asset workflow management
Move approved assets through the pipeline:

```
creative/generated/{client-slug}/   → QA pass → creative/approved/{client-slug}/
                                    → QA fail → creative/rejected/{client-slug}/ + reason logged
```

For approved assets, rename to final convention:
```
creative/approved/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_{format}_final.png
```

Update the creative brief status: `Status: Approved`

For rejected variants, save with reason:
```
creative/rejected/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_{format}_rejected_v1_[reason].png
```

Note: Never delete rejected variants — they are logged for reference and skill improvement.

### Step 10 — Photography shot brief (if applicable)
When the visual requirement calls for real practice photos rather than AI generation, produce a photography shot brief instead of an image prompt.

Save to: `creative/briefs/{client-slug}/{YYYY-MM-DD}_{client-slug}_photo-session_brief_v1.md`

**Photography shot brief structure:**
```markdown
# Photography Session Brief — {Practice Name}
**Date:** {session date or TBD}
**Location:** {practice address}
**Estimated duration:** {N hours}

## Shot List

### Priority shots (must-have)
1. [Description]: [subject, setting, composition, lighting]
   PHI note: [Any PHI risk — e.g., "ensure no patient charts visible"]

### Secondary shots (nice-to-have)
...

## Talent Required
- [ ] Doctor: [name] — consent confirmed
- [ ] Staff: [names/roles] — consent confirmed
- [ ] No patients — do not film patients without documented written consent + HIPAA authorization

## Equipment Recommendations
- Smartphone (high-quality camera mode) or DSLR
- Ring light or portable LED panel
- Clean background or office set-up

## Brand Direction
- Colors visible in frame: [brand colors]
- Wardrobe: [professional attire, brand-colored scrubs if applicable]
- Avoid: [anything inconsistent with brand-style-guide.md]
```

## Output Files
```
creative/briefs/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_{format}_brief_v1.md    ← one per asset
  {YYYY-MM-DD}_{client-slug}_photo-session_brief_v1.md        ← if photo session needed

creative/generated/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_{format}_gen_v1_a.png
  {YYYY-MM-DD}_{client-slug}_{topic}_{format}_gen_v1_b.png
  {YYYY-MM-DD}_{client-slug}_{topic}_{format}_gen_v1_c.png

creative/approved/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_{format}_final.png       ← after QA pass

creative/rejected/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_{format}_rejected_v1_[reason].png
```

## Quality Checklist
- [ ] Campaign brief and approved copy/script loaded before starting
- [ ] `brand-style-guide.md` loaded — brand colors, typography, visual tone extracted
- [ ] `asset-inventory.md` checked — existing assets used where available
- [ ] Visual type selected and asset approach determined (AI / existing / photo session)
- [ ] Visual concept written: big idea + mood + pillar-aligned direction
- [ ] Gemini prompt written using six-component anatomy: style + subject + composition + lighting/color + mood + format
- [ ] Negative prompt includes all mandatory exclusions
- [ ] QA criteria defined before generation
- [ ] Creative brief template filled completely and saved to `creative/briefs/{client-slug}/`
- [ ] Brief approved by account lead before generation runs
- [ ] 2–3 variants generated per brief
- [ ] All generated assets saved to `creative/generated/{client-slug}/` (not moved until QA passes)
- [ ] QA checklist applied to each variant
- [ ] Visual-verbal check: image pairs correctly with approved copy
- [ ] Approved variant moved to `creative/approved/{client-slug}/`
- [ ] Rejected variants logged with reason in `creative/rejected/{client-slug}/`
- [ ] Creative brief status updated to Approved
- [ ] No patient imagery, no realistic before/after, no clinical procedures on people in any generated output

## Failure Modes
- **No `brand-style-guide.md` exists** — run `client-context-ingestion` first; do not invent brand colors or typography
- **AI generation triggers Gemini safety filter** — the prompt may contain medical terminology that triggers the filter; rewrite using lifestyle/marketing language rather than clinical terms; avoid "dental procedure," "tooth extraction," "surgery" — use "dental visit," "smile care," "dental health"
- **3 generation rounds fail QA** — flag for escalation; recommend practice photography or licensed stock imagery as alternative; note in brief: "AI generation insufficient — manual asset required"
- **Before/after content requested** — halt; do not generate; require documented HIPAA marketing authorization confirmed by the practice before proceeding
- **Gemini/Nano Banana not configured** — produce the creative brief and prompts as deliverables; flag that image generation credentials need to be set up; note `GEMINI_API_KEY` env var is required
- **Practice wants AI imagery to look like their actual team** — do not generate recognizable likenesses; produce a photography shot brief instead; AI-generated "people" imagery should be used for concept ideation only
- **Generated image has AI artifacts** — extra fingers, blurred text, distorted faces — note in QA as Fail; modify prompt to specify "no artifacts, photorealistic quality" or switch to illustration style

## Dependencies
- Gemini image generation API (`GEMINI_API_KEY` env var) or Nano Banana (`.mcp.json` configured)
- `campaign-brief` (must be approved first)
- `social-copy` or `short-form-video-script` (must be reviewed — copy strategy locked before visuals)
- `client-context-ingestion` (provides brand-style-guide, brand-context, asset-inventory)
- `compliance-review` (visual assets should pass through compliance-qa-reviewer before client delivery)
- `_context/global/compliance-rules.md`
- `_context/global/platform-rules.md`
- `_templates/creative-brief-template.md`

**Feeds into:** `compliance-qa-reviewer` agent (visual compliance check), `creative/approved/` (client-ready assets), `content-creator` agent (final package assembly)
