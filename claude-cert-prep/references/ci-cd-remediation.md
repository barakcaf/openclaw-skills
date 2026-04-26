# CI/CD Remediation — Claude Code for CI/CD

_Historically the hardest exam scenario (often scored <50% on first practice exam). Master everything here before retaking._

## Table of Contents
- [The CI/CD Mental Model](#the-cicd-mental-model)
- [Running Claude Code Non-Interactively](#running-claude-code-non-interactively)
- [Structured Output for CI](#structured-output-for-ci)
- [Auto-Approving Tools in CI](#auto-approving-tools-in-ci)
- [System Prompt Customization](#system-prompt-customization)
- [Continuing Conversations in CI](#continuing-conversations-in-ci)
- [Patterns the Exam Tests](#patterns-the-exam-tests)
- [Claude Code GitHub Actions](#claude-code-github-actions)
- [15 Practice Questions](#15-practice-questions)

---

## The CI/CD Mental Model

Claude Code in CI/CD is NOT just "run Claude in a pipeline." The exam tests nuanced decisions about:
- How to run it non-interactively
- How to get structured output for automated processing
- How to handle re-reviews after new commits
- Session isolation between code generation and review
- What CLAUDE.md provides in CI context
- When batch API is appropriate vs real-time

## Running Claude Code Non-Interactively

The **`-p` (or `--print`) flag** is THE answer:
```bash
claude -p "Review this PR for security issues"
```

Without `-p`, Claude Code opens an interactive session and **hangs** waiting for terminal input.

**`--bare` mode** goes further — skips all auto-discovery:
```bash
claude --bare -p "Analyze this code" --allowedTools "Read"
```
- No hooks, skills, plugins, MCP servers, auto memory, CLAUDE.md loaded
- Only explicit flags take effect
- Fastest startup, most reproducible across machines
- **Recommended for scripted/CI calls**

## Structured Output for CI

```bash
# JSON output with metadata
claude -p "Review this PR" --output-format json

# JSON conforming to a schema
claude -p "Extract function names" \
  --output-format json \
  --json-schema '{"type":"object","properties":{"functions":{"type":"array","items":{"type":"string"}}}}'
```

- `--output-format json` → response in `result` field + session_id, metadata
- `--output-format json` + `--json-schema` → structured data in `structured_output` field
- `--output-format stream-json` → newline-delimited JSON for real-time streaming

## Auto-Approving Tools in CI

```bash
claude -p "Fix test failures" --allowedTools "Bash,Read,Edit"
```

Permission modes:
- `--permission-mode dontAsk` → denies anything not in `permissions.allow` (locked-down CI)
- `--permission-mode acceptEdits` → allows file writes, but shell/network still need explicit allow

## System Prompt Customization

```bash
# APPEND to default system prompt (keeps Claude Code's defaults)
gh pr diff "$1" | claude -p \
  --append-system-prompt "You are a security engineer. Review for vulnerabilities." \
  --output-format json

# REPLACE entire system prompt (rare, specialized use)
claude -p --system-prompt "Custom prompt here"
```

## Continuing Conversations in CI

```bash
# First pass
claude -p "Review this codebase"

# Continue same conversation
claude -p "Now focus on database queries" --continue

# Resume specific session by ID
session_id=$(claude -p "Start review" --output-format json | jq -r '.session_id')
claude -p "Continue" --resume "$session_id"
```

## Patterns the Exam Tests

### Pattern 1: Session Isolation for Review
The model that generated code is **biased** when reviewing its own output. It retains reasoning context and is less likely to question its decisions.

- ✅ Correct: separate, independent Claude instance reviews generated code
- ❌ Wrong: same session generates code then reviews it

### Pattern 2: Re-Review After New Commits
When a PR gets new commits and you re-run review:
- **Include prior review findings** in context
- Instruct Claude to report **only new or still-unaddressed** issues
- Prevents duplicate comments flooding the PR

### Pattern 3: Test Generation Context
- **Provide existing test files** in context → avoids duplicates
- **Document testing standards in CLAUDE.md** → frameworks, fixtures, valuable test criteria

### Pattern 4: Prompt Chaining for Large Reviews
1. Per-file local analysis
2. Cross-file integration pass (data flow, API contracts, consistency)

Prevents attention dilution and contradictory findings.

### Pattern 5: Iterative Refinement Techniques
- **Input/output examples** — most effective when prose produces inconsistent results (2–3 before/after examples)
- **Test-driven iteration** — write tests first, iterate by sharing failures
- **Interview pattern** — have Claude ask questions before implementing (surfaces considerations)
- **Single vs sequential fix messages** — multiple interacting issues → one detailed message; independent issues → sequential

## Claude Code GitHub Actions

```yaml
# Basic workflow with @claude mentions
- uses: anthropics/claude-code-action@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}

# With custom prompt and CLI args
- uses: anthropics/claude-code-action@v1
  with:
    prompt: "Review this PR for security issues"
    claude_args: |
      --append-system-prompt "Follow our coding standards"
      --max-turns 10
      --model claude-sonnet-4-6
```

### v1.0 Changes (exam tests these)
- `direct_prompt` → **`prompt`** (old name silently ignored!)
- `mode` → **removed** (auto-detected)
- `custom_instructions` → **`claude_args: --append-system-prompt`**
- `max_turns` → **`claude_args: --max-turns`**

---

## 15 Practice Questions

**Q-CI-1:** CI runs `claude "Analyze this PR"` and hangs. Fix?

A) Add `-p`: `claude -p "Analyze this PR"`
B) Set `CLAUDE_HEADLESS=true`
C) Redirect stdin: `< /dev/null`
D) Add `--batch` flag

<details><summary>Answer</summary>
**A.** `-p` is the only correct non-interactive flag. B, C, D reference mechanisms that don't exist.
</details>

**Q-CI-2:** Want CI review output as machine-parseable JSON for inline PR comments. Which flags?

A) `--output-format text` and parse with regex
B) `--output-format json` with `--json-schema` defining findings structure
C) `--output-format xml`
D) `--format structured`

<details><summary>Answer</summary>
**B.** `--output-format json` + `--json-schema` produces structured output in `structured_output`. A is fragile. C and D don't exist.
</details>

**Q-CI-3:** Code review CI job needs reproducibility across machines. Developers have different MCP servers and CLAUDE.md configs locally. How?

A) Delete all local configs before running
B) Use `--bare` to skip all auto-discovery, pass only explicit flags
C) Run in Docker container
D) Use `--no-config` flag

<details><summary>Answer</summary>
**B.** `--bare` skips hooks, skills, plugins, MCP servers, auto memory, CLAUDE.md. Only explicit flags take effect. D doesn't exist. C works but is over-engineered.
</details>

**Q-CI-4:** CI review posts comments on PRs. After developers push fixes, re-running produces duplicates for already-addressed issues. Fix?

A) Delete all previous comments before each run
B) Include prior findings in context, instruct Claude to report only new/unaddressed issues
C) Track commented line numbers, skip already-commented
D) Run review only once per PR, on final commit

<details><summary>Answer</summary>
**B.** Exam-correct approach. A loses history. C is fragile (line numbers shift). D defeats iterative review.
</details>

**Q-CI-5:** Claude generates code in CI, then the same pipeline step reviews it. Review rarely finds issues, but humans find bugs. Why?

A) CI model too conservative
B) Review prompt not specific enough
C) Same Claude session retains reasoning context from generation, making it biased against questioning its own decisions
D) Review needs more tool access

<details><summary>Answer</summary>
**C.** Self-review limitation. Solution: separate independent Claude instance for review.
</details>

**Q-CI-6:** GitHub Actions workflow triggers on PR comments mentioning `@claude`. Using claude-code-action v1, correct config?

A) `mode: "tag"` with `direct_prompt: "Review this PR"`
B) Just set `anthropic_api_key` — @claude mention detection is automatic in v1
C) `mode: "agent"` with `override_prompt: "Review"`
D) Set `trigger: "@claude"` in workflow config

<details><summary>Answer</summary>
**B.** In v1.0, mode is auto-detected and removed. Action auto-detects @claude mentions when no `prompt` is set. A, C use deprecated beta params. D doesn't exist.
</details>

**Q-CI-7:** Team generates tests with Claude in CI. Output duplicates existing coverage. How to improve?

A) `--max-turns 3` to limit output
B) Provide existing test files in context + document testing standards/fixtures in CLAUDE.md
C) Switch models
D) Add "only generate unique tests" to prompt

<details><summary>Answer</summary>
**B.** Existing tests in context prevents duplicates. CLAUDE.md documents standards and fixtures. A limits turns not quality. D is vague.
</details>

**Q-CI-8:** PR modifies 14 files. Single-pass review gives inconsistent depth and contradictory findings. Restructure?

A) Split into per-file analysis passes, then a separate cross-file integration pass
B) Require smaller PRs
C) Larger context model
D) Three independent passes, only flag consensus issues

<details><summary>Answer</summary>
**A.** Prompt chaining: per-file for local issues + cross-file for integration. B shifts burden. C doesn't solve attention quality. D suppresses valid findings.
</details>

**Q-CI-9:** Auto-approve Read and Bash in CI without prompting, block Write and Edit. Which approach?

A) `--allowedTools "Read,Bash"` with `--permission-mode dontAsk`
B) `--allowedTools "Read,Bash,Write,Edit"`
C) `--permission-mode acceptEdits`
D) `--allowedTools "Read,Bash"` (without setting permission mode)

<details><summary>Answer</summary>
**A.** `--allowedTools "Read,Bash"` auto-approves those. `--permission-mode dontAsk` denies anything not explicitly allowed. D might hang if Claude tries to edit.
</details>

**Q-CI-10:** Manager wants to move both pre-merge code review (blocking) and overnight technical debt analysis to Batch API for 50% savings. Recommendation?

A) Move both — batch is usually fast enough
B) Move only overnight analysis; keep real-time for pre-merge
C) Keep both real-time
D) Move both with timeout fallback

<details><summary>Answer</summary>
**B.** Batch API has no latency SLA (up to 24hr). Blocking pre-merge can't tolerate that. Overnight is perfect.
</details>

**Q-CI-11:** Claude Code in CI suggests a refactoring approach. You want to iterate: first have it explain the plan, then implement. How?

A) Send both instructions in one prompt
B) Use `--continue` to continue the conversation after the first response
C) Two separate sessions
D) `--resume latest` flag

<details><summary>Answer</summary>
**B.** `--continue` continues the most recent conversation in the current directory. Preserves planning context into implementation. D isn't correct syntax.
</details>

**Q-CI-12:** CI produces review findings as JSON. Claude sometimes returns conversational text instead of calling the extraction tool. Which `tool_choice` guarantees structured output?

A) `"auto"` — default
B) `"any"` — must call a tool
C) `"none"` — disable tools
D) `"required"` — same as "any"

<details><summary>Answer</summary>
**B.** `"any"` guarantees a tool call. `"auto"` lets it return text. `"none"` disables tools. `"required"` is not a valid Claude API option.
</details>

**Q-CI-13:** CI review flags a pattern as "problematic" in one file but approves identical code in another file in the same PR. What's happening?

A) Model non-determinism — run at temperature 0
B) Attention dilution from reviewing too many files in a single pass
C) CLAUDE.md has contradictory rules
D) Context window too small

<details><summary>Answer</summary>
**B.** Attention dilution across many files causes inconsistent findings. Fix: multi-pass (per-file + cross-file).
</details>

**Q-CI-14:** Configuring `claude-code-action@v1` to pass custom instructions + limit turns. Beta used `custom_instructions` and `max_turns`. v1 equivalent?

A) Same parameters — backward compatible
B) `claude_args: "--append-system-prompt 'Custom instructions' --max-turns 10"`
C) `prompt: "Custom instructions"` and `max_turns: 10`
D) `settings: {"instructions": "Custom", "maxTurns": 10}`

<details><summary>Answer</summary>
**B.** In v1.0: `custom_instructions` → `claude_args: --append-system-prompt`, `max_turns` → `claude_args: --max-turns`. A is wrong — beta params removed/renamed.
</details>

**Q-CI-15:** Process 500 documents overnight, have results by morning. Each doc needs single extraction pass (no multi-turn tool calling). Most cost-effective?

A) Sequential real-time API calls
B) Message Batches API with `custom_id` per document
C) Batches API with multi-turn tool calling for validation
D) Parallel real-time API calls

<details><summary>Answer</summary>
**B.** Batch API: 50% savings, up to 24hr, `custom_id` correlates results. No multi-turn needed. C is wrong — Batch API doesn't support multi-turn tool calling. A and D cost 2x.
</details>

---

## CI/CD Quick-Fire Memorize List

- `-p` flag = non-interactive mode. **THE** answer for CI.
- `--bare` = skip all auto-discovery. Fastest, most reproducible.
- `--output-format json` + `--json-schema` = structured CI output.
- `--allowedTools "Read,Bash"` = auto-approve specific tools.
- `--permission-mode dontAsk` = deny everything not explicitly allowed.
- `--continue` = continue most recent conversation. `--resume <id>` = specific session.
- `--append-system-prompt` = ADD to default. `--system-prompt` = REPLACE.
- Self-review is biased → use separate independent instance.
- Re-review: include prior findings → report only new/unaddressed.
- Provide existing tests in context → prevents duplicate test generation.
- Batch API: 50% savings, up to 24hrs, NO multi-turn tools, NOT for blocking workflows.
- claude-code-action v1: `prompt` not `direct_prompt`, `mode` removed (auto-detect).
- Per-file + cross-file passes > single-pass for large reviews.
