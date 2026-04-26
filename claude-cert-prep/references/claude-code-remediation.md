# Claude Code Remediation — Code Generation with Claude Code

_Common second-weakest scenario. Deep dive into CLAUDE.md hierarchy, skills, plan mode, iterative refinement — exam-level nuance._

## Table of Contents
- [Deep Dive: CLAUDE.md Hierarchy](#deep-dive-claudemd-hierarchy)
- [Deep Dive: Skills System](#deep-dive-skills-system)
- [Deep Dive: Plan Mode](#deep-dive-plan-mode)
- [Deep Dive: Iterative Refinement](#deep-dive-iterative-refinement)
- [12 Practice Questions](#12-practice-questions)

---

## Deep Dive: CLAUDE.md Hierarchy

### Full Resolution Order
```
Managed policy (/Library/.../CLAUDE.md)     → Org-wide (IT-deployed)
  ↓
Project (.claude/CLAUDE.md or ./CLAUDE.md)   → Team (git-tracked)
  ↓
User (~/.claude/CLAUDE.md)                   → Personal (NOT shared)
  ↓
Local (./CLAUDE.local.md)                    → Personal + project (gitignored)
  ↓
Directory (subdir/CLAUDE.md)                 → Loads on-demand when entering dir
```

### Critical Details
- Files **concatenate**, they don't override. If project says "use tabs" and user says "use spaces", both are in context — Claude resolves the conflict (last-read wins practically).
- `CLAUDE.local.md` — personal project-specific prefs, goes in `.gitignore`. NOT shared.
- Subdirectory CLAUDE.md files load **on demand** (when Claude reads files in that dir), not at startup.
- `@import` — loads referenced files at launch. Relative paths resolve from the importing file, not CWD. Max 5 hops deep.
- `AGENTS.md` — Claude Code reads CLAUDE.md, NOT AGENTS.md. Bridge: `CLAUDE.md` with `@AGENTS.md` import.
- HTML comments (`<!-- -->`) are stripped before injection — use for human-only notes that don't consume tokens.
- Target **under 200 lines** per CLAUDE.md file. Longer = more tokens + reduced adherence.

### .claude/rules/ Deep Dive

```yaml
---
paths:
  - "src/api/**/*"        # Only when editing API files
---
# API conventions
Use async/await with try/catch...
```

- Rules WITHOUT `paths` frontmatter → load at startup (same as CLAUDE.md)
- Rules WITH `paths` → load only when editing matching files (saves tokens)
- Glob patterns work across the entire codebase — better than directory CLAUDE.md for cross-cutting concerns

### `claudeMdExcludes`
In large monorepos, other teams' CLAUDE.md files get picked up. Use `claudeMdExcludes` in settings to skip them.

## Deep Dive: Skills System

### Skills = Commands + More
- `.claude/commands/deploy.md` and `.claude/skills/deploy/SKILL.md` both create `/deploy`
- If both exist, **skill takes precedence**
- Skills support a directory with supporting files (templates, examples, scripts)

### Frontmatter Options

```yaml
---
name: my-skill                     # Becomes /my-skill. Default: directory name
description: "What it does"        # Helps Claude decide when to auto-invoke
context: fork                      # Run in isolated subagent context
allowed-tools: Read Grep Glob      # Restrict tools (space-separated)
argument-hint: "File path"         # Prompt user for args
disable-model-invocation: true     # ONLY user-invoked, Claude can't auto-trigger
---
```

### `context: fork` — When and Why
- Fork = runs in an **isolated sub-agent context** with its own window
- Main conversation only gets the **final result**, not verbose intermediate output
- Use for: codebase analysis, brainstorming, any verbose/exploratory work
- Without fork: skill output dumps into main conversation context → fills it up

### `disable-model-invocation: true`
- Prevents Claude from auto-triggering the skill
- Only runs when YOU type `/skill-name`
- Use for: destructive operations (deploy), expensive operations, things that should be deliberate

## Deep Dive: Plan Mode

### When Plan Mode Adds Value

| Task | Plan Mode? | Why |
|---|---|---|
| Monolith → microservices restructuring | ✅ YES | Multiple valid approaches, architectural decisions |
| Library migration (45+ files) | ✅ YES | Large scale, many approaches |
| Choosing between integration approaches | ✅ YES | Different infrastructure implications |
| Single-file bug fix with clear stack trace | ❌ NO | Well-scoped |
| Adding a date validation check | ❌ NO | Simple, one function |
| Config change | ❌ NO | Known location, known change |

### The Explore Subagent
- Built-in, runs on **Haiku** (fast, cheap)
- **Read-only** (no Write/Edit tools)
- Isolates verbose discovery output from main context
- Claude delegates to Explore for search/understand without making changes
- Thoroughness levels: quick, medium, very thorough

### Plan Mode + Direct Execution Combo
Best pattern for complex work:
1. **Plan mode** → investigate, understand dependencies, design approach
2. **Direct execution** → implement the planned approach

## Deep Dive: Iterative Refinement

### The 4 Techniques

**1. Input/Output Examples (most effective for inconsistent formatting)**
When Claude interprets your description differently each time:
- Provide 2–3 concrete before/after examples
- Shows exact transformation you want
- Claude generalizes from examples to novel cases

**2. Test-Driven Iteration**
```
Write test suite → Run → Share failures → Claude fixes → Run again → Repeat
```
- Tests define success criteria
- Failures give specific actionable feedback
- Edge cases covered upfront

**3. Interview Pattern**
Have Claude **ask questions before implementing**:
- Surfaces considerations you didn't think of
- Especially valuable in unfamiliar domains
- e.g., "What cache invalidation strategy?" "What failure modes?"

**4. Single vs Sequential Messages**
- Multiple **interacting** issues → send all in **one** detailed message
- **Independent** issues → fix them **sequentially** (one at a time)

---

## 12 Practice Questions

**Q-CC-1:** New team member can't get Claude Code to follow coding standards. Everyone else has them. Standards are in `~/.claude/CLAUDE.md`. What happened?

A) File corrupted
B) User-level CLAUDE.md isn't shared via version control — move to project-level
C) New member needs to run `/init`
D) CLAUDE.md needs to be in `.claude/CLAUDE.md` format specifically

<details><summary>Answer</summary>
**B.** `~/.claude/CLAUDE.md` is user-scoped, NOT version controlled. Team standards must be in `./CLAUDE.md` or `./.claude/CLAUDE.md`.
</details>

**Q-CC-2:** Project has React components (functional + hooks), API handlers (async/await + error handling), DB models (repository pattern), and test files spread throughout directories. Most maintainable way to apply different conventions?

A) One big CLAUDE.md with sections for each area
B) `.claude/rules/` files with glob patterns: `paths: ["src/api/**/*"]`, `paths: ["**/*.test.*"]`, etc.
C) CLAUDE.md in every subdirectory
D) Skills for each code type

<details><summary>Answer</summary>
**B.** Rules with glob patterns apply based on file paths regardless of location — essential for tests spread across directories. A relies on inference. D requires manual invocation.
</details>

**Q-CC-3:** Creating a codebase analysis skill producing a 2000-line architecture report. Running inline fills most of the context window. Which frontmatter?

A) `allowed-tools: Read Grep Glob`
B) `context: fork`
C) `disable-model-invocation: true`
D) `argument-hint: "Path to analyze"`

<details><summary>Answer</summary>
**B.** `context: fork` runs in an isolated sub-agent context. Only final result returns to main conversation.
</details>

**Q-CC-4:** Want Claude to use codebase analysis skill only when explicitly invoked with `/analyze`, never automatically. Which frontmatter?

A) `context: fork`
B) `allowed-tools: none`
C) `disable-model-invocation: true`
D) `argument-hint: "required"`

<details><summary>Answer</summary>
**C.** `disable-model-invocation: true` prevents auto-triggering. Only user invocation works.
</details>

**Q-CC-5:** Restructuring a monolith into microservices. Which approach?

A) Direct execution — start making changes
B) Plan mode — explore dependencies and design approach first
C) Create a skill for the migration
D) Direct execution with comprehensive upfront instructions

<details><summary>Answer</summary>
**B.** Complex architectural decisions with multiple valid approaches = plan mode. Safe exploration before committing. A risks costly rework.
</details>

**Q-CC-6:** During a long exploration session, Claude gives inconsistent answers, referencing "typical patterns" instead of specific classes it found earlier. What's happening and what do you do?

A) Model hallucinating — restart
B) Context degradation in extended sessions — use `/compact` to reduce or spawn subagents for exploration
C) Context window full — increase max tokens
D) Model needs better instructions

<details><summary>Answer</summary>
**B.** Extended sessions → context degradation. `/compact` reduces context. Subagents (like Explore) isolate verbose discovery. Scratchpad files persist findings.
</details>

**Q-CC-7:** Have `AGENTS.md` in your repo but Claude Code ignores it. Fix?

A) Rename to CLAUDE.md
B) Create a CLAUDE.md that imports it: `@AGENTS.md`
C) Set `agents_md_support: true` in config
D) Claude Code should read it automatically

<details><summary>Answer</summary>
**B.** Claude Code reads CLAUDE.md, not AGENTS.md. Bridge with CLAUDE.md + `@AGENTS.md` import. Both tools then read the same instructions.
</details>

**Q-CC-8:** Claude describes formatting requirements inconsistently — sometimes tabs, sometimes spaces, sometimes mixed. Detailed prose doesn't help. Most effective technique?

A) Add "IMPORTANT: use spaces" in bold
B) Provide 2–3 concrete input/output examples showing exact formatting
C) Linting step that reformats after Claude
D) Switch models

<details><summary>Answer</summary>
**B.** Input/output examples are most effective when prose produces inconsistent results. Show the exact transformation.
</details>

**Q-CC-9:** Building a feature in unfamiliar domain (payment processing). Want to surface considerations you might miss before implementing. Which technique?

A) Just implement and iterate on bugs
B) Interview pattern — have Claude ask questions to surface design considerations before implementing
C) Read all documentation first
D) Copy patterns from similar project

<details><summary>Answer</summary>
**B.** Interview pattern surfaces considerations (cache invalidation, failure modes, compliance) BEFORE implementation.
</details>

**Q-CC-10:** CLAUDE.md has grown to 400 lines. Claude follows some instructions but ignores others. What should you do?

A) Add "FOLLOW ALL INSTRUCTIONS" at the top
B) Split into focused `.claude/rules/` files and use `@import` — target under 200 lines per file
C) Multiple CLAUDE.md files in the same directory
D) Convert to JSON

<details><summary>Answer</summary>
**B.** Target under 200 lines per CLAUDE.md. Longer files consume more tokens and reduce adherence. Split into `.claude/rules/` topic files and/or `@import`.
</details>

**Q-CC-11:** You want to use the `/memory` command. What does it show?

A) List of all files Claude has read in this session
B) Which memory files (CLAUDE.md, rules, auto memory) are currently loaded
C) Model's internal token count
D) History of past sessions

<details><summary>Answer</summary>
**B.** `/memory` shows loaded memory files — useful for diagnosing inconsistent behavior (e.g., checking if a rule file is picked up).
</details>

**Q-CC-12:** Test files live in `tests/` AND `*.spec.ts` files alongside source code. Want one set of testing conventions for ALL tests regardless of location. Which approach?

A) CLAUDE.md in `tests/` directory
B) `.claude/rules/testing.md` with `paths: ["tests/**/*", "**/*.spec.ts"]`
C) Two CLAUDE.md files, one in `tests/` and one at root
D) Skill `/test-conventions`

<details><summary>Answer</summary>
**B.** Glob patterns in `.claude/rules/` match files across multiple locations. A only covers `tests/`. D requires manual invocation.
</details>

---

## Claude Code Quick-Fire Memorize List

- CLAUDE.md files **concatenate**, not override.
- `~/.claude/CLAUDE.md` = user only, NOT shared, NOT version controlled.
- `CLAUDE.local.md` = personal + project-specific, add to `.gitignore`.
- Target **under 200 lines** per CLAUDE.md.
- `@import` resolves relative to the **importing file**, not CWD. Max 5 hops.
- `.claude/rules/` with `paths:` glob = conditional loading = saves tokens.
- Skills with `context: fork` = isolated execution, preserves main context.
- `disable-model-invocation: true` = only user can trigger.
- Plan mode: architectural decisions, multiple approaches, 45+ files.
- Direct execution: single-file, clear scope, known fix.
- Explore subagent: Haiku, read-only, isolates discovery.
- `/compact` for context reduction in long sessions.
- `/memory` to check loaded memory files.
- `AGENTS.md` → Claude Code doesn't read it. Import via `@AGENTS.md` in CLAUDE.md.
- Input/output examples > prose for inconsistent formatting.
- Interview pattern: Claude asks questions before implementing (unfamiliar domains).
- Test-driven iteration: write tests first, iterate on failures.
