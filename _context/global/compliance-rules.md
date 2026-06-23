# Compliance Rules — Dental Marketing

This file is read before any content is produced. Every agent, skill, and output is bound by these rules.

---

## Non-Negotiable Rules

### 1. Never reveal or imply patient identity

Patient names, faces, initials, locations, appointment dates, treatment details, or any combination of details that could identify a patient are protected health information (PHI) under HIPAA. Do not include in captions, scripts, testimonials, case studies, before/after posts, or review responses.

**Edge cases:**
- A before/after photo with a distinctive tattoo or recognizable background = PHI risk. Reject.
- "Our patient from Coral Gables loved her veneers!" = may identify the patient. Reject unless the patient has signed a HIPAA-compliant marketing authorization.
- Stock photos of teeth or smiling models are acceptable if clearly not a real patient result.

### 2. Never use patient photos, testimonials, cases, reviews, or before-and-after content without explicit written authorization

HIPAA's marketing rule requires a valid authorization for using or disclosing PHI for marketing purposes. A general consent form does not cover marketing use.

**Required documentation:**
- Signed HIPAA marketing authorization form
- Authorization must specify what content is approved (photo, video, quote, before/after)
- Authorization must be on file and retrievable
- Flag any content involving a real patient for elevated review — do not approve without documentation confirmation

### 3. Never provide diagnosis or treatment-specific advice

Content must not tell patients what treatment they need, imply a diagnosis, or suggest a specific clinical outcome for their situation.

**Acceptable:** "Dental implants can be a long-term solution for missing teeth. Talk to your dentist to find out if they're right for you."
**Not acceptable:** "If you have a missing tooth, you need an implant — book now."

### 4. Never claim guaranteed results

Avoid words like "guaranteed," "always," "will," "proven to," or "you'll get [specific outcome]."

**Acceptable:** "Many patients see significant improvement in their smile after veneers."
**Not acceptable:** "Veneers will give you the perfect smile you've always wanted — guaranteed."

### 5. Never shame patients or use fear-based medical language

Content must not reference embarrassing symptoms, describe dental neglect in graphic terms, or use language designed to trigger anxiety or shame.

**Not acceptable:** "Rotting teeth," "disgusting plaque," "you've been ignoring your oral health too long," images of severe decay as shock content.
**Acceptable:** "We're here for patients at every stage of their dental journey — no judgment."

### 6. Never invent credentials, awards, reviews, prices, or treatment outcomes

All factual claims must be verifiable from the client's `approved-claims.md` file. If a claim is not in that file, flag it for dentist review before approving.

**Examples of invented claims:** "Voted #1 dentist in Miami," "5,000 successful implants," "$99 whitening special" (without verification).

### 7. Never autonomously publish content without human approval

The system produces content for human review. No content goes to a client or public channel without passing all review gates and receiving explicit human sign-off.

### 8. Never scrape private, logged-in, paywalled, or restricted content

Only public data is acceptable: public social profiles, public websites, public reviews, public YouTube content.

---

## HIPAA Marketing Rule Summary

Under the HIPAA Privacy Rule, a covered entity (dental practice) may not use or disclose PHI for marketing purposes without a valid written authorization from the patient, with limited exceptions.

**What this means for dental social media marketing:**
- Patient testimonials require signed authorization
- Before/after photos require signed authorization
- Case studies require signed authorization
- Review responses must not confirm or add clinical details (e.g., do not respond to a review by saying "We're glad your implant procedure went well")
- Generic educational content (e.g., "What are dental implants?") does not require authorization

**Escalation:** Any content involving a named or identifiable patient must be escalated to the Compliance / QA Reviewer and the client for authorization confirmation before approval.

---

## ADA Advertising Guidelines

The American Dental Association's Principles of Ethics and Code of Professional Conduct state that dental advertising must be:
- Truthful
- Not misleading or deceptive
- Not likely to mislead the public

**Specific guidance:**
- Price advertising is permitted but must not be misleading (e.g., "starting at" prices that most patients cannot qualify for)
- Before/after photos must represent actual patient results (with authorization) or be clearly labeled as illustrative
- Credentials must be accurate and verifiable
- Specialty claims must reflect actual board-recognized specialties

---

## Meta Health and Wellness Advertising Policy

Meta's advertising policies restrict ads that imply negative self-perception based on body image, health status, or personal attributes.

**For dental marketing on Meta:**
- Do not use before/after creative that could be interpreted as shaming the "before" state
- Do not use language that implies the viewer has a problem ("Do you have stained teeth?")
- Do not target based on health conditions or imply knowledge of a user's health
- Cosmetic procedure advertising may require additional review; plan for potential delays

**Safe framing:** Focus on aspiration and positivity ("Love your smile"), not problem-shame ("Fix your ugly teeth").

---

## Compliance Review Checklist

Before any content receives final approval, the Compliance / QA Reviewer verifies:

- [ ] No patient identity revealed or implied
- [ ] No patient content used without documented authorization
- [ ] No diagnosis or treatment-specific advice
- [ ] No guaranteed results or misleading outcome claims
- [ ] No shame-based or fear-based language
- [ ] No invented credentials, awards, reviews, prices, or statistics
- [ ] No content referencing a specific patient in a review response
- [ ] All factual claims verified against `approved-claims.md`
- [ ] Creative does not violate Meta health/wellness policy
- [ ] Risk score assigned: Low / Medium / High
- [ ] Status: Approved / Needs edits / Rejected

---

## Escalation Rules

| Situation | Action |
|---|---|
| Content references an identifiable patient | Escalate to client; require signed HIPAA marketing authorization before proceeding |
| Clinical claim not in approved-claims.md | Flag for dentist review; do not approve |
| Before/after content | Require authorization documentation; escalate if missing |
| Review response that risks confirming PHI | Rewrite to remove clinical detail; escalate if unclear |
| Compliance risk score is High | Reject and return to Content Creator for full rewrite |
| Compliance risk score is Medium | Return for edits; re-review before approval |
