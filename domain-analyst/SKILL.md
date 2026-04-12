---
name: domain-analyst
description: "Design and scaffold recurring domain-specific research reports with checkpointed pipelines, evolving source registries, scoring transparency, and generation statistics. Use when: (1) creating a new recurring report for any domain (AI, security, markets, health, etc.), (2) improving an existing domain report's methodology or pipeline, (3) adding a new domain to the report infrastructure. Triggers: 'create a report', 'new domain report', 'set up recurring analysis', 'build a weekly/daily report', 'domain analyst'. NOT for: one-off research tasks, writing a single report without recurring infrastructure, or modifying cron schedules directly."
---

# Domain Analyst — Recurring Report Builder

Build recurring domain research reports through a guided conversation. The goal is a complete report infrastructure: methodology, sources, checkpointed pipeline, cron job, and delivery.

## Philosophy

Every report is different. A stock analysis needs real-time data feeds. A GenAI report needs benchmark cross-referencing. A cybersecurity report needs CVE databases. Guide the user through decisions and let the domain shape the output.

What IS universal: the pipeline pattern (research → score → synthesize), source evolution, checkpoint persistence for evals, generation stats, and quality gates.

## Workflow

Walk through these phases conversationally. Don't dump all questions at once — adapt based on answers. Skip phases that are already defined.

### Phase 1: Purpose & Persona

Define the report's purpose and the agent persona that writes it.

**Ask about:**
- What domain does this report cover?
- Who is the audience? (CTO peers, personal briefing, team, public)
- What's the report's unique angle? What makes it worth reading vs. just searching the web?
- What voice should the report have? (sharp analyst, academic, casual, opinionated strategist)

**Then help define:**
- A persona name and 2-3 paragraph background (like "The Engineering Operator" or "The AI Economist")
- What the persona filters FOR (what makes something signal)
- What the persona filters OUT (what gets auto-discarded)
- Anti-patterns specific to this domain (e.g., "vendor self-reported benchmarks without independent verification")

### Phase 2: Domain Topics & Themes

Define what the report tracks.

**Ask about:**
- What are the 5-10 top-level themes this report should monitor?
- Which themes are always-include vs. include-when-newsworthy?
- Are there specific entities to track? (companies, models, regulations, people)
- What makes something "signal" vs. "noise" in this domain?

**Output:** A numbered themes list. Each theme gets a 1-line description of what to look for.

### Phase 3: Source Deep Dive

Build the initial source registry.

**Guide the user through:**
1. **Tier 1 sources** (must-check every run) — who are the authoritative voices? Primary data producers? Practitioners with skin in the game?
2. **Tier 2 sources** (check on signal) — secondary voices, good-but-not-essential, emerging analysts
3. **Tier 3 sources** (quarterly/major releases) — annual surveys, big reports, academic papers

**For each source, capture:**
- Name and URL
- RSS feed if available
- Tavily search query that finds their content
- Why this source matters (what unique signal do they provide?)

**Output:** A `sources.md` file following the tiered registry pattern. Load `references/SOURCES-TEMPLATE.md` for the registry structure.

### Phase 4: Research Pipeline

Design the checkpointed research steps. This varies by domain. Load `references/PIPELINE-PATTERNS.md` to select the right pattern (research-heavy weekly, data-driven daily, or hybrid).

**The universal pattern:**
1. One or more **research steps** that gather raw data (searches, API calls, data fetches)
2. A **scoring step** that evaluates every result against a rubric
3. A **synthesis step** that drafts the report from scored results

**For each research step, define:**
- Step name and description
- What queries/data sources it hits
- What raw data it produces

**For the scoring step, define rubric dimensions.** Load `references/SCORING-EXAMPLES.md` for examples from existing reports. Common ones:
- Novelty (0-3): New vs. previously covered?
- Evidence quality (0-3): Primary data > opinion > speculation
- Relevance (0-3): Direct theme hit > tangential

But let the domain customize. A stock report might use Relevance/Evidence/Timeliness. A security report might use Severity/Exploitability/Relevance. Ask the user what dimensions matter for scoring.

**Key principle: every result gets scored individually with a decision (include/exclude) and reasoning.** This is the eval chain — raw data → scoring decisions → final report. No black box.

**Also define:**
- Include threshold (default ≥5 on 9-point scale)
- Checkpoint format — each step saves JSON to S3 with timestamps, query counts, raw results
- Checkpoints are KEPT after delivery — they're the foundation for future evals

### Phase 5: Report Template

Design the report output format.

**Ask about:**
- What sections should the report have? (Lead signal? Patterns? Tensions? Forward-looking?)
- Word count range?
- Must-have sections vs. omit-if-quiet sections?
- Any specific formatting rules? (no tables for Telegram, links required, etc.)

**Always include:**
- A **dedup review pass** between draft and quality gate:
  - One home per story — each development gets ONE primary section; others reference in ≤1 sentence
  - Cross-section entity scan — anything appearing in 3+ sections gets consolidated to the strongest section
  - Bullet dedup — same point, different framing → keep the stronger, kill the other
  - Lead signal rule — if a story is the lead, other sections add new angles only, no restatement
  - If word count drops below minimum after dedup, expand remaining stories with deeper analysis — don't re-inflate with redundancy
- A quality gate with minimum requirements and a scoring rubric (0-10)
- A `📊 Generation Stats` section:
  - Sources consulted / cited / scored (included vs excluded)
  - Per-step research timing
  - Token usage + cost estimate
  - Model used for generation
- A `📚 Sources` section with linked citations
- Continuity protocol (load last N reports, track evolving narratives, don't repeat leads)

### Phase 6: Delivery & Schedule

**Ask about:**
- Frequency? (daily, weekly, biweekly, monthly)
- If weekly — which day and why? (e.g., Monday catches weekend drops, Friday summarizes the week)
- Timezone?
- Delivery channels:
  - Telegram (dual: file attachment + inline message)
  - S3 bucket (for archival + eval access)
  - Other? (email, Slack, Discord)
- Quiet day policy? (if insufficient signal, say so rather than pad)

**Suggest:** Dual Telegram delivery (file + inline) is the current standard. S3 is mandatory for checkpoint/eval infrastructure.

### Phase 7: Scaffold & Wire

Once all phases are complete, generate:

1. **METHODOLOGY.md** — the full methodology file for the report, including persona, themes, research pipeline, scoring rubric, report template, quality gate, and run checklist
2. **sources.md** — the initial source registry
3. **S3 bucket** — create if needed (pattern: `{domain}-reports-{account_id}`)
4. **Cron job** — register with OpenClaw cron, including the full agent prompt

**For the cron prompt, follow these principles:**
- Tell the agent to read METHODOLOGY.md and sources.md at the start
- Include the checkpointed pipeline with S3 paths
- Include the scoring step explicitly
- Include Generation Stats requirements
- Include dual delivery instructions
- Set appropriate timeout (900s for weekly research reports, 600s for daily)
- Use Sonnet 4.6 as default model for cost efficiency

**File locations:**
- Methodology: `projects/{domain}-report/METHODOLOGY.md`
- Sources: `projects/{domain}-report/sources.md`
- S3: `{domain}-reports-{account_id}/reports/`, `{domain}-reports-{account_id}/checkpoints/`
- Config memory: `memory/skills/domain-analyst/{domain}.md` — store domain config summary for future sessions (persona, themes, S3 bucket, cron ID, delivery settings)
