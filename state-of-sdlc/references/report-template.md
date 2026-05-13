# Report Template — State of AI in SDLC

The Telegram-ready markdown template. Used by the drafter (step 5) and re-applied by the auditor (step 3 revise).

> **⚠️ Drafter contract:** The drafter does **not** include any "Quality self-score" block, rubric line items, or scoring rationale. Only the auditor scores. If a draft contains a self-score, it's a pipeline bug — strip it in revision.

```
📡 STATE OF AI IN SDLC — [Date] Edition
━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔑 LEAD SIGNAL
[1 paragraph, 80-120 words. The single most important development.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔍 PATTERNS THIS PERIOD
**[Title]** — [2-3 sentences evidence, 1 sentence "so what"]. **What to do this week:** [1 concrete, testable action the reader can take in their own team this week — not an aspiration. Examples: "audit your repo for harness artifacts — if AGENTS.md isn't owned by a named engineer, it isn't owned"; "require a 3-sentence 'why this design' comment in any PR where the author didn't write the majority of the code."]
[3-5 patterns, each ending with its own **What to do this week:**]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚡ TENSION / COUNTER-SIGNAL
[1 paragraph, 60-80 words. Where credible sources disagree.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

🏗️ ORG & ROLES WATCH
[3-4 bullets, 60-100 words]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚠️ ANTI-PATTERNS & FAILURES
[1 paragraph or 3-4 bullets, 60-80 words. Required section. Each anti-pattern ends with **Mechanism to prevent:** [concrete check/rule/process, not a principle].]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔭 FORWARD SIGNAL
[3-4 bullets. What to watch next week.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

📚 SOURCES
[Linked list: [Source Name](url) · [Source Name](url) · ...]
```

The auditor adds the `📊 Pipeline Stats` block after revision. The drafter never writes it.

## Two non-negotiable rules

These are quality-gate requirements:

1. **Every pattern in "Patterns This Period" ends with `**What to do this week:**`** — one concrete, testable action. Not "teams should consider…" — a real testable instruction. The auditor downgrades the report if patterns end in aspiration.

2. **Every item in "Anti-Patterns & Failures" ends with `**Mechanism to prevent:**`** — a concrete check, rule, or process. Not a principle. Not a value. A specific mechanism.

These rules force the report to be operational rather than observational. They're how the report earns its place on a CTO's Monday morning.

## Section-omission rules

| Section | Rule |
|---------|------|
| Lead Signal | Always required. If insufficient signal, regenerate from Step 4 with a wider net. |
| Patterns | 3–5 minimum. Below 3, the report fails the quality gate. |
| Tension | Required. If no genuine intellectual conflict surfaced, the auditor flags missing dialogue. |
| Org & Roles | Required. Even if soft, must surface at least one structural shift signal. |
| Anti-Patterns | Required, every edition. This section is the report's ballast against AI-engineering hype. |
| Forward Signal | Required. ≥1 dated upcoming event preferred. |
| Sources | Required. ≥8 unique sources cited. |

## Delivery format

The same template is rendered into:

1. **Telegram inline message** — split at section boundaries if it exceeds 4096 chars.
2. **Markdown attachment** — `YYYY-MM-DD-slug.md`, the canonical version.
3. **S3 final artifact** — `s3://ai-sdlc-reports-{ACCOUNT_ID}/{REPORT_DATE}/final.md`.
4. **Local mirror** — `{WORKSPACE}/projects/ai-sdlc-report/reports/{REPORT_DATE}-{slug}.md` for grep/reference.
