# Baseline Assessment — 15 Questions Across All 5 Domains

_Use this BEFORE assigning a learning path. Diagnostic only — designed to reveal which domains need foundation work vs exam-prep drilling._

## How to Administer

1. Ask all 15 questions in order. **Do not** reveal answers between questions.
2. For each question, accept the user's answer + ask them to rate their confidence: **guess / unsure / confident**.
3. At the end, score against the rubric below and recommend a personalized path.

## The Questions

_3 questions per domain, ordered easy → hard within each domain._

### Domain 1 — Agentic Architecture (27%)

**B1.** An "agentic loop" in Claude's SDK is best described as:

A) A pre-configured sequence of tools that Claude calls in order
B) A loop where Claude decides its next action, executes a tool, sees the result, and decides again — until it signals it's done
C) A way to avoid using tools by looping over text responses
D) A synonym for a for-loop in the Python SDK

<details><summary>Answer</summary>**B.** The loop is driven by Claude's `stop_reason` (tool_use → continue, end_turn → stop).</details>

**B2.** You have a coordinator agent that needs to delegate to 3 subagents in parallel. How?

A) The coordinator emits multiple Task tool calls in a single response
B) Each subagent calls the next one in sequence
C) You run 3 separate coordinators, one per subagent
D) Use `asyncio.gather()` in your Python code to run them concurrently

<details><summary>Answer</summary>**A.** Parallel = multiple Task calls in one response turn. Hub-and-spoke: subagents never call each other.</details>

**B3.** Your agent skips mandatory customer verification in 12% of cases, causing incorrect refunds. The most reliable fix is:

A) A stronger system prompt emphasizing verification is required
B) A programmatic hook that blocks `process_refund` until `get_customer` has returned a verified ID
C) Few-shot examples showing verification first
D) Increase the model size to reduce error rate

<details><summary>Answer</summary>**B.** Compliance requires deterministic hooks, not probabilistic prompts.</details>

---

### Domain 2 — Tool Design & MCP (18%)

**B4.** What is the PRIMARY mechanism Claude uses to decide which tool to call?

A) The order tools appear in the tools array
B) The tool's description field
C) Keywords the user types in their message
D) A built-in classifier model

<details><summary>Answer</summary>**B.** Tool descriptions drive selection. This is a foundational concept.</details>

**B5.** What does `tool_choice: "any"` do?

A) Lets Claude decide whether to call a tool or return text
B) Forces Claude to call a specific named tool
C) Forces Claude to call some tool (its choice) — guarantees structured output
D) Disables all tool use

<details><summary>Answer</summary>**C.** `"any"` = must call one. `"auto"` = may return text. `"required"` is not a valid option.</details>

**B6.** Your team's Jira MCP server is in `~/.claude.json`. New teammates don't have it. Why and fix?

A) `~/.claude.json` is user-scoped and not shared — move config to `.mcp.json` in the repo
B) The Jira server needs to be restarted
C) Teammates need to run `claude init`
D) MCP servers can only serve one user

<details><summary>Answer</summary>**A.** `.mcp.json` (project, version-controlled) for team tooling. `~/.claude.json` is personal.</details>

---

### Domain 3 — Claude Code Config (20%)

**B7.** A new hire reports Claude Code doesn't follow your team's standards. Standards are in `~/.claude/CLAUDE.md`. What happened?

A) The file is corrupted
B) User-level CLAUDE.md isn't shared via version control — move it to `.claude/CLAUDE.md` or root `CLAUDE.md`
C) They need to restart their terminal
D) CLAUDE.md files must be in JSON format

<details><summary>Answer</summary>**B.** Classic scoping trap. `~/.claude/CLAUDE.md` is personal-only.</details>

**B8.** You want testing conventions to apply to ALL test files regardless of where they live in the codebase (`tests/`, `**/*.spec.ts`, etc.). Best approach?

A) One big CLAUDE.md at the repo root
B) A CLAUDE.md in every directory that contains tests
C) `.claude/rules/testing.md` with `paths: ["**/*.test.*", "**/*.spec.*"]` in YAML frontmatter
D) A slash command `/test-conventions`

<details><summary>Answer</summary>**C.** Glob-scoped rule files apply across directories and only load when editing matching files.</details>

**B9.** Your CI pipeline runs `claude "Review this PR"` and hangs forever. Fix?

A) Add the `-p` flag: `claude -p "Review this PR"`
B) Set `CLAUDE_HEADLESS=true`
C) Redirect stdin from `/dev/null`
D) Add `--batch` flag

<details><summary>Answer</summary>**A.** `-p` (or `--print`) is the non-interactive flag for CI. The others don't exist.</details>

---

### Domain 4 — Prompt Engineering (20%)

**B10.** Your extraction tool requires a `phone_number` field. When documents don't have a phone number, Claude invents one. Fix?

A) Add "don't make up phone numbers" to the prompt
B) Make `phone_number` optional/nullable in the JSON schema so the model can return null
C) Post-validate and reject extractions with fake numbers
D) Use a larger model

<details><summary>Answer</summary>**B.** Required fields force fabrication. Optional/nullable lets the model honestly say "not present."</details>

**B11.** Your manager wants both the blocking pre-merge code review AND the overnight tech debt analysis moved to the Message Batches API for 50% savings. What do you recommend?

A) Move both — batch is usually fast enough
B) Move only the overnight analysis; keep pre-merge on real-time
C) Keep both on real-time
D) Move both with a timeout fallback

<details><summary>Answer</summary>**B.** Batch API has no latency SLA (up to 24h) — disqualifies blocking workflows.</details>

**B12.** Your PR review on 14 files gives inconsistent feedback — flags a pattern as bad in one file, approves identical code elsewhere. Root cause and fix?

A) Temperature too high — set temperature 0
B) Attention dilution from reviewing too many files at once — split into per-file passes + a cross-file integration pass
C) Model context window too small — use a larger model
D) Reviewer prompt too vague — add more rules

<details><summary>Answer</summary>**B.** Multi-pass review beats single-pass for large diffs. Attention dilution is the named failure mode.</details>

---

### Domain 5 — Context & Reliability (15%)

**B13.** In a long support conversation handling 3 issues, the agent gets the first and third right but confuses details of the second. What's happening?

A) The model is hallucinating randomly
B) "Lost in the middle" — models reliably process the beginning and end of long inputs but miss content in the middle
C) The context window overflowed
D) Temperature is too high

<details><summary>Answer</summary>**B.** Well-documented LLM failure mode. Fix: extract key facts into a persistent case-facts block at a consistent position.</details>

**B14.** When should your agent escalate to a human?

A) Whenever sentiment analysis detects frustration
B) Whenever the agent's self-reported confidence is low
C) When the customer explicitly asks for a human, when there's a policy gap, or when no meaningful progress is being made
D) After any tool failure

<details><summary>Answer</summary>**C.** Sentiment ≠ complexity. Self-reported confidence is poorly calibrated. Exam tests the WHEN criteria.</details>

**B15.** Your research system synthesizes a report claiming "AI market grew 45% in 2025" — but one source said 38%, another said 52%. What went wrong?

A) The synthesis agent averaged the two — add "don't average" to the prompt
B) Source attribution was lost during synthesis — require structured claim-source mappings preserved through every pass
C) One source is wrong — add a fact-checker subagent
D) The synthesis agent picked the wrong source

<details><summary>Answer</summary>**B.** Provenance collapse. Both values should have been preserved with sources: "A reports 38%, B reports 52%."</details>

---

## Scoring & Path Recommendations

### Score per domain (3 questions each)

| Domain | Questions | Your score |
|---|---|---|
| 1 — Agentic Architecture | B1, B2, B3 | __/3 |
| 2 — Tool Design & MCP | B4, B5, B6 | __/3 |
| 3 — Claude Code Config | B7, B8, B9 | __/3 |
| 4 — Prompt Engineering | B10, B11, B12 | __/3 |
| 5 — Context & Reliability | B13, B14, B15 | __/3 |

### Interpretation Rubric

Count **confident correct** answers (answered correctly AND flagged confidence ≥ "unsure").

**Per-domain score:**
- **3/3 confident correct** → You're solid. Light drill using the domain reference file's practice questions only.
- **2/3 correct** → Foundation is there. Read Core Concepts + Anti-Patterns, then drill.
- **0–1/3 correct, or any "guess" on a correct answer** → Foundation gap. Start from [concept-primer.md](concept-primer.md) for this domain, THEN move to the domain reference file.

**Overall interpretation:**

| Total confident correct | Recommended path |
|---|---|
| 13–15 | **Ready to register.** Run the full mock exam (Workflow E). If ≥ 72%, schedule the exam. |
| 9–12 | **Targeted prep.** Focus on domains scoring < 2/3. Read domain reference files fully, then drill. |
| 5–8 | **Structured study.** Read [concept-primer.md](concept-primer.md) end-to-end first, then work through all 5 domain files in weight order (1 → 3 → 4 → 2 → 5). |
| 0–4 | **Full foundation.** Start with Anthropic's official Claude Code docs + Agent SDK tutorial BEFORE this skill. Come back after 1–2 weeks of hands-on practice. This skill assumes working familiarity with Claude Code, tools, and the Agent SDK. |

### Personalized Report Template

After scoring, give the user this summary:

```
📊 CCA-F Baseline Assessment

Score: X/15 confident correct

Per-domain breakdown:
• Agentic Architecture (27% of exam):     X/3
• Tool Design & MCP (18%):                X/3
• Claude Code Config (20%):               X/3
• Prompt Engineering (20%):               X/3
• Context & Reliability (15%):            X/3

Weakest domains (prioritize):
1. [domain name]  — [concept-primer section or domain file]
2. [domain name]  — [concept-primer section or domain file]

Recommended path:
→ [Specific next step based on rubric above]

Estimated time to exam-ready: [based on score]
• 13–15: ~1–2 hrs (mock exam + targeted review)
• 9–12:  ~4–6 hrs over 2–3 sessions
• 5–8:   ~10–15 hrs over 1–2 weeks
• 0–4:   Come back after Anthropic docs + hands-on Claude Code practice
```

## Administration Notes

- **Don't reveal answers between questions** — batch-score at the end so later questions aren't biased by earlier feedback.
- **Probe "guess" answers** — if the user guesses correctly, treat it as wrong for path recommendation purposes. The goal is identifying true gaps, not measuring luck.
- **Respect "I don't know"** — if the user can't answer 3 in a row, pause and offer to route to the concept primer immediately instead of grinding through the remaining 12.
- **Time budget** — expect 5–10 minutes to run the full assessment.
