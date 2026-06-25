---
name: notebooklm-research-os
description: |
  Use when conducting deep dental market research that requires synthesizing
  multiple sources, researching a competitor practice, building a client
  proposal research packet, exploring clinical topics for content accuracy,
  analyzing SE Florida dental trends, or converting NotebookLM exports into
  reusable project artifacts. Also use when the user says "research this in
  NotebookLM," "create a research notebook for," "analyze these sources," or
  needs research depth beyond a single Firecrawl session. Triggers for market
  research, competitor analysis, proposal research, trend deep-dives, and
  clinical topic research in the dental marketing context.
metadata:
  source_url: community download (no GitHub URL — community skill file)
  source_commit: unknown
  adapted_for: ai-marketing-team
  adapted_date: 2026-06-25
  project_context_source: evals/project-context.json
  audit_verdict: FLAG (accepted — see scratchpad/NotebookLMSkill-AUDIT.md)
  base_package: notebooklm-py v0.3.4
---

# notebooklm-research-os

Full programmatic access to Google NotebookLM via the `notebooklm-py` CLI —
adapted for SE Florida dental marketing research. Claude Code creates the
notebook, adds sources, asks structured research questions, generates a
briefing document, and converts the output into reusable project artifacts
(`research-digest.md`, `insight-log.md`, `source-map.md`).

## HIPAA Guardrails — Read Before Adding Any Source

**Never add to a NotebookLM notebook:**
- Patient names, dates of birth, contact information, or any identifiable detail
- Clinical records, treatment notes, or appointment histories
- Before/after patient photos or case documentation
- Practice management system exports containing patient data
- Any content that could identify a specific patient

Safe sources: public websites, competitor service pages, ADA publications,
dental trade publications, local news, public review aggregates (themes only),
DataForSEO exports, Firecrawl crawls of public pages.

If unsure whether a source contains PHI — do not add it.

---

## Step 0: Setup (First Use Only)

### Check Python version

```bash
python3 --version
```

If below 3.10, install via Homebrew:
```bash
brew install python@3.12
```

### Install the CLI

```bash
PYTHON=$(command -v python3.12 2>/dev/null || command -v python3.11 2>/dev/null || command -v python3.10 2>/dev/null || command -v python3)
$PYTHON -c "import sys; assert sys.version_info >= (3,10), f'Python {sys.version} is too old'; print(f'Using {sys.version}')"
$PYTHON -m venv ~/.notebooklm-venv
source ~/.notebooklm-venv/bin/activate
pip install "notebooklm-py[browser]"
playwright install chromium
mkdir -p ~/bin
ln -sf ~/.notebooklm-venv/bin/notebooklm ~/bin/notebooklm
export PATH="$HOME/bin:$PATH"
notebooklm --help
```

### Authenticate

Tell the user: "I'll open a browser — sign into your Google account and navigate to notebooklm.google.com. I'll wait for your confirmation."

```bash
cat > /tmp/nlm_login.py << 'PYEOF'
import json, os, time
from pathlib import Path
from playwright.sync_api import sync_playwright

STORAGE_PATH = Path.home() / ".notebooklm" / "storage_state.json"
PROFILE_PATH = Path.home() / ".notebooklm" / "browser_profile"
SIGNAL_FILE = Path("/tmp/nlm_save_signal")
SIGNAL_FILE.unlink(missing_ok=True)
STORAGE_PATH.parent.mkdir(parents=True, exist_ok=True)
print("Opening browser for Google login...")
with sync_playwright() as p:
    browser = p.chromium.launch_persistent_context(
        user_data_dir=str(PROFILE_PATH), headless=False,
        args=["--disable-blink-features=AutomationControlled"])
    page = browser.pages[0] if browser.pages else browser.new_page()
    page.goto("https://notebooklm.google.com/")
    print("Browser open. Waiting for save signal...")
    while not SIGNAL_FILE.exists():
        time.sleep(1)
    storage = browser.storage_state()
    with open(STORAGE_PATH, "w") as f:
        json.dump(storage, f)
    print(f"Saved {len(storage.get('cookies', []))} cookies")
    browser.close()
SIGNAL_FILE.unlink(missing_ok=True)
print(f"Auth saved to: {STORAGE_PATH}")
PYEOF
source ~/.notebooklm-venv/bin/activate
python3 /tmp/nlm_login.py > /tmp/nlm_login_output.txt 2>&1 &
echo "Browser opening (PID=$!)..."
```

Wait ~10 seconds, confirm with the user, then save the session:
```bash
touch /tmp/nlm_save_signal && sleep 8 && cat /tmp/nlm_login_output.txt
```

Verify:
```bash
export PATH="$HOME/bin:$PATH"
notebooklm auth check && notebooklm list
```

Cleanup: `rm -f /tmp/nlm_login.py /tmp/nlm_login_output.txt /tmp/nlm_save_signal`

If auth fails (SID missing): `rm -rf ~/.notebooklm/browser_profile ~/.notebooklm/storage_state.json` then retry.

---

## Dental Marketing Research Workflows

For SE Florida dental research question templates, see `references/dental-research-templates.md`.

### Market Research Workflow

Use when building a market brief for a new client or quarterly refresh.

```bash
export PATH="$HOME/bin:$PATH"
notebooklm create "Market Research: [client-name] — [service] — [YYYY-MM]"
notebooklm use <notebook_id>

# Add sources (examples — adapt per client)
notebooklm source add "https://[competitor-website].com"
notebooklm source add "https://[local-news-source].com/dental"
notebooklm source add ./data/raw/firecrawl/{client-slug}/market-research-*.md

notebooklm source wait --all
```

Ask the structured market research questions from `references/dental-research-templates.md`:
```bash
notebooklm ask "$(cat .claude/skills/notebooklm-research-os/references/dental-research-templates.md | grep -A5 'Market Research Questions')"
```

Generate the briefing document:
```bash
notebooklm generate report --format briefing-doc
notebooklm artifact wait <artifact_id>
notebooklm download report ./research/notebooklm/exported-notes/{client-slug}-market-{YYYY-MM-DD}.md
```

### Competitor Analysis Workflow

```bash
notebooklm create "Competitor Analysis: [client-name] vs [competitor] — [YYYY-MM]"
notebooklm use <notebook_id>

# Add competitor public sources only — no private or logged-in content
notebooklm source add "https://[competitor].com"
notebooklm source add "https://[competitor-gbp-url]"

notebooklm source wait --all
notebooklm generate report --format study-guide
notebooklm artifact wait <artifact_id>
notebooklm download report ./research/notebooklm/exported-notes/{client-slug}-competitor-{YYYY-MM-DD}.md
```

### Proposal Research Workflow

For prospective client proposals — builds a research packet before the pitch.

```bash
notebooklm create "Proposal Research: [prospect-name] — [YYYY-MM]"
notebooklm use <notebook_id>
notebooklm source add "https://[prospect-website].com"
notebooklm source add-research "dental [service] [city] market trends" --mode deep --no-wait
notebooklm research wait --import-all
notebooklm generate report --format briefing-doc
notebooklm artifact wait <artifact_id>
notebooklm download report ./research/notebooklm/exported-notes/{prospect-slug}-proposal-{YYYY-MM-DD}.md
```

---

## Convert Export to Project Artifacts

After downloading any NotebookLM report, convert it into the three project artifacts that downstream skills consume.

**Input:** `research/notebooklm/exported-notes/{slug}-{topic}-{YYYY-MM-DD}.md`

**Step 1 — research-digest.md**
Curated summary of the key findings. Pull the most important sections from the briefing doc. Add a source list and confidence notes. Remove raw transcripts and filler.

Save to: `research/notebooklm/research-digests/{slug}-{topic}-digest.md`

Format:
```markdown
# Research Digest — {topic} — {date}
**Client/prospect:** {name}
**Sources analyzed:** {N}
**Research conducted:** {YYYY-MM-DD}

## Key Findings
[3-7 major findings, each with source attribution]

## Market Signals
[Demand signals, gaps, competitive signals]

## Content Opportunities
[Angles the campaign brief can use directly]

## Reliability Notes
[Any sources that were weak, outdated, or contradictory]
```

**Step 2 — insight-log.md**
Distilled bullet-point insights for the knowledge base. These feed into `_context/` updates and future campaign briefs.

Save to: `research/notebooklm/research-digests/{slug}-{topic}-insights.md`

Format: a flat, dated bullet list — one insight per line, source noted inline.

**Step 3 — source-map.md**
Documents what each source contributed. Enables future validation and re-use.

Save to: `research/notebooklm/research-digests/{slug}-{topic}-sources.md`

Format:
```markdown
| Source | URL | Type | Key contribution | Reliability |
|---|---|---|---|---|
| [name] | [url] | Web page / PDF / YouTube | [what it contributed] | High/Med/Low |
```

**Curated insights only move to `_context/`** once reviewed and confirmed — never raw NotebookLM exports.

---

## Quick Reference

| Task | Command |
|---|---|
| List notebooks | `notebooklm list` |
| Create notebook | `notebooklm create "Title"` |
| Set context | `notebooklm use <id>` |
| Check context | `notebooklm status` |
| Add URL source | `notebooklm source add "https://..."` |
| Add local file | `notebooklm source add ./file.pdf` |
| Wait for source | `notebooklm source wait <id>` |
| Web research (fast) | `notebooklm source add-research "query"` |
| Web research (deep) | `notebooklm source add-research "query" --mode deep --no-wait` |
| Wait for research | `notebooklm research wait --import-all` |
| Ask a question | `notebooklm ask "question"` |
| Generate briefing doc | `notebooklm generate report --format briefing-doc` |
| Generate study guide | `notebooklm generate report --format study-guide` |
| Check artifact status | `notebooklm artifact list` |
| Wait for completion | `notebooklm artifact wait <id>` |
| Download report | `notebooklm download report ./output.md` |

## Autonomy Rules

Run automatically: `status`, `auth check`, `list`, `source list`, `artifact list`, `source wait`, `research status`, `research wait`, `use <id>`, `create`, `ask`, `history`, `source add`

Ask before running: `delete` (destructive), `generate *` (long-running), `download *` (writes files), `ask --save-as-note` (writes note)

## Error Handling

| Error | Cause | Fix |
|---|---|---|
| Auth/cookie error | Session expired | Re-run login script from Step 0 |
| "No notebook context" | Context not set | `notebooklm use <id>` |
| Rate limiting | Google throttle | Wait 5–10 min, retry |
| Download fails | Generation incomplete | Check `artifact list` for status |
