# Skill: compliance-review

## Description
Reviews dental marketing content before it leaves the project. Checks for PHI exposure, testimonial risk, clinical claims, misleading offers, before/after imagery issues, and platform policy violations. Produces a risk score, required edits list, and an approve / redline / reject decision.

## When to Use
- Before any caption, script, creative brief, or campaign brief is delivered to a client
- Before any content is scheduled or published
- When a before/after concept, patient story, or review response is created
- When a new offer, price, or credential claim appears in copy
- When any content references a named or identifiable patient

This skill is mandatory. No content leaves the project without a compliance review pass.

## Required Inputs
- Draft content to review (caption, script, creative brief, offer, or review response)
- `_context/global/compliance-rules.md` — the non-negotiable rules
- `_context/global/platform-rules.md` — per-platform policy constraints
- `_context/clients/{client}/approved-claims.md` — verified claims for this practice
- `_context/clients/{client}/brand-context.md` — practice context

## Process

### Step 1 — Load context
Read `_context/global/compliance-rules.md` and `_context/clients/{client}/approved-claims.md` before reviewing any content. Do not proceed without both files.

### Step 2 — PHI scan
Check every word and image description for:
- Patient names, initials, locations, or identifying details
- Appointment dates, treatment history, or clinical outcomes tied to a specific person
- Before/after content — flag immediately; require HIPAA authorization documentation
- Review responses — check for any confirmation of clinical details

If PHI risk is detected: **stop, flag as High risk, reject until resolved.**

### Step 3 — Claims verification
For every factual claim in the content:
- Check against `approved-claims.md`
- If the claim is not listed: flag it for dentist review — do not approve
- Check for guaranteed outcome language: "will," "guaranteed," "always," "proven to"
- Check for invented statistics, awards, rankings, or credentials

### Step 4 — Clinical content check
- Does the content tell a patient what treatment they need? → Flag
- Does the content imply a diagnosis? → Flag
- Does the content describe a specific clinical result for a named or implied patient? → Flag
- Is there any DIY dental advice? → Reject immediately

### Step 5 — Tone and language check
- Shame-based language ("your teeth are rotting," "you've been neglecting your smile") → Flag
- Fear-based urgency designed to manipulate rather than inform → Flag
- Negative self-perception language that violates Meta health/wellness policy → Flag

### Step 6 — Platform policy check
Reference `_context/global/platform-rules.md` for the target platform:
- Before/after content in a paid Meta context → Flag; advise organic-only or legal review
- Health condition targeting language → Flag
- Graphic procedure imagery → Flag for platform-specific review

### Step 7 — Score and decide
Assign a risk score and decision:

| Risk score | Criteria | Decision |
|---|---|---|
| **Low** | No flags; all claims verified; no PHI risk; platform-compliant | **Approved** |
| **Medium** | 1–2 minor flags; claims need dentist sign-off; tone adjustment needed | **Redline** — return with specific edits |
| **High** | PHI risk; unverified clinical claim; guaranteed result; before/after without auth; shame/fear language | **Reject** — full rewrite required |

### Step 8 — Produce output
Write a compliance review note to `social/approvals/` using this format:

```
Content: [file name or description]
Date: YYYY-MM-DD
Risk score: Low / Medium / High
Decision: Approved / Redline / Reject

Flags:
- [flag 1 — specific text or element + rule violated]
- [flag 2]

Required edits (if Redline):
- [specific edit 1]
- [specific edit 2]

Reason for rejection (if Reject):
- [specific reason]

Reviewer: Compliance / QA Reviewer
```

## Output Files
- `social/approvals/YYYY-MM-DD_{client}_{content-slug}_compliance.md`

## Quality Checklist
- [ ] `compliance-rules.md` was loaded before review began
- [ ] `approved-claims.md` was checked for every factual claim
- [ ] PHI scan completed — no patient identity revealed or implied
- [ ] No patient content without documented HIPAA authorization
- [ ] No diagnosis or treatment-specific advice
- [ ] No guaranteed results
- [ ] No shame-based or fear-based language
- [ ] No invented credentials, statistics, prices, or outcomes
- [ ] Platform policy checked for target platform
- [ ] Risk score assigned: Low / Medium / High
- [ ] Decision stated: Approved / Redline / Reject
- [ ] Output written to `social/approvals/`

## Failure Modes
- **Missing approved-claims.md** — do not proceed; ask the client intake agent to create it first
- **PHI detected** — do not attempt to sanitize and approve; reject and escalate to client
- **Before/after content without authorization** — reject; do not approve as "illustrative" without explicit documentation
- **Claim not in approved-claims.md** — do not approve; flag for dentist review every time, even if the claim seems obviously true
- **Ambiguous patient reference** — treat as PHI risk; escalate rather than guess

## Dependencies
- `_context/global/compliance-rules.md`
- `_context/global/platform-rules.md`
- `_context/clients/{client}/approved-claims.md`
- `_context/clients/{client}/brand-context.md`
