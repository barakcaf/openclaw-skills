# claude-cert-prep

Adaptive exam prep for Anthropic's **Claude Certified Architect — Foundations (CCA-F)** — the first official Anthropic technical certification, launched March 12, 2026.

## What this skill does

When a user asks to prepare for the CCA-F exam, this skill runs them through a personalized study flow:

1. **Assesses** familiarity with a 15-question diagnostic across all 5 exam domains
2. **Routes** them to the right starting point based on score — from a zero-assumptions concept primer up to an exam-eve quick-fire
3. **Drills** weak domains with practice questions + explanations
4. **Remediates** the two historically hardest areas: Claude Code configuration and Claude Code for CI/CD

It assumes nothing. A user who's never heard of MCP can start here; a user reviewing the night before the exam can also start here.

## When it triggers

Phrases like:
- "help me study for CCA-F" / "prep for Claude cert" / "Anthropic certification"
- "drill me on agentic architecture / tool use / MCP / prompt engineering"
- "practice questions on Claude Code"
- "how do I register for the Claude exam"
- "where should I start with Claude cert"

## Exam at a glance

| | |
|---|---|
| Format | 60 multiple-choice, 120 min, online proctored |
| Passing | 720 / 1000 (scaled) |
| Fee | $99 (free for first 5,000 Claude Partner Network employees) |
| Registration | https://anthropic.skilljar.com |
| Prep courses | Free and open to everyone, no partner affiliation |
| Exam access | Requires Claude Partner Network membership (org joins free) |
| Badge validity | **6 months** (short — matches Claude Code/MCP release cadence) |

> Registration moved to Skilljar at the March 2026 launch. **Do NOT register at webassessor.com/anthropic** — that was the pre-launch Kryterion URL and is obsolete. See `references/registration-logistics.md` for the full walkthrough.

## The 5 exam domains

| # | Domain | Weight | Reference |
|---|--------|--------|-----------|
| 1 | Agentic Architecture & Orchestration | **27%** | `references/domain-1-agentic-architecture.md` |
| 2 | Tool Design & MCP Integration | 18% | `references/domain-2-tools-mcp.md` |
| 3 | Claude Code Configuration | 20% | `references/domain-3-claude-code.md` |
| 4 | Prompt Engineering for Production | 20% | `references/domain-4-prompt-engineering.md` |
| 5 | Context Management & Reliability | 15% | `references/domain-5-context-reliability.md` |

Agentic Architecture + Claude Code = **47% of the exam**. Weight your study accordingly.

## Progressive-disclosure design

The `SKILL.md` stays lean and orchestrates. Deep material lives in `references/` and loads only when a specific path is chosen:

```
claude-cert-prep/
├── SKILL.md                                     ← workflow + triggers (always loaded)
└── references/
    ├── registration-logistics.md                ← signup, partner network, badge lifecycle
    ├── baseline-assessment.md                   ← 15-question diagnostic (always first for new users)
    ├── concept-primer.md                        ← zero-assumptions intro (for beginners)
    ├── domain-1-agentic-architecture.md         ← hub/spoke, hooks, sessions, subagents
    ├── domain-2-tools-mcp.md                    ← tool_choice, MCP, built-in tools
    ├── domain-3-claude-code.md                  ← CLAUDE.md, skills, plan mode, permissions
    ├── domain-4-prompt-engineering.md           ← XML, structured output, chain-of-thought
    ├── domain-5-context-reliability.md          ← windows, summarization, failure modes
    ├── ci-cd-remediation.md                     ← deep dive on the hardest scenario (CI/CD)
    ├── claude-code-remediation.md               ← deep dive on Claude Code config edge cases
    └── quick-fire-concepts.md                   ← night-before / morning-of review
```

## Why this skill exists

CCA-F is new (March 2026), the industry prep market is thin, and the exam tests **architectural judgment** — not trivia. Most free prep material online covers 60–70% of the required ground and skips the hardest scenarios entirely (CI/CD session isolation, Claude Code context chunking, MCP vs built-in tool tradeoffs).

This skill was built from real practice-exam failure analysis — a 40/60 baseline followed by targeted drilling that closed the gap. The remediation references are weighted toward where people actually lose points, not where the study guide pretends they will.

## Caveats

- **Re-verify registration details** before acting on them. Anthropic has iterated registration and pricing at least once since launch. The live site at anthropic.skilljar.com wins over any file here.
- **Badge expires in 6 months.** Plan for recertification or the next tier when it launches.
- **This isn't a shortcut.** The exam is 301-level and assumes ~6 months of hands-on production experience. A drill skill can close 10–20 points of gap; it cannot substitute for actual building.

## Customization

To adapt this skill for a different Anthropic cert (e.g., a future Developer or Advanced Architect tier):

1. Copy the directory and rename
2. Update `SKILL.md` frontmatter `name` and `description`
3. Replace `baseline-assessment.md` questions with ones scoped to the new exam
4. Adjust domain files to match new weighting / scope
5. Update `registration-logistics.md` with the new exam's details

The workflow scaffolding (baseline → route → drill → remediate → quick-fire) is generic to scenario-based architecture exams.

## License

Custom skill — provided as-is.
