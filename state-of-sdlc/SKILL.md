---
name: state-of-sdlc
description: "Operate the weekly State of AI in SDLC report — a Monday recurring engineering-leadership briefing covering agentic AI in software development, productivity metrics, agent-harness architecture, org/role changes, anti-patterns, and dev-experience. Use when: (1) running, debugging, or replaying the SDLC report pipeline, (2) reviewing the methodology, source registry, or scoring rubric, (3) extending or adapting the SDLC report into another engineering-leadership domain, (4) reproducing the drafter/auditor split (Sonnet 4.6 → Opus 4.7) for a similar weekly research report. NOT for: ad-hoc developer-tool reviews (just read the docs), creating a brand new domain from scratch (use domain-analyst), or modifying the live cron schedule directly."
---

# State of AI in SDLC — Weekly Engineering-Leadership Report

A weekly recurring research report scheduled for **Monday 9:00 AM Israel time (UTC+3)**, written by an "Engineering Operator" persona — a VP/SVP-of-engineering voice briefing CTO peers on what actually changed in AI-assisted software development this week. This skill ships the methodology, source registry pattern, drafter/auditor pipeline, and all reference material needed to run, replay, debug, or fork the report.

## What this skill is for

Use this skill when you need to:

1. **Run or replay the pipeline** — understand the 5-step drafter and 4-step auditor flow and resume from the last completed S3 checkpoint.
2. **Inspect the methodology** — see what the Engineering Operator filters for/against, the 7 themes tracked, the quality gate, and the report template (with mandatory `**What to do this week:**` actions and `**Mechanism to prevent:**` lines).
3. **Review the source registry pattern** — how Tier 1/2/3 sources evolve, when sources get promoted/demoted, how new sources are discovered every run.
4. **Fork the report** — adapt the pattern to another weekly engineering-leadership domain (use `domain-analyst` for greenfield reports).

## Skill File Map

| File | Purpose | When to load |
|------|---------|--------------|
| [references/METHODOLOGY.md](references/METHODOLOGY.md) | Full agent profile, themes, 5-step pipeline, scoring rubric, report template, quality gate | Load when running the report, debugging output quality, or adapting the methodology |
| [references/sources.md](references/sources.md) | Source registry — Tier 1/2/3 sources with RSS feeds, Tavily queries, promotion/demotion log, discovered-sources audit trail | Load at the start of every run; update at the end if sources changed |
| [references/cron-payloads.md](references/cron-payloads.md) | The two cron job payloads (drafter + auditor) — exact prompts, S3 paths, telemetry format, delivery flow | Load when re-creating crons in a fresh OpenClaw deployment, or debugging cron behavior |
| [references/report-template.md](references/report-template.md) | The standalone Telegram-ready template with all section headers and mandatory action/mechanism lines | Load when reformatting a report or when the methodology template needs to be referenced in isolation |

**Progressive disclosure rule:** load only the file the current step needs.

## Pipeline At-A-Glance

```
Monday 09:00 IDT  ─►  weekly-sdlc-draft   (Sonnet 4.6, 15min budget)
                       ├─ Step 0: Load last 3 reports + optional inbound context
                       ├─ Step 1: Source sweeps (Tier 1 then Tier 2)
                       ├─ Step 2: Theme cross-cuts
                       ├─ Step 3: Source discovery → updates sources.md
                       ├─ Step 4: Score & filter (every result scored)
                       └─ Step 5: Synthesize → step-5-draft.md
                       ⚠️ Drafter does NOT self-score — auditor scores in next stage
Monday 09:20 IDT  ─►  weekly-sdlc-audit   (Opus 4.7, 15min budget)
                       ├─ Step 1: Load draft + scoring + drafter stats
                       ├─ Step 2: Audit (4-dim rubric, harsh) → step-6-audit.json
                       ├─ Step 3: Revise → step-7-revised.md
                       ├─ Step 4: Final grade → step-8-final-grade.json
                       └─ Publish → final.md (S3 + local mirror + Telegram)
```

All artifacts under `s3://ai-sdlc-reports-{ACCOUNT_ID}/{REPORT_DATE}/`. Checkpoints persist for evals — do **not** delete them.

## Drafter / Auditor Split

Same pattern as the GenAI report (deliberately mirrored for cross-report consistency):

- **Drafter (Sonnet 4.6)** — fast, cheap research and synthesis. Does NOT self-score. The METHODOLOGY explicitly forbids the drafter from including any "Quality self-score" block — that's a pipeline bug to strip in revision.
- **Auditor (Opus 4.7)** — slower, more critical reasoning. Audits the draft as if it were mediocre, applies improvement instructions, re-grades the revised version.

**Why two sessions?** The auditor's job is to find every flaw assuming the draft is mediocre. That posture only works when the auditor never saw the draft being written. Same workspace, two clean isolated sessions, S3 as the only handoff channel.

A cross-report data point: after migrating both pipelines to Opus 4.7 in late April 2026, the SDLC pipeline showed a +6 quality lift on the first run (draft 6 → final 10), validating the model swap.

## What makes this report distinctive

Unlike a general AI-news briefing, the SDLC report enforces two unusual rules:

1. **Every pattern ends with `**What to do this week:**`** — one concrete, testable action the reader can take in their own team this week. Not "teams should consider…" — a real action like "audit your repo for harness artifacts; if AGENTS.md isn't owned by a named engineer, it isn't owned."

2. **Every anti-pattern ends with `**Mechanism to prevent:**`** — a concrete check, rule, or process. Not a principle. This enforces the workspace-wide "discipline comes from mechanism, never from good intention" rule.

Both lines are quality-gate requirements. The auditor downgrades the report if either is missing or if the actions are aspirational rather than testable.

## Quick Operations

### Replay a report from a specific step
```bash
DATE=2026-05-11
aws s3 ls s3://ai-sdlc-reports-{ACCOUNT_ID}/${DATE}/
# pipeline auto-resumes from first missing step
```

### Force a re-audit
```bash
aws s3 rm s3://ai-sdlc-reports-{ACCOUNT_ID}/${DATE}/step-6-audit.json
aws s3 rm s3://ai-sdlc-reports-{ACCOUNT_ID}/${DATE}/step-7-revised.md
aws s3 rm s3://ai-sdlc-reports-{ACCOUNT_ID}/${DATE}/step-8-final-grade.json
aws s3 rm s3://ai-sdlc-reports-{ACCOUNT_ID}/${DATE}/final.md
# trigger audit cron manually
```

### Inspect quality trend
```bash
for d in $(aws s3 ls s3://ai-sdlc-reports-{ACCOUNT_ID}/ | grep PRE | awk '{print $2}' | sort -r | head -10); do
  aws s3 cp s3://ai-sdlc-reports-{ACCOUNT_ID}/${d}step-8-final-grade.json - 2>/dev/null | \
    jq -r --arg d "$d" '"\($d): final \(.total)/10 (draft \(.delta_from_draft.draft_score)) — \(.summary // "no summary")"'
done
```

### Inbound-context handoff (one-shot input)

The drafter supports a one-shot inbound-context channel:

- If `{WORKSPACE}/projects/ai-sdlc-report/INBOUND-CONTEXT.md` exists, the drafter reads it as a curated input alongside prior reports (useful for steering toward a specific topic, or piping in a related GenAI report).
- After the draft is written, the drafter **deletes** the file. It never leaks into subsequent runs.

This is how cross-report continuity is achieved: a fresh GenAI report can be staged as `INBOUND-CONTEXT.md` for the next SDLC run, and the SDLC drafter will reconcile coverage gaps without permanent coupling.

## Quality Gate

Final report must score ≥7/10 on the 4-dimension rubric (Sources, Coverage, Insight, Freshness) — see [METHODOLOGY.md](references/METHODOLOGY.md) section 6 for the full rubric. Below 5: regenerate. 5–6: revise. The auditor pipeline applies this automatically.

## Configuration Placeholders

Throughout the reference files you'll see:

- `{ACCOUNT_ID}` — your AWS account ID (12 digits)
- `{TELEGRAM_CHAT_ID}` — recipient chat ID for delivery
- `{S3_BUCKET}` — derived as `ai-sdlc-reports-{ACCOUNT_ID}`
- `{REPORT_DATE}` — `YYYY-MM-DD`, set at run start
- `{SEARCH_FROM}` — 7 days before report date
- `{WORKSPACE}` — OpenClaw workspace path (typically `/home/<user>/.openclaw/workspace`)

Substitute these at deployment time.

## Related Skills

- `domain-analyst` — use this if you're building a **brand new** domain report. This skill (state-of-sdlc) is the operational reference for the already-running SDLC report.
- `state-of-genai` — sister report covering the broader GenAI economy, runs Sundays.
