# Pipeline Patterns

Choose the pattern that fits the domain, then customize.

## Pattern A: Research-Heavy Weekly (SDLC, GenAI)

Best for: domains where signal comes from many scattered sources that need synthesis.

```
Step 0: Load Context (last 3 reports from S3)
Step 1-N: Research Steps (domain-specific, each saves raw JSON checkpoint)
Step N+1: Score & Filter (every result scored, decisions + reasoning saved)
Step N+2: Synthesize & Draft (only included results, dedup review, quality gate ≥7)
```

**Characteristics:**
- 3-5 research steps, each targeting different source categories
- Heavy Tavily usage (15-30 queries per run)
- Source discovery step hunts for new voices
- 900s timeout typical
- Weekly cadence catches full news cycle

**Example (GenAI):**
1. Model releases & benchmarks
2. Pricing & infrastructure
3. Industry & open source
4. Source discovery
5. Score & filter
6. Synthesize (draft → dedup review → quality gate)

## Pattern B: Data-Driven Daily (Stock Analysis)

Best for: domains with a structured data source + research overlay.

```
Step 1: Fetch Data (API/script → structured JSON)
Step 2: Research Context (targeted searches for movers/outliers)
Step 3: Score & Filter (research results scored against data)
Step 4: Write Analysis (data + scored research → report)
```

**Characteristics:**
- Starts with structured data, not open-ended research
- Research is targeted — only look up what moved
- Shorter pipeline, faster execution
- 600s timeout typical
- Daily cadence, weekdays only for markets

## Pattern C: Hybrid (not yet built, for future domains)

Best for: domains that combine a data feed with periodic deep research.

```
Daily: Pattern B (data + quick research)
Weekly: Pattern A (deep research, source evolution, trend synthesis)
```

Example: A cybersecurity report might do daily CVE monitoring (Pattern B) with weekly threat landscape analysis (Pattern A).

## Universal Pipeline Requirements

Regardless of pattern:

1. **Every step records start/end timestamps**
2. **Every step saves raw data as JSON checkpoint to S3**
3. **Checkpoints are KEPT after delivery** (eval foundation)
4. **Scoring step is mandatory** — no black box between research and synthesis
5. **Generation Stats in every report** (sources, timing, tokens, cost, model)
6. **Continuity** — load last N reports, track narratives, don't repeat leads
7. **Dedup review** — mandatory pass between draft and quality gate. Each development gets ONE primary section; other sections reference with ≤1 sentence. Cross-section entity scan: anything in 3+ sections gets consolidated. Same point in different framing → keep stronger, kill the other.
8. **Quality gate** — self-score, minimum threshold, regenerate if below

## S3 Structure

```
{domain}-reports-{account_id}/
  reports/{date}-slug.md          # final report (canonical)
  checkpoints/{date}/
    step-1-{name}.json            # raw research data
    step-2-{name}.json            # raw research data
    step-N-scoring.json           # scoring decisions + reasoning
  evals/{date}-eval.json          # future: LLM-as-a-judge scores
```
