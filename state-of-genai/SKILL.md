---
name: state-of-genai
description: "Operate the weekly State of GenAI report — a Sunday recurring AI-economics briefing covering model releases, pricing dynamics, infrastructure, agentic tools, AWS/Bedrock, and regulation. Use when: (1) running, debugging, or replaying the GenAI report pipeline, (2) reviewing the methodology, source registry, or scoring rubric, (3) extending or adapting the GenAI report into another domain, (4) reproducing the drafter/auditor split (Sonnet 4.6 → Opus 4.7) for a similar weekly research report. NOT for: one-off ad-hoc AI-news summaries (just search), creating a brand new domain from scratch (use domain-analyst), or modifying the live cron schedule directly."
---

# State of GenAI — Weekly AI Economics Report

A weekly recurring research report scheduled for **Sunday 9:00 AM Israel time (UTC+3)**, written by an "AI Economist" persona. This skill ships the methodology, source registry pattern, drafter/auditor pipeline, and all reference material needed to run, replay, debug, or fork the report.

## What this skill is for

Use this skill when you need to:

1. **Run or replay the pipeline** — understand the 8-step checkpointed flow (drafter steps 0–6, auditor steps 1–4) and resume from the last completed S3 checkpoint.
2. **Inspect the methodology** — see what the AI Economist filters for/against, the 13 themes tracked, the quality gate, and the report template.
3. **Review the source registry pattern** — how Tier 1/2/3 sources evolve, when sources get promoted/demoted, how new sources are discovered.
4. **Fork the report** — adapt the pattern to another weekly domain (use `domain-analyst` skill for greenfield reports).

## Skill File Map

| File | Purpose | When to load |
|------|---------|--------------|
| [references/METHODOLOGY.md](references/METHODOLOGY.md) | Full agent profile, themes, 8-step pipeline, scoring rubric, report template, quality gate | Load when running the report, debugging output quality, or adapting the methodology |
| [references/sources.md](references/sources.md) | Source registry — Tier 1/2/3 sources with RSS feeds, Tavily queries, promotion/demotion log | Load at the start of every run; update at the end if sources changed |
| [references/cron-payloads.md](references/cron-payloads.md) | The two cron job payloads (drafter + auditor) — exact prompts, S3 paths, telemetry format, delivery flow | Load when re-creating crons in a fresh OpenClaw deployment, or debugging cron behavior |
| [references/report-template.md](references/report-template.md) | The standalone Telegram-ready template with all section headers and dividers | Load when reformatting a report or when the methodology template needs to be referenced in isolation |

**Progressive disclosure rule:** load only the file the current step needs.

## Pipeline At-A-Glance

```
Sunday 09:00 IDT  ─►  weekly-genai-draft   (Sonnet 4.6, 15min budget)
                       ├─ Step 0: Load last 3 reports
                       ├─ Step 1: Model release & benchmark sweep
                       ├─ Step 2: Pricing & infrastructure sweep
                       ├─ Step 3: Industry & open source sweep
                       ├─ Step 4: Source discovery
                       ├─ Step 5: Score & filter (every result scored)
                       └─ Step 6: Synthesize → step-6-draft.md
Sunday 09:20 IDT  ─►  weekly-genai-audit   (Opus 4.7, 15min budget)
                       ├─ Step 1: Load draft + scoring + drafter stats
                       ├─ Step 2: Audit (4-dim rubric, harsh) → step-7-audit.json
                       ├─ Step 3: Revise → step-8-revised.md
                       ├─ Step 4: Final grade → step-9-final-grade.json
                       └─ Publish → final.md (S3 + local mirror + Telegram)
```

All artifacts under `s3://genai-reports-{ACCOUNT_ID}/{REPORT_DATE}/`. Checkpoints persist for evals — do **not** delete them.

## Quick Operations

### Replay a report from a specific step
```bash
DATE=2026-05-12
aws s3 ls s3://genai-reports-{ACCOUNT_ID}/${DATE}/
# pipeline auto-resumes from first missing step
```

### Force a re-audit
```bash
aws s3 rm s3://genai-reports-{ACCOUNT_ID}/${DATE}/step-7-audit.json
aws s3 rm s3://genai-reports-{ACCOUNT_ID}/${DATE}/step-8-revised.md
aws s3 rm s3://genai-reports-{ACCOUNT_ID}/${DATE}/step-9-final-grade.json
aws s3 rm s3://genai-reports-{ACCOUNT_ID}/${DATE}/final.md
# trigger audit cron manually
```

### Inspect quality trend
```bash
for d in $(aws s3 ls s3://genai-reports-{ACCOUNT_ID}/ | grep PRE | awk '{print $2}' | sort -r | head -10); do
  aws s3 cp s3://genai-reports-{ACCOUNT_ID}/${d}step-9-final-grade.json - 2>/dev/null | \
    jq -r --arg d "$d" '"\($d): final \(.total)/10 (draft \(.delta_from_draft.draft_score)) — \(.summary // "no summary")"'
done
```

## Drafter / Auditor Split

The report deliberately uses two different models in two different sessions:

- **Drafter (Sonnet 4.6)** — fast, cheap research and synthesis. Does NOT self-score.
- **Auditor (Opus 4.7)** — slower, more critical reasoning. Audits the draft as if it were mediocre, applies improvement instructions, re-grades the revised version.

This split was chosen because: (1) drafters who self-score consistently inflate quality, (2) Opus 4.7 catches structural issues Sonnet 4.6 missed, (3) the delta from `draft_score → final_score` is itself a useful pipeline-quality metric (typical lift: +3 to +6 points after revision).

The auditor *must not* have read the draft when it was being written — that's why it runs in a separate isolated session 20 minutes later.

## Quality Gate

Final report must score ≥7/10 on the 4-dimension rubric (Sources, Coverage, Insight, Freshness) — see [METHODOLOGY.md](references/METHODOLOGY.md) section 6 for the full rubric. Below 5: regenerate. 5–6: revise. The auditor pipeline applies this automatically.

## Configuration Placeholders

Throughout the reference files you'll see:

- `{ACCOUNT_ID}` — your AWS account ID (12 digits)
- `{TELEGRAM_CHAT_ID}` — recipient chat ID for delivery
- `{S3_BUCKET}` — derived as `genai-reports-{ACCOUNT_ID}`
- `{REPORT_DATE}` — `YYYY-MM-DD`, set at run start
- `{SEARCH_FROM}` — 7 days before report date

Substitute these at deployment time. The cron payloads and methodology never hard-code the production values.

## Related Skills

- `domain-analyst` — use this if you're building a **brand new** domain report. This skill (state-of-genai) is the operational reference for the already-running GenAI report.
- `state-of-sdlc` — sister report covering AI in software development lifecycle, runs Mondays.
