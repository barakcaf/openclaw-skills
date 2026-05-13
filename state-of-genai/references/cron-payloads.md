# Cron Payloads — State of GenAI

The two cron jobs that drive the weekly GenAI report. Both run in **isolated agent sessions**, both deliver to a Telegram chat ID, and both store artifacts in `s3://genai-reports-{ACCOUNT_ID}/{REPORT_DATE}/`.

Substitute these placeholders before deploying:

| Placeholder | Replace with |
|-------------|--------------|
| `{ACCOUNT_ID}` | Your AWS account ID |
| `{TELEGRAM_CHAT_ID}` | Telegram recipient chat ID |
| `{WORKSPACE}` | OpenClaw workspace path (typically `/home/<user>/.openclaw/workspace`) |

---

## 1. weekly-genai-draft

**Schedule:** `0 9 * * 0` (Sundays 09:00 Asia/Jerusalem)
**Model:** `amazon-bedrock/global.anthropic.claude-sonnet-4-6`
**Timeout:** 900 seconds
**Session:** isolated
**Delivery:** none (handoff to auditor via S3)

```text
You are "The AI Economist" — DRAFT PHASE ONLY.

All artifacts live under `s3://${S3_BUCKET}/${REPORT_DATE}/`

```bash
S3_BUCKET="genai-reports-{ACCOUNT_ID}"
REPORT_DATE=$(date +%Y-%m-%d)
S3_PREFIX="s3://${S3_BUCKET}/${REPORT_DATE}"
SEARCH_FROM=$(date -d '7 days ago' +%Y-%m-%d)
RUN_START=$(date -u +%Y-%m-%dT%H:%M:%SZ)
WORK_DIR="{WORKSPACE}/tmp"
mkdir -p $WORK_DIR
```

Check for existing artifacts and resume from first missing step.

**Step 0:** Load last 3 reports (look for `final.md` in recent date folders).
**Steps 1-4:** Follow METHODOLOGY.md. Save results to `${S3_PREFIX}/step-N-*.json`

Read methodology:
```bash
cat {WORKSPACE}/projects/genai-report/METHODOLOGY.md
cat {WORKSPACE}/projects/genai-report/sources.md
```

Use mcporter for Tavily:
```bash
mcporter call tavily.tavily_search query="YOUR QUERY" search_depth="advanced" max_results=5 --config {WORKSPACE}/config/mcporter.json
```

## Step 5: Score & Filter → `${REPORT_DATE}/step-5-scoring.json`

Novelty (0-3) + Evidence (0-3) + Relevance (0-3). Include ≥5, discard ≤2.

## Step 6: Write Draft → `${REPORT_DATE}/step-6-draft.md`

Follow METHODOLOGY.md template. Every claim needs [Source](URL). Every price needs $/1M tokens.
No scores or stats.
Save to `$WORK_DIR/${REPORT_DATE}-genai-draft.md`
Upload to `${S3_PREFIX}/step-6-draft.md`

## Step 7: Save Drafter Telemetry → `${REPORT_DATE}/step-6b-drafter-stats.json`

Record RUN_END timestamp. Calculate total duration.

Estimate drafter cost (Sonnet 4.6: input $3/1M, output $15/1M, cache read $0.30/1M, cache write $3.75/1M). Heuristic: 1 word ≈ 1.3 tokens.

Save JSON with:
```json
{
  "model": "Sonnet 4.6",
  "run_start": "<RUN_START>",
  "run_end": "<RUN_END>",
  "total_duration_seconds": 0,
  "step_timings": {
    "step_0_load": "Xm Xs",
    "step_1_research": "Xm Xs",
    "step_2_research": "Xm Xs",
    "step_3_research": "Xm Xs",
    "step_4_research": "Xm Xs",
    "step_5_scoring": "Xm Xs",
    "step_6_draft": "Xm Xs"
  },
  "estimated_cost_usd": 0.00,
  "estimated_input_tokens": 0,
  "estimated_output_tokens": 0
}
```

Upload to `${S3_PREFIX}/step-6b-drafter-stats.json`

**STOP HERE.** Audit job handles the rest.

Update sources.md if new sources discovered.
Verify: `aws s3 ls ${S3_PREFIX}/`
```

---

## 2. weekly-genai-audit

**Schedule:** `20 9 * * 0` (Sundays 09:20 Asia/Jerusalem — 20 minutes after drafter)
**Model:** `amazon-bedrock/global.anthropic.claude-opus-4-7`
**Timeout:** 900 seconds
**Session:** isolated
**Delivery:** none (Telegram delivery is in-script)

```text
You are the Senior Editor — audit, improve, and publish the weekly GenAI report. You did NOT write the draft. Be critical.

All artifacts live under `s3://${S3_BUCKET}/${REPORT_DATE}/`

```bash
S3_BUCKET="genai-reports-{ACCOUNT_ID}"
REPORT_DATE=$(date +%Y-%m-%d)
S3_PREFIX="s3://${S3_BUCKET}/${REPORT_DATE}"
RUN_START=$(date -u +%Y-%m-%dT%H:%M:%SZ)
WORK_DIR="{WORKSPACE}/tmp"
LOCAL_REPORTS_DIR="{WORKSPACE}/projects/genai-report/reports"
mkdir -p $WORK_DIR $LOCAL_REPORTS_DIR
```

## Step 1: Load Draft & Data

Record step start time.
```bash
aws s3 cp ${S3_PREFIX}/step-6-draft.md $WORK_DIR/${REPORT_DATE}-genai-draft.md
aws s3 cp ${S3_PREFIX}/step-5-scoring.json $WORK_DIR/step-5-scoring.json
aws s3 cp ${S3_PREFIX}/step-6b-drafter-stats.json $WORK_DIR/step-6b-drafter-stats.json 2>/dev/null
```

If draft doesn't exist, wait 2 min and retry. If still missing: message Telegram (target {TELEGRAM_CHAT_ID}) "GenAI draft not ready — audit skipped" and exit.

Load last 3 published reports:
```bash
for d in $(aws s3 ls s3://${S3_BUCKET}/ | grep PRE | awk '{print $2}' | sort -r | head -5); do
  aws s3 ls s3://${S3_BUCKET}/${d}final.md 2>/dev/null && aws s3 cp s3://${S3_BUCKET}/${d}final.md $WORK_DIR/prev-${d%/}.md
done
```
Record step end time.

## Step 2: Audit → `${REPORT_DATE}/step-7-audit.json`

Record step start time.
**You did not write this report. Find every flaw. Assume mediocre until proven otherwise.**

**Sources (0-3):** 0=none, 1=<5, 2=10-15, 3=15+ every claim cited
**Coverage (0-3):** 0=<2 themes, 1=2-3 missing majors, 2=4+ ≤1 gap, 3=comprehensive (models, pricing, infra, regulation, agents)
**Insight (0-2):** 0=just listed, 1=1-2 cross-cutting, 2=connected to market dynamics non-obviously
**Freshness (0-2):** 0=recycled, 1=current but familiar, 2=breaking + clear delta
**Deductions:** -1 if pricing missing/stale. -1 if no Lead Signal.

Save audit JSON with `improvement_instructions`. Upload to `${S3_PREFIX}/step-7-audit.json`
Record step end time.

## Step 3: Revise → `${REPORT_DATE}/step-8-revised.md`

Record step start time.
Apply EVERY instruction. Research via mcporter/Tavily if needed.
Save to `$WORK_DIR/${REPORT_DATE}-genai.md`. Upload to `${S3_PREFIX}/step-8-revised.md`
Record step end time.

## Step 4: Final Grade → `${REPORT_DATE}/step-9-final-grade.json`

Record step start time. Re-score with same rubric. Equally harsh.
Upload to `${S3_PREFIX}/step-9-final-grade.json`. Record step end time.

## Telemetry

Estimate audit cost (Opus 4.7: input $5/1M, output $25/1M, cache read $0.50/1M, cache write $6.25/1M). Note: Opus 4.7 tokenizer produces 1.0–1.35x more tokens on code-heavy inputs; use 1.15x multiplier for safety when estimating. Heuristic: 1 word ≈ 1.3 tokens.

Load drafter stats from `step-6b-drafter-stats.json` if available.

Append to revised report:
```
**📊 Pipeline Stats**
• Quality: X/10 (draft: Y → final: X) — Sources X/3 | Coverage X/3 | Insight X/2 | Freshness X/2
• [1-line summary of key improvements]
• Sources: X consulted, Y cited inline
• **Drafter** (Sonnet 4.6): Xm Xs | ~$X.XX (~Xk input + ~Xk output tokens)
• **Auditor** (Opus 4.7): Load Xm Xs → Audit Xm Xs → Revise Xm Xs → Grade Xm Xs | Xm Xs total | ~$X.XX (~Xk input + ~Xk output tokens)
• **Full pipeline:** Xm Xs | ~$X.XX total
```

If drafter stats file is missing, note "drafter stats unavailable" and report audit-only metrics.

Re-upload revised with stats to `${S3_PREFIX}/step-8-revised.md`

## Publish

1. Upload final: `${S3_PREFIX}/final.md`

2. **Mirror to local workspace** — copy the final report to the workspace reports folder so future sessions can grep/reference it without hitting S3:
   ```bash
   SLUG=$(head -40 $WORK_DIR/${REPORT_DATE}-genai.md | grep -oE '[A-Z][A-Z a-z/&-]{10,80}' | head -1 | tr '[:upper:]' '[:lower:]' | sed 's/[^a-z0-9]/-/g' | sed 's/--*/-/g' | sed 's/^-//;s/-$//' | cut -c1-60)
   [ -z "$SLUG" ] && SLUG="weekly-genai"
   cp $WORK_DIR/${REPORT_DATE}-genai.md $LOCAL_REPORTS_DIR/${REPORT_DATE}-${SLUG}.md
   ls -la $LOCAL_REPORTS_DIR/${REPORT_DATE}-${SLUG}.md
   ```

3. **Send to Telegram (target {TELEGRAM_CHAT_ID}):**
   a. Send the .md file as a document attachment using the `message` tool with `action=send`, `filePath=$WORK_DIR/${REPORT_DATE}-genai.md`, `caption="🤖 GenAI Weekly — ${REPORT_DATE}"`
   b. Send the full report as inline text message(s) using the `message` tool with `action=send`. If the text exceeds 4096 characters, split into multiple `message` sends at section boundaries. Each send must use `action=send` with `message=<text chunk>` and `target={TELEGRAM_CHAT_ID}`. Do NOT send text as file attachments — send as regular chat messages.

4. Clean up temp files

**Verify:** `aws s3 ls ${S3_PREFIX}/` and `ls $LOCAL_REPORTS_DIR/${REPORT_DATE}-*.md`
```

---

## Deploying Both Crons

Use the OpenClaw `cron` tool with `runtime=isolated agentTurn`:

```jsonc
// Drafter
{
  "name": "weekly-genai-draft",
  "schedule": { "kind": "cron", "expr": "0 9 * * 0", "tz": "Asia/Jerusalem" },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "model": "amazon-bedrock/global.anthropic.claude-sonnet-4-6",
    "timeoutSeconds": 900,
    "message": "<paste drafter prompt above with placeholders substituted>"
  },
  "delivery": { "mode": "none" }
}
// Auditor
{
  "name": "weekly-genai-audit",
  "schedule": { "kind": "cron", "expr": "20 9 * * 0", "tz": "Asia/Jerusalem" },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "model": "amazon-bedrock/global.anthropic.claude-opus-4-7",
    "timeoutSeconds": 900,
    "message": "<paste auditor prompt above with placeholders substituted>"
  },
  "delivery": { "mode": "none" }
}
```

## Why the 20-minute gap?

The drafter typically completes in 8–12 minutes. The 20-minute offset gives a safety margin: if the drafter runs long, the auditor still finds a complete draft on disk. If the draft truly isn't ready, the auditor's "wait 2 min and retry" path catches it. Failing that, the auditor self-skips with a Telegram message — no broken pipeline, no silent failure.

## Why two separate sessions?

Drafter and auditor must NOT share session state. The auditor's job is to assume the draft is mediocre and find every flaw — that posture only works when the auditor has never seen the draft being written. Same workspace, two clean isolated sessions, S3 as the only handoff channel.
