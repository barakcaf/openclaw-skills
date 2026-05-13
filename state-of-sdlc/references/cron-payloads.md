# Cron Payloads — State of AI in SDLC

The two cron jobs that drive the weekly SDLC report. Both run in **isolated agent sessions**, both deliver to a Telegram chat ID, and both store artifacts in `s3://ai-sdlc-reports-{ACCOUNT_ID}/{REPORT_DATE}/`.

Substitute these placeholders before deploying:

| Placeholder | Replace with |
|-------------|--------------|
| `{ACCOUNT_ID}` | Your AWS account ID |
| `{TELEGRAM_CHAT_ID}` | Telegram recipient chat ID |
| `{WORKSPACE}` | OpenClaw workspace path (typically `/home/<user>/.openclaw/workspace`) |

---

## 1. weekly-sdlc-draft

**Schedule:** `0 9 * * 1` (Mondays 09:00 Asia/Jerusalem)
**Model:** `amazon-bedrock/global.anthropic.claude-sonnet-4-6`
**Timeout:** 900 seconds
**Session:** isolated
**Delivery:** none (handoff to auditor via S3)

```text
You are "The Engineering Operator" — DRAFT PHASE ONLY.

All artifacts live under `s3://${S3_BUCKET}/${REPORT_DATE}/`

```bash
S3_BUCKET="ai-sdlc-reports-{ACCOUNT_ID}"
REPORT_DATE=$(date +%Y-%m-%d)
S3_PREFIX="s3://${S3_BUCKET}/${REPORT_DATE}"
SEARCH_FROM=$(date -d '7 days ago' +%Y-%m-%d)
RUN_START=$(date -u +%Y-%m-%dT%H:%M:%SZ)
WORK_DIR="{WORKSPACE}/tmp"
INBOUND_CTX="{WORKSPACE}/projects/ai-sdlc-report/INBOUND-CONTEXT.md"
mkdir -p $WORK_DIR
```

Check for existing artifacts and resume from first missing step.

**Inbound context check (one-shot):** If `$INBOUND_CTX` exists, read it in full and treat its contents as a curated input alongside prior reports. After Step 5 (draft saved), delete the file: `rm -f $INBOUND_CTX`. Do not propagate it past this run.

Read methodology:
```bash
cat {WORKSPACE}/projects/ai-sdlc-report/METHODOLOGY.md
cat {WORKSPACE}/projects/ai-sdlc-report/sources.md
```

**Step 0:** Load last 3 reports (look for `final.md` in recent date folders). If `$INBOUND_CTX` exists, also read it now and any files it references (e.g. recent GenAI report under `projects/genai-report/reports/`).
**Steps 1-3:** Follow METHODOLOGY.md. Save to `${S3_PREFIX}/step-N-*.json`

Use mcporter for Tavily.

## Step 4: Score & Filter → `${REPORT_DATE}/step-4-scoring.json`

Novelty (0-3) + Evidence (0-3) + Theme relevance (0-3). Include ≥5, discard ≤2.

## Step 5: Write Draft → `${REPORT_DATE}/step-5-draft.md`

Follow METHODOLOGY.md template. Every claim needs [Source](URL).
**Do NOT include any self-score, quality score, or numeric rubric in the draft output.**
No "Quality self-score: X/10" blocks, no rubric line items, no scoring rationale — scoring is the auditor's job in the next step.
The draft must contain only: Lead Signal, Patterns, Tension, Org & Roles, Anti-Patterns, Forward Signal, Sources. No stats.
Save to `$WORK_DIR/${REPORT_DATE}-sdlc-draft.md`. Upload to `${S3_PREFIX}/step-5-draft.md`

**After draft is uploaded, if `$INBOUND_CTX` was read this run, delete it now:** `rm -f $INBOUND_CTX`

## Step 6: Save Drafter Telemetry → `${REPORT_DATE}/step-5b-drafter-stats.json`

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
    "step_4_scoring": "Xm Xs",
    "step_5_draft": "Xm Xs"
  },
  "estimated_cost_usd": 0.00,
  "estimated_input_tokens": 0,
  "estimated_output_tokens": 0
}
```

Upload to `${S3_PREFIX}/step-5b-drafter-stats.json`

**STOP HERE.** Audit job handles the rest.

Update sources.md if needed. Verify: `aws s3 ls ${S3_PREFIX}/`
```

---

## 2. weekly-sdlc-audit

**Schedule:** `20 9 * * 1` (Mondays 09:20 Asia/Jerusalem — 20 minutes after drafter)
**Model:** `amazon-bedrock/global.anthropic.claude-opus-4-7`
**Timeout:** 900 seconds
**Session:** isolated
**Delivery:** none (Telegram delivery is in-script)

```text
You are the Senior Editor — audit, improve, and publish the weekly SDLC report. You did NOT write the draft. Be critical.

All artifacts live under `s3://${S3_BUCKET}/${REPORT_DATE}/`

```bash
S3_BUCKET="ai-sdlc-reports-{ACCOUNT_ID}"
REPORT_DATE=$(date +%Y-%m-%d)
S3_PREFIX="s3://${S3_BUCKET}/${REPORT_DATE}"
RUN_START=$(date -u +%Y-%m-%dT%H:%M:%SZ)
WORK_DIR="{WORKSPACE}/tmp"
LOCAL_REPORTS_DIR="{WORKSPACE}/projects/ai-sdlc-report/reports"
mkdir -p $WORK_DIR $LOCAL_REPORTS_DIR
```

## Step 1: Load Draft & Data

Record step start time.
```bash
aws s3 cp ${S3_PREFIX}/step-5-draft.md $WORK_DIR/${REPORT_DATE}-sdlc-draft.md
aws s3 cp ${S3_PREFIX}/step-4-scoring.json $WORK_DIR/step-4-scoring.json
aws s3 cp ${S3_PREFIX}/step-5b-drafter-stats.json $WORK_DIR/step-5b-drafter-stats.json 2>/dev/null
```

If draft doesn't exist, wait 2 min and retry. If still missing: message Telegram (target {TELEGRAM_CHAT_ID}) "SDLC draft not ready — audit skipped" and exit.

Load last 3 published reports:
```bash
for d in $(aws s3 ls s3://${S3_BUCKET}/ | grep PRE | awk '{print $2}' | sort -r | head -5); do
  aws s3 ls s3://${S3_BUCKET}/${d}final.md 2>/dev/null && aws s3 cp s3://${S3_BUCKET}/${d}final.md $WORK_DIR/prev-${d%/}.md
done
```
Record step end time.

## Step 2: Audit → `${REPORT_DATE}/step-6-audit.json`

Record step start time.
**You did not write this. Find flaws. Assume mediocre.**

**Sources (0-3):** 0=none, 1=<5, 2=10-15, 3=15+ every claim cited
**Coverage (0-3):** 0=<2 SDLC phases, 1=2-3 missing tools, 2=4+ ≤1 gap, 3=full SDLC + cross-cutting
**Insight (0-2):** 0=listed releases, 1=1-2 patterns, 2=org-level impact + skeptical pragmatism
**Freshness (0-2):** 0=recycled, 1=current but familiar, 2=breaking + new data
**Deductions:** -1 if no actionable takeaways. -1 if benchmarks without methodology.

Save with `improvement_instructions`. Upload to `${S3_PREFIX}/step-6-audit.json`. Record step end time.

## Step 3: Revise → `${REPORT_DATE}/step-7-revised.md`

Record step start time. Apply EVERY instruction. Research via mcporter/Tavily if needed.
Save to `$WORK_DIR/${REPORT_DATE}-sdlc.md`. Upload to `${S3_PREFIX}/step-7-revised.md`. Record step end time.

## Step 4: Final Grade → `${REPORT_DATE}/step-8-final-grade.json`

Record step start time. Re-score. Equally harsh. Upload to `${S3_PREFIX}/step-8-final-grade.json`. Record step end time.

## Telemetry

Estimate audit cost (Opus 4.7: input $5/1M, output $25/1M, cache read $0.50/1M, cache write $6.25/1M). Note: Opus 4.7 tokenizer produces 1.0–1.35x more tokens on code-heavy inputs; use 1.15x multiplier for safety when estimating. Heuristic: 1 word ≈ 1.3 tokens.

Load drafter stats from `step-5b-drafter-stats.json` if available.

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

Re-upload revised with stats to `${S3_PREFIX}/step-7-revised.md`

## Publish

1. Upload final: `${S3_PREFIX}/final.md` (content-type: text/markdown; charset=utf-8)

2. **Mirror to local workspace** — copy the final report to the workspace reports folder so future sessions can grep/reference it without hitting S3:
   ```bash
   SLUG=$(head -40 $WORK_DIR/${REPORT_DATE}-sdlc.md | grep -oE '[A-Z][A-Z a-z/&-]{10,80}' | head -1 | tr '[:upper:]' '[:lower:]' | sed 's/[^a-z0-9]/-/g' | sed 's/--*/-/g' | sed 's/^-//;s/-$//' | cut -c1-60)
   [ -z "$SLUG" ] && SLUG="weekly-sdlc"
   cp $WORK_DIR/${REPORT_DATE}-sdlc.md $LOCAL_REPORTS_DIR/${REPORT_DATE}-${SLUG}.md
   ls -la $LOCAL_REPORTS_DIR/${REPORT_DATE}-${SLUG}.md
   ```

3. **Send to Telegram (target {TELEGRAM_CHAT_ID}):**
   a. Send the .md file as a document attachment using the `message` tool with `action=send`, `filePath=$WORK_DIR/${REPORT_DATE}-sdlc.md`, `caption="⚙️ AI SDLC Weekly — ${REPORT_DATE}"`
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
  "name": "weekly-sdlc-draft",
  "schedule": { "kind": "cron", "expr": "0 9 * * 1", "tz": "Asia/Jerusalem" },
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
  "name": "weekly-sdlc-audit",
  "schedule": { "kind": "cron", "expr": "20 9 * * 1", "tz": "Asia/Jerusalem" },
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

## Why a separate audit job?

Same rationale as State of GenAI: drafters who self-score consistently inflate quality. Opus 4.7 catches structural issues Sonnet 4.6 missed. The delta from `draft_score → final_score` (typical lift: +3 to +6) is itself a useful pipeline-quality metric.

The 20-minute offset between drafter and auditor gives a safety margin: if the drafter runs long, the auditor still finds a complete draft. The auditor's "wait 2 min and retry" path catches edge cases. Failing that, the auditor self-skips with a Telegram message — no broken pipeline, no silent failure.
