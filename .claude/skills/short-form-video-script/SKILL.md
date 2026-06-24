# Skill: short-form-video-script

## Description
Produces complete, production-ready short-form video scripts for Instagram Reels, TikTok, and YouTube Shorts. Each script includes scene-by-scene breakdown, voiceover text, on-screen text, realistic B-roll direction for smartphone production, audio guidance, and a caption. Designed for private dental practices in SE Florida producing content with a smartphone and basic lighting — no film crew assumed. All scripts must pass compliance review before production begins.

## When to Use
- When the campaign brief includes a Reel, TikTok, or YouTube Shorts content idea
- When a trend opportunity requires video content (Priority trend with a short production window)
- When the practice needs a recurring video format (weekly FAQ Reel, monthly team spotlight)
- When repurposing a high-performing caption as a video

Run after `campaign-brief` is approved and after `social-media-mining` (which provides trending audio signals). Run before `creative-brief` (which handles visual direction) and before `compliance-review`.

## Required Inputs

| Input | Priority | Source |
|---|---|---|
| Approved campaign brief — specific content idea | Required | `social/calendars/{client-slug}/` |
| `_context/clients/{client-slug}/brand-voice-guide.md` | Required | brand-voice-normalizer |
| `_context/clients/{client-slug}/brand-context.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/hook-library.md` | Required | social-media-mining |
| `_context/clients/{client-slug}/approved-claims.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/banned-claims.md` | Required | client-context-ingestion |
| `_context/clients/{client-slug}/keyword-clusters.md` | Required for YouTube Shorts | keyword-research |
| `_context/clients/{client-slug}/asset-inventory.md` | Required | client-context-ingestion |
| `_context/global/compliance-rules.md` | Required | Global context |
| `_context/global/platform-rules.md` | Required | Global context |
| `_context/global/dental-service-taxonomy.md` | Required | Global context |
| `_templates/reel-script-template.md` | Required | Template |

## Video Types
Select the type that best matches the campaign brief content idea. Type determines narrative structure and production approach.

| Type | Core question it answers | Best duration | Best platform |
|---|---|---|---|
| **FAQ Answer** | "What's the #1 question we get about [treatment]?" | 30–45s | Reels, TikTok, Shorts |
| **Myth-Bust** | "Stop believing this dental myth" | 15–30s | TikTok, Reels |
| **Process Walkthrough** | "Here's exactly what happens at your first [appointment]" | 45–60s | Shorts, Reels |
| **Team Spotlight** | "Meet [person] — [why they do what they do]" | 30–45s | Reels, TikTok |
| **Service Spotlight** | "[Treatment] explained in [N] seconds" | 30–60s | Shorts, Reels |
| **Day-in-the-Life** | "A morning at [practice name]" | 30–60s | Reels, TikTok |
| **Trend Participation** | [Dental take on trending format or audio] | 15–30s | TikTok, Reels |
| **Seasonal/Promotional** | "Use your dental benefits before [date]" | 15–30s | All platforms |
| **Educational Explainer** | "Here's what [dental concept] actually means" | 45–60s | Shorts, Reels |

## Duration Guide and VO Word Counts

| Duration | Hook | Body | CTA | Max VO words (at ~2.5 wps) |
|---|---|---|---|---|
| 15 seconds | 0–3s | 3–12s | 12–15s | ~25 words |
| 30 seconds | 0–3s | 3–25s | 25–30s | ~55 words |
| 45 seconds | 0–3s | 3–40s | 40–45s | ~85 words |
| 60 seconds | 0–3s | 3–55s | 55–60s | ~110 words |

**Conversational dental content reads at approximately 2–2.5 words per second.** Count VO words before finalizing — over-long scripts get rushed or cut awkwardly. Trim body content before cutting the hook or CTA.

## On-Camera Approach
Check `brand-context.md` for the practice's camera preference before writing. Different approaches require different script structures:

| Approach | What it means | When to use |
|---|---|---|
| **Doctor direct-to-lens** | Doctor speaks directly to camera | Highest trust; use for FAQ, Team Spotlight, Service Spotlight |
| **VO over B-roll** | Doctor or staff narrates off-camera; footage runs | Flexible; great when talent is camera-shy or footage is visual |
| **OST over B-roll** | Text appears over footage; no speaking required | Easiest to produce; good for Myth-Bust, Trend Participation |
| **Hybrid** | Mix of direct-to-lens moments + B-roll cutaways | Most dynamic; recommended for Process Walkthrough |

If the practice has not stated a preference, default to **VO over B-roll** — it's the most flexible, requires the least on-camera comfort, and performs well across platforms.

## PHI Production Rules (Non-Negotiable)
These rules apply to every script produced by this skill:

- **Never direct filming of real patients** — all B-roll involving people shows staff, doctors, or empty spaces only
- **Never suggest filming patient X-rays, charts, or records** — models and props only
- **Never suggest filming a patient interaction** — even with faces blurred, the interaction itself may be identifiable
- **Smile footage** — use stock footage or photos of smiles, never an actual patient's smile unless HIPAA marketing authorization is on file and documented
- **Testimonials on camera** — if the script concept requires a patient testimonial, flag it: "Patient on camera requires HIPAA marketing authorization on file — do not film until documented"
- **Before/after concepts** — if the brief calls for before/after video content, flag it: "Before/after video requires documented HIPAA authorization — do not produce without confirmation"

## Process

### Step 1 — Load context and identify video specifications
Read all required inputs. From the campaign brief content idea, extract:

```
Content idea #: [N]
Video type: [type from table above]
Platform: [Instagram Reel / TikTok / YouTube Short / All]
Duration: [15 / 30 / 45 / 60 seconds]
Service / topic: [from dental-service-taxonomy.md]
Content pillar: [pillar name]
On-camera approach: [from brand-context.md or default to VO over B-roll]
Language: [English / Spanish / Bilingual]
Trend tie-in: [from campaign brief — if applicable]
Keyword tie-in: [from campaign brief — for YouTube Shorts]
```

From `asset-inventory.md`, note what footage and photos are already available — this informs realistic B-roll direction.

From `hook-library.md` Trending Audio section, note any audio recommendations with valid use-by dates.

### Step 2 — Select and write the hook (0–3 seconds)
The hook is the only scene that matters for retention. Platform algorithm data consistently shows 60%+ of viewers who watch past 3 seconds will watch to completion. Invest the most time here.

**Hook first: check `hook-library.md`** for a high-signal pattern matching the video type and service. Adapt it before writing from scratch.

**Hook components — write all three:**
- **On-screen text (OST):** The bold text that appears on screen in the first frame. Under 8 words. Must create immediate curiosity, recognition, or tension.
- **Voiceover / spoken (if using):** The exact words spoken. Must match or complement the OST — never repeat it word for word.
- **Visual direction:** What the viewer sees in the first 3 seconds. Be specific.

**Hook structure by video type:**

| Type | Hook pattern |
|---|---|
| FAQ Answer | "The #1 question we get about [treatment]..." + doctor looking at camera |
| Myth-Bust | "[Common misconception] — and why it's wrong" + bold text on black or close-up |
| Process Walkthrough | "Here's exactly what happens when you [action]" + montage cut to office |
| Team Spotlight | "[Name]'s answer to [relatable question]" + talking-head tight shot |
| Service Spotlight | "[Outcome] is possible in [timeframe]" + visual of result (model/illustration) |
| Day-in-the-Life | "What our morning looks like before you arrive" + time-lapse or walk-in |
| Trend Participation | [Trend-specific hook — use the trend format literally] |
| Seasonal/Promotional | "Your [insurance/benefits] expires in [N days]" + countdown urgency |
| Educational Explainer | "Nobody tells you this about [dental topic]" + direct-to-lens |

### Step 3 — Write the body beats
The body delivers the hook's promise. Every beat must justify its time — cut anything that doesn't serve what the hook promised.

**Beat count by duration:**
- 15s: 1–2 beats
- 30s: 3–4 beats
- 45s: 4–6 beats
- 60s: 5–8 beats

**For each beat, write:**
```
Beat [N] ([start time]–[end time]s)
On-screen text: [8 words max per text overlay]
VO / spoken: [exact words — count them against the word budget]
Visual: [what's on screen — specific and filmable]
```

**Body structure by video type:**

**FAQ Answer:** Problem → answer (simple) → why it matters → next step
**Myth-Bust:** State the myth → "Here's the truth" → evidence or explanation → what to do instead
**Process Walkthrough:** Step 1 → Step 2 → Step 3 (max 4 steps for 60s) → "That's it"
**Team Spotlight:** [Person]'s background → why they chose dentistry → what they love about the practice → patient-facing warmth
**Service Spotlight:** What it is (plain language) → who it's for → what to expect → the outcome
**Day-in-the-Life:** Opening prep → patient-ready setup (no patients) → team moment → CTA bridge
**Trend Participation:** Follow the trend format exactly for beats 1-N; dental angle introduced at beat 2+
**Educational Explainer:** Hook claim → explanation beat 1 → explanation beat 2 → surprising connection → takeaway

### Step 4 — Write the CTA scene (last 3–5 seconds)
One action only. Never two CTAs in the same video.

**CTA by campaign type:**
- Acquisition/Promotional: "Book your free consult at [URL] — link in bio" or "Call [phone]"
- Education/Trust: "Save this for later" or "Follow for more dental tips"
- Service spotlight: "Ask us about [service] at your next visit" or "Book at [URL]"
- Team/Brand: "We'd love to meet you — book your first visit at [URL]"

**CTA components:**
```
CTA ([start]–[end]s)
On-screen text: [exact CTA text — brand-voice-guide.md CTA style]
VO / spoken: [spoken version — can be slightly warmer than OST]
Visual: [doctor looking at camera / logo card / booking URL on screen]
```

### Step 5 — Build the B-roll shot list
Every shot must be filmable with a smartphone in the practice's actual space. Reference `asset-inventory.md` for existing footage.

**Dental practice smartphone B-roll library — use these categories:**

| Category | Examples | PHI risk |
|---|---|---|
| Office spaces | Reception desk, waiting room, treatment room (empty), signage, exterior | None |
| Equipment | Dental chair, X-ray machine (no patient), whitening light, CEREC machine, instruments laid out | None |
| Team (with consent) | Doctor washing hands, hygienist at desk, front desk smiling, team meeting | None — staff consent required |
| Props and models | Dental model (plastic teeth), mold of teeth, toothbrush and floss, before/after illustration (not photo) | None |
| Stock smiles | Licensed stock footage of smiles, teeth close-ups | None — stock only, never real patient |
| Doctor on-camera moments | Doctor speaking directly to lens, doctor reviewing a chart (blank), doctor pointing at model | None |

**Shot list format:**
```
| Shot # | Description | Duration needed | Notes |
|---|---|---|---|
| 1 | [specific visual direction] | ~[N]s | [existing footage? / needs filming] |
```

Flag any shot that requires a new filming session vs. using existing assets from `asset-inventory.md`.

### Step 6 — Write audio direction
Reference `hook-library.md` Trending Audio section for current recommendations.

**Audio approach options:**

```
Approach: [select one]
□ Original VO — doctor or staff narrates; no background music (cleanest for educational)
□ Trending audio (low) — trending sound at low volume under VO narration
□ Trending audio (primary) — video is built around the audio trend; minimal VO
□ Music bed — royalty-free background music under VO (warmer feel)
□ OST only — text on screen; no VO; use trending audio or music bed
```

**If using trending audio:**
```
Audio: [name from hook-library.md] — use by [date from hook-library.md]
If expired: Find currently trending dental audio on TikTok/Reels at time of production
Placement: [Background under VO / Primary sound / Intro only]
```

**Subtitles/captions:**
Always required. All videos must have closed captions for accessibility and silent autoplay (most social video is watched without sound on first scroll). Note caption style in Production Notes: auto-generated and reviewed, or manually added.

### Step 7 — YouTube Shorts title + keyword (if platform includes Shorts)
YouTube Shorts titles are search-indexed. Apply keyword from `keyword-clusters.md`.

```
Title: [Primary keyword from keyword-clusters.md] — [patient benefit in 4-6 words]
(Max 100 characters; keyword must appear first)

Description (for Shorts):
[Practice name] | [Service one-liner]
📞 [Phone number]
🗓️ Book: [booking URL]

[1-2 natural keyword sentences derived from the script content]
```

### Step 8 — Write the caption
For most videos, write a brief caption inline rather than routing to `social-copy`. The video carries the content — the caption teases and drives the CTA.

**Short-form video caption structure:**
```
[Hook line — first 125 characters; can reuse or echo the video's OST hook]

[1-3 lines of body — supports video but doesn't repeat it verbatim]

[CTA — match the video's CTA action]

[Hashtags from hashtag-sets.md — platform appropriate count]
```

**For carousels or long-form educational content:** flag for `social-copy` skill to produce a full caption separately.

**For YouTube Shorts:** the title is the primary copy asset; the description serves SEO — the caption field is the description.

### Step 9 — Bilingual script production (required if `bilingual: true`)
If `brand-context.md` has `bilingual: true`:

Produce a separate Spanish-language version of the full script. Do not translate the English script — adapt it:
- Rewrite VO in natural Latin American Spanish (per brand-voice-guide.md dialect notes)
- Use the usted/tú register from brand-voice-guide.md — never mix
- Rewrite OST text in Spanish (shorten if needed — Spanish text can run longer)
- Note: If the practice produces a bilingual video (both languages in one), plan the beats to include one language segment each or use bilingual captions

Mark the Spanish script: `<!-- Spanish version — needs native-speaker review before production -->`

### Step 10 — Compliance pre-check
Before finalizing the script, check:

- [ ] No real patients filmed or referenced
- [ ] No patient X-rays, records, or identifying materials in shot list
- [ ] No before/after patient footage (unless authorization flagged and confirmed)
- [ ] No guaranteed outcome claims in VO or OST
- [ ] No diagnosis or treatment-specific advice directed at the viewer
- [ ] No invented statistics, credentials, or awards
- [ ] All factual claims in VO are in `approved-claims.md`
- [ ] No banned phrases from `banned-claims.md`
- [ ] No shame-based or fear-based language
- [ ] Trending audio does not carry unsafe health messaging or controversial content
- [ ] On-camera talent noted as consented (doctor/staff)
- [ ] Spanish script flagged for native-speaker review (if applicable)

Flag every failure with `<!-- COMPLIANCE FLAG: [issue] -->` in the script. Do not self-clear flags. Escalate to `compliance-review`.

**Risk score:** Assign Low / Medium / High based on:
- Low: Education, team, FAQ — no clinical claims, no patient imagery
- Medium: Service spotlight with approved claims, process walkthrough
- High: Before/after references, testimonials, clinical statistics, promotional claims

### Step 11 — Assemble and save
Fill `_templates/reel-script-template.md` with all content from Steps 1–10.

**Save path:**
```
social/scripts/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_{platform}_script_v1.md
```

For bilingual scripts, save English and Spanish as separate files:
```
social/scripts/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_{platform}_script_v1_en.md
social/scripts/{client-slug}/{YYYY-MM-DD}_{client-slug}_{topic}_{platform}_script_v1_es.md
```

Include at the top of every file:
```
<!-- Campaign brief idea #: [N] | Video type: [type] | Duration: [N]s | Approach: [on-camera approach] -->
<!-- Compliance risk: Low / Medium / High | Flags: [None / list] -->
```

Every file ends with: `**Status:** Needs review`

Submit to `compliance-review` before production begins. Scripts are not draft suggestions — they are production documents. Do not film without a reviewed script.

## Output Files
```
social/scripts/{client-slug}/
  {YYYY-MM-DD}_{client-slug}_{topic}_{platform}_script_v1.md
  {YYYY-MM-DD}_{client-slug}_{topic}_{platform}_script_v1_en.md  (bilingual)
  {YYYY-MM-DD}_{client-slug}_{topic}_{platform}_script_v1_es.md  (bilingual)
```

## Quality Checklist
- [ ] Campaign brief content idea number identified
- [ ] Video type selected from defined types
- [ ] Platform and duration confirmed
- [ ] On-camera approach determined from brand-context.md
- [ ] `asset-inventory.md` checked for existing footage before writing shot list
- [ ] `hook-library.md` checked before writing hook — adapted where possible
- [ ] Hook components written: OST + VO/spoken + visual direction
- [ ] Hook passes cold-read test: works with no prior context
- [ ] Body beat count matches duration (1-2 beats/15s; 3-4/30s; 4-6/45s; 5-8/60s)
- [ ] VO word count within budget for selected duration
- [ ] All beats have OST + VO + visual direction written
- [ ] CTA is one action only; matches campaign brief CTA style
- [ ] B-roll shot list uses only staff, equipment, props, or stock — no real patients
- [ ] Each shot in shot list is filmable with a smartphone in the practice's actual space
- [ ] Audio direction specified; trending audio use-by date checked
- [ ] Subtitles noted as required
- [ ] YouTube Shorts title includes keyword from keyword-clusters.md (if applicable)
- [ ] Caption written using hook-library.md and hashtag-sets.md
- [ ] Spanish script produced and flagged for native-speaker review (if bilingual: true)
- [ ] Compliance pre-check complete — all failures flagged, none self-cleared
- [ ] Risk score assigned (Low / Medium / High)
- [ ] Script saved to correct path with campaign brief idea number in header
- [ ] Submitted to compliance-review before production begins

## Failure Modes
- **No campaign brief exists** — do not produce scripts; run `campaign-brief` first
- **Practice unwilling to appear on camera** — default to OST over B-roll; do not force; note in pre-production and adjust to a video type that doesn't require on-camera talent
- **VO word count exceeds duration budget** — trim body content first; never cut the hook or CTA; if content genuinely can't fit, produce at the next duration up (30s → 45s)
- **Trending audio has expired** — remove specific audio name from script; write "Find currently trending dental audio at time of production" in audio direction with a brief note on the mood/style to match
- **Content idea requires patient footage** — do not produce; flag: "This content idea requires patient authorization before a script can be written — confirm authorization status with client"; suggest an alternative video type with same educational goal
- **Script concept requires unverified clinical claims** — write around the claim; add `<!-- CLINICAL CLAIM FLAG: "[claim]" — dentist must verify accuracy before production -->` ; do not invent or assume
- **No asset-inventory.md exists** — note all shots as "needs new filming"; write production notes that estimate time needed for a smartphone filming session

## Dependencies
- `campaign-brief` (must be approved — specific content idea required)
- `social-media-mining` (provides hook-library.md and trending audio signals)
- `keyword-research` (provides keyword-clusters.md — required for YouTube Shorts)
- `brand-voice-normalizer` (provides brand-voice-guide.md and Spanish voice profile)
- `client-context-ingestion` (provides asset-inventory.md for B-roll planning)
- `compliance-review` (mandatory before production begins)
- `_context/global/compliance-rules.md`
- `_context/global/platform-rules.md`
- `_context/global/dental-service-taxonomy.md`
- `_templates/reel-script-template.md`

**Feeds into:** `compliance-review`, creative-brief (visual direction), `content-creator` agent, production scheduling
