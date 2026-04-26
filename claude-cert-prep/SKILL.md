---
name: claude-cert-prep
description: Adaptive exam prep for Anthropic's Claude Certified Architect — Foundations (CCA-F). Use when a user asks to study for, prepare for, drill, or practice for the CCA-F exam, Claude certification, or Anthropic certification, or requests practice questions on Claude Code, Agent SDK, MCP, agentic architecture, tool design, prompt engineering, or context management. Triggers on phrases like "help me study for CCA-F", "practice Claude certification questions", "drill me on agentic architecture", "prep for Anthropic exam", "where should I start with Claude cert", "I'm new to Claude certification". Starts with a 15-question baseline assessment to personalize the learning path, then routes to a zero-assumptions concept primer, domain-level drills, remediation deep-dives (CI/CD and Claude Code — historically hardest), or exam-eve quick review accordingly. Covers all 5 exam domains — Agentic Architecture (27%), Tool Design & MCP (18%), Claude Code Config (20%), Prompt Engineering (20%), Context Management (15%).
---

# Claude Certification Prep (CCA-F)

Guide users through **personalized** preparation for Anthropic's Claude Certified Architect — Foundations exam. The skill adapts to the user's current knowledge level rather than assuming prior expertise.

## Exam At-A-Glance

- **Format:** 60 multiple-choice questions, 120 minutes, scenario-based. 4 of 6 production scenarios drawn randomly.
- **Passing score:** 720/1000 (scaled)
- **Fee:** $99 (free for first 5,000 Claude Partner Network employees)
- **Registration:** https://anthropic.skilljar.com (Anthropic Academy). **Prep courses are free and open to everyone; the exam itself requires Claude Partner Network access.**
- **Badge validity:** 6 months from pass date

For the full registration walkthrough — including how to join the Claude Partner Network and the common "I registered on Webassessor" mistake — load [references/registration-logistics.md](references/registration-logistics.md).

### Domain Weights

| # | Domain | Weight |
|---|--------|--------|
| 1 | Agentic Architecture & Orchestration | **27%** |
| 2 | Tool Design & MCP Integration | 18% |
| 3 | Claude Code Configuration & Workflows | 20% |
| 4 | Prompt Engineering & Structured Output | 20% |
| 5 | Context Management & Reliability | 15% |

Domain 1 is highest-weight — master it first once baseline is established.

## Skill File Map

| File | Purpose | When to load |
|------|---------|--------------|
| [references/registration-logistics.md](references/registration-logistics.md) | How to register, join the Claude Partner Network, exam format, badge validity, common confusions | Load when the user asks about registering, cost, partner network, exam logistics, retake policy, or badge expiration |
| [references/baseline-assessment.md](references/baseline-assessment.md) | 15-question diagnostic across all 5 domains | **ALWAYS load first** for any new user studying for the exam |
| [references/concept-primer.md](references/concept-primer.md) | Zero-assumption explanations of Claude Code, Agent SDK, MCP, tools, hooks, skills | Load when baseline score < 9/15 or user is unfamiliar with fundamentals |
| [references/domain-1-agentic-architecture.md](references/domain-1-agentic-architecture.md) | Agentic loops, subagents, hooks, sessions | After primer (if needed) or directly for users with baseline ≥ 9/15 |
| [references/domain-2-tools-mcp.md](references/domain-2-tools-mcp.md) | Tool design, MCP config, built-in tools, tool_choice | Same |
| [references/domain-3-claude-code.md](references/domain-3-claude-code.md) | CLAUDE.md, skills, plan mode, CI basics | Same |
| [references/domain-4-prompt-engineering.md](references/domain-4-prompt-engineering.md) | Few-shot, schemas, validation, batch API, multi-pass review | Same |
| [references/domain-5-context-reliability.md](references/domain-5-context-reliability.md) | Lost-in-middle, escalation, provenance | Same |
| [references/ci-cd-remediation.md](references/ci-cd-remediation.md) | 15 drill questions on Claude Code for CI/CD (hardest scenario) | User failed CI/CD practice, or requesting CI deep dive |
| [references/claude-code-remediation.md](references/claude-code-remediation.md) | 12 drill questions on CLAUDE.md/skills/plan-mode nuance | User failed Code Generation scenario, or wants deep Claude Code review |
| [references/quick-fire-concepts.md](references/quick-fire-concepts.md) | One-line recall list for exam eve | Last 24 hours before exam only |

**Progressive disclosure rule:** load only the file the current step needs. Never preload all references.

---

## Default Workflow (Unknown Baseline)

**This is the workflow for any user whose knowledge level you don't already know.** Do NOT assume they know what Claude Code, Agent SDK, MCP, or hooks are.

### Step 1 — Confirm intent and context
Ask these two questions first:
1. "Have you taken the CCA-F exam before, or scheduled it?"
2. "How familiar are you with Claude Code, the Agent SDK, and MCP — rate 1 (never used) to 5 (use daily)?"

If self-reported familiarity is ≤ 2, load [references/concept-primer.md](references/concept-primer.md) FIRST and walk through it before any assessment. The assessment questions assume working familiarity with the vocabulary.

If self-reported familiarity is ≥ 3, proceed directly to Step 2.

### Step 2 — Run the baseline assessment
Load [references/baseline-assessment.md](references/baseline-assessment.md).

Administer all 15 questions (3 per domain) with these rules:
- **Ask one at a time.** Do NOT show multiple questions at once.
- **Do NOT reveal the answer** until ALL 15 are answered. Batch-score at the end to avoid biasing later answers.
- For each question, also ask confidence: **guess / unsure / confident**.
- If the user can't answer 3 in a row or says "I don't know" repeatedly, pause and offer to route them straight to the concept primer. Don't grind through the remaining questions.

### Step 3 — Score and personalize
Score the 15 answers. Count **confident correct** (correct answers where confidence was ≥ "unsure" — guesses don't count as knowledge even if correct).

Use this routing rubric:

| Total confident correct | Route the user to |
|---|---|
| **13–15** | Workflow E (full mock exam). If they pass ≥72%, recommend scheduling. |
| **9–12** | Workflow B (targeted drill) on domains that scored < 2/3. Read domain Core Concepts first, then practice. |
| **5–8** | Workflow F (structured study). Full concept-primer read, then all 5 domains in weight order: 1 → 3 → 4 → 2 → 5. |
| **0–4** | Pause. Recommend the user spend 1–2 weeks with Anthropic's official docs and hands-on Claude Code practice before returning. This skill assumes baseline familiarity. |

Present the score as a table (see [baseline-assessment.md](references/baseline-assessment.md) "Personalized Report Template").

### Step 4 — Execute the recommended workflow
See the workflow sections below. Never skip straight to a domain file without assessment or explicit user override.

---

## Secondary Workflows

### Workflow A — User explicitly wants to skip assessment
If the user says "I don't want to be quizzed, just start with X" or "I know my weak area already," honor that. Go to their chosen domain reference file or remediation file directly. But flag once: "Heads up — the domain reference files assume baseline familiarity with Claude Code, Agent SDK, MCP, tools, and hooks. If any of these terms feel unfamiliar, say so and I'll pull up the concept primer first."

### Workflow B — Targeted Domain Drill
Used when the user scored 2/3 on a domain, or says "drill me on domain X."

1. Load the matching `references/domain-N-*.md` file.
2. Walk the user through Core Concepts + Anti-Patterns (5 min).
3. Run the Practice Questions. Don't auto-reveal answers — wait for user commit.
4. Score at the end. Recap each miss with the underlying concept, not just the letter.

### Workflow C — CI/CD or Claude Code Remediation
Used when Domain 3 score is low, user mentions CI pipeline issues, or requests deep Claude Code review.

1. Load the matching remediation file.
2. Walk through Deep Dive sections first — these contain exam-level nuance the domain files don't.
3. Then drill on the 12–15 scenario questions.

### Workflow D — Exam Eve / Morning Review
Used when user says "exam tomorrow" or "final review."

1. Load ONLY [references/quick-fire-concepts.md](references/quick-fire-concepts.md).
2. Read each bullet; have the user complete or explain it.
3. Flag any bullet they stumble on — offer a 5-min refresher from the matching domain file.
4. Remind them: Webassessor, 720/1000 to pass, 4 of 6 scenarios random.

### Workflow E — Full Mock Exam
Used when user wants to gauge readiness.

1. Sample practice questions weighted by exam percentages:
   - ~5 from Domain 1 (27%)
   - ~4 from Domain 3 (20%)
   - ~4 from Domain 4 (20%)
   - ~4 from Domain 2 (18%)
   - ~3 from Domain 5 (15%)
2. Randomize order. Do NOT reveal answers until all are answered.
3. Score at the end. Map wrong answers to their domain.
4. ≥72% = pass-likely. < 72% = remediate weakest domains.

### Workflow F — Structured Foundation Study
Used when baseline is 5–8/15 (foundation gaps across multiple domains).

1. Load [references/concept-primer.md](references/concept-primer.md). Read it end-to-end with the user (~20 min).
2. Re-run the 3 questions for the weakest domain to confirm the primer helped.
3. Proceed through all 5 domain files in weight order (1 → 3 → 4 → 2 → 5). For each:
   - Read Core Concepts + Anti-Patterns together
   - Run the practice questions
   - Recap misses
4. End with Workflow E (mock exam) once all 5 are done.

---

## Rules When Running Practice Questions

- **Never reveal the answer before the user commits.** `<details>` blocks in reference files must stay collapsed until the user answers.
- **Ask for confidence** on each question, not just the answer.
- **Treat guessed-correct as gaps.** Rubric tracks confident-correct only.
- **Explain misses with the concept**, not just the letter. Reference the Core Concepts section of the domain file.
- **Distinguish exam-correct from real-world pragmatism.** Some exam answers follow idiomatic Anthropic patterns that may differ from what a user does in production. Call this out when relevant.
- **Anti-patterns are heavily tested.** When a user asks "why not option A?" always connect to the anti-pattern list.

## Domain Loading Heuristics (after baseline)

Only load the reference file the user actively needs. Do not preload all five.

- "agentic loop", "stop_reason", "subagents", "coordinator", "hooks" → Domain 1
- "MCP", "tool description", "tool_choice", "structured errors", "Grep/Glob/Read" → Domain 2
- "CLAUDE.md", "skills", "plan mode", ".claude/rules", "slash commands" → Domain 3
- "few-shot", "JSON schema", "validation", "batch API", "multi-pass review" → Domain 4
- "lost in the middle", "escalation", "provenance", "context window" → Domain 5
- "CI", "pipeline", "`-p` flag", "claude-code-action", "github actions" → CI/CD remediation
- "exam tomorrow", "final check", "quick review" → quick-fire only
- "never used Claude Code", "what is MCP", "new to this" → concept primer FIRST
- "how do I register", "how much does it cost", "partner network", "where do I sign up", "is the badge still valid" → registration-logistics FIRST

## Exam-Day Logistics

Full walkthrough lives in [references/registration-logistics.md](references/registration-logistics.md). Quick summary:

- Register at **https://anthropic.skilljar.com** (NOT webassessor.com — that was the pre-launch URL)
- Requires Claude Partner Network affiliation to book the exam (your employer joins for free; you can take prep courses free without it)
- Bring government ID. Webcam + quiet room required for remote proctoring.
- No notes, no docs, no Claude access during the exam. 2 min/question average.
- Scenarios randomize — 4 of 6 selected. Every domain is fair game regardless of which scenarios land.
