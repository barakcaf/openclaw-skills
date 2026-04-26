# Quick-Fire Concepts — Final Review

_Use the night before or morning of the exam. One-line recall on every concept. If you can't complete a bullet, open the matching domain file for a 5-minute refresher._

## Domain 1 — Agentic Architecture (27%)

- [ ] `stop_reason: "tool_use"` → continue loop. `"end_turn"` → stop.
- [ ] Subagents have **isolated** context — must pass findings explicitly.
- [ ] Parallel subagents = multiple Task tool calls in **one** coordinator response.
- [ ] Hooks = deterministic. Prompts = probabilistic.
- [ ] `PostToolUse` hooks transform results AFTER tool execution, BEFORE the model sees them.
- [ ] Hub-and-spoke only. Subagents never talk to each other.
- [ ] `fork_session` for divergent exploration from a shared baseline.
- [ ] When coverage is narrow but subagents succeeded → look at coordinator decomposition first.

## Domain 2 — Tool Design & MCP (18%)

- [ ] Tool descriptions are the **primary** mechanism for tool selection.
- [ ] 4–5 tools per agent. More = degraded selection.
- [ ] `tool_choice: "any"` = must call a tool. `"auto"` = might return text.
- [ ] `"required"` is NOT a valid `tool_choice` option.
- [ ] `.mcp.json` = project (shared). `~/.claude.json` = user (personal).
- [ ] Grep = content search. Glob = file name matching.
- [ ] Structured errors: `errorCategory`, `isRetryable`, `partialResults`, not generic messages.
- [ ] Transient = retry. Validation/Business/Permission = don't retry same input.
- [ ] Edit fails on non-unique anchors → fallback to Read + Write.

## Domain 3 — Claude Code Config (20%)

- [ ] `~/.claude/CLAUDE.md` = user only, NOT shared.
- [ ] `.claude/CLAUDE.md` or `./CLAUDE.md` = project, shared via git.
- [ ] `.claude/rules/` with `paths: ["glob"]` = conditional loading.
- [ ] `context: fork` in skills = isolated execution.
- [ ] `disable-model-invocation: true` = only user can trigger.
- [ ] Plan mode: architectural, 45+ files, unfamiliar. Direct: simple, clear.
- [ ] `@import` resolves relative to the importing file. Max 5 hops.
- [ ] Target under **200 lines** per CLAUDE.md.
- [ ] **`/memory` shows which memory files are loaded** (not file read history).
- [ ] **For automatic enforcement across scattered files → glob rules, NOT skills** (skills require manual invocation).
- [ ] `AGENTS.md` not read by Claude Code. Bridge: `CLAUDE.md` + `@AGENTS.md`.
- [ ] `-p` flag = non-interactive CI. **THE** answer (not CLAUDE_HEADLESS, not --batch).
- [ ] `--bare` = skip all auto-discovery (reproducible CI).
- [ ] `--output-format json` + `--json-schema` → structured output in `structured_output` field.
- [ ] `--continue` = most recent conversation. `--resume <id>` = specific. `--resume latest` doesn't exist.
- [ ] claude-code-action v1: `prompt` (not `direct_prompt`). `mode` removed (auto-detects @claude).

## Domain 4 — Prompt Engineering (20%)

- [ ] Few-shot examples > vague prose for ambiguous cases. 2–4 examples.
- [ ] Optional/nullable fields prevent hallucination of missing data.
- [ ] Enum + "other" + detail string for extensible categories.
- [ ] Validation retries work for format errors, NOT absent information.
- [ ] Batch API: 50% savings, up to 24hrs, NO multi-turn tools, NOT for blocking workflows.
- [ ] `custom_id` correlates Batch API results.
- [ ] Per-file + cross-file passes > single-pass for large reviews.
- [ ] Independent instance > self-review (context bias).
- [ ] Explicit criteria > "be conservative" / "be accurate" / "only important issues."
- [ ] High FP in one category destroys trust across all → temporarily disable.

## Domain 5 — Context & Reliability (15%)

- [ ] Lost-in-the-middle: put key facts at **beginning**, use headers.
- [ ] Extract transactional facts into persistent "case facts" block.
- [ ] Trim tool outputs to relevant fields (via hooks or prompt).
- [ ] Escalate: explicit customer ask, policy gap, no progress, human judgment required.
- [ ] DON'T escalate on: sentiment, self-reported confidence, "seems complex."
- [ ] Valid empty results ≠ errors. Distinguish them.
- [ ] Structured handoffs: customer ID + root cause + attempts + recommendation.
- [ ] Preserve claim-source mappings through synthesis. Annotate conflicts with sources.

---

## Exam Logistics

- Registration: https://anthropic.skilljar.com (NOT webassessor.com — that was pre-launch Kryterion)
- Exam requires Claude Partner Network access. Prep courses are free and open to all.
- Fee: $99 (free for first 5,000 partner employees)
- Format: 60 MCQ, 120 min, online proctored (webcam + ID)
- Passing score: **720/1000**
- **4 of 6 scenarios randomly selected** — every domain is fair game regardless of which scenarios land
- Badge validity: **6 months**. Plan for recertification.

## Final Mindset

1. **Read the question twice.** The exam loves traps where option A sounds right but is actually an anti-pattern.
2. **When two options look correct, pick the one that matches Anthropic's idiomatic pattern** (hub-and-spoke, structured errors, programmatic hooks for compliance).
3. **"Add to the prompt" is usually wrong** when the question is about reliability, consistency, or compliance. Hooks, schemas, and structured approaches beat prompt instructions.
4. **"Larger model" / "more context" is almost never the right answer.** The exam tests architectural decisions, not brute force.

Good luck. 🦊
