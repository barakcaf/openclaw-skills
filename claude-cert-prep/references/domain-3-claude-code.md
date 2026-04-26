# Domain 3 — Claude Code Configuration & Workflows (20%)

## Table of Contents
- [3.1 CLAUDE.md Hierarchy](#31-claudemd-hierarchy)
- [3.2 Custom Commands and Skills](#32-custom-commands-and-skills)
- [3.3 Plan Mode vs Direct Execution](#33-plan-mode-vs-direct-execution)
- [3.4 CI/CD Integration](#34-cicd-integration)

> For deep dives on CLAUDE.md + skills edge cases, see [claude-code-remediation.md](claude-code-remediation.md).
> For CI/CD nuance, see [ci-cd-remediation.md](ci-cd-remediation.md).

---

## 3.1 CLAUDE.md Hierarchy

### The Layers (broadest → narrowest)

```
~/.claude/CLAUDE.md          → User-level (personal, NOT shared)
.claude/CLAUDE.md             → Project-level (shared via git)
  or root CLAUDE.md
subdirectory/CLAUDE.md        → Directory-level (applies in that dir)
.claude/rules/*.md            → Rule files with glob-pattern scoping
```

### Key Rules
- **User-level** (`~/.claude/CLAUDE.md`): only for YOU, NOT version controlled, teammates don't get them
- **Project-level** (`.claude/CLAUDE.md` or root `CLAUDE.md`): shared, version controlled, team standards
- **`@import`** syntax: reference external files for modularity
- **`.claude/rules/`**: topic-specific rule files, alternative to monolithic CLAUDE.md

### .claude/rules/ with Glob Patterns

```yaml
---
paths:
  - "src/api/**/*"
---
# API Conventions
Always use async/await with try/catch...
```

Rules only load when editing matching files → saves tokens.

**When to use rules/ vs directory CLAUDE.md:**
- Rules with globs: conventions for files **spread across the codebase** (e.g., all `**/*.test.tsx`)
- Directory CLAUDE.md: conventions for everything **in one directory**

### Practice Questions

**Q3.1:** Test files are co-located with source throughout the codebase (e.g., `Button.test.tsx` next to `Button.tsx`). Want all tests to follow same conventions. Most maintainable approach?

A) `.claude/rules/testing.md` with `paths: ["**/*.test.*"]` in YAML frontmatter
B) Root CLAUDE.md "Testing" header
C) CLAUDE.md in every directory containing tests
D) Skill in `.claude/skills/` for test generation

<details><summary>Answer</summary>
**A.** Glob-pattern rules apply regardless of location. B relies on Claude inferring context. C doesn't scale. D requires manual invocation.
</details>

**Q3.2:** New engineer reports Claude Code doesn't follow team's coding standards. Standards are in `~/.claude/CLAUDE.md`. Fix?

A) Have the new engineer copy the file to their home directory
B) Move to `.claude/CLAUDE.md` (project-level) so it's version controlled
C) Add to `.claude/rules/standards.md`
D) Both B and C

<details><summary>Answer</summary>
**D.** Both project-level CLAUDE.md and rules/ files are version controlled. The root cause is user-level config isn't shared. Either option moves to project scope.
</details>

---

## 3.2 Custom Commands and Skills

### Commands

| Scope | Location | Shared? |
|---|---|---|
| Project | `.claude/commands/` | ✅ Via git |
| User | `~/.claude/commands/` | ❌ Personal |

### Skills (`.claude/skills/`)

Skills use SKILL.md with frontmatter:

```yaml
---
context: fork          # Run in isolated sub-agent context
allowed-tools:         # Restrict tool access
  - Read
  - Write
argument-hint: "Provide the file path to analyze"
---
# My Skill
Instructions here...
```

- **`context: fork`** — isolates skill output from main conversation. Use for: verbose output (codebase analysis), exploratory work
- **`allowed-tools`** — restrict what tools the skill can use. Use to prevent destructive actions
- **`argument-hint`** — prompts user for required parameters

### Skills vs CLAUDE.md
- **CLAUDE.md** — always loaded. Universal standards.
- **Skills** — on-demand invocation. Task-specific workflows.

### Practice Questions

**Q3.3:** You're creating a skill that analyzes an entire codebase and produces a verbose report. Running inline would fill most of the context window. Which frontmatter option?

A) `allowed-tools: [Read, Grep, Glob]`
B) `context: fork`
C) `argument-hint: "Path to codebase"`
D) `max-tokens: 5000`

<details><summary>Answer</summary>
**B.** `context: fork` runs the skill in an isolated sub-agent context. Only the final result returns to main conversation.
</details>

---

## 3.3 Plan Mode vs Direct Execution

| Use Plan Mode When | Use Direct Execution When |
|---|---|
| Large-scale changes (45+ files) | Single-file bug fix |
| Multiple valid approaches | Clear, well-scoped change |
| Architectural decisions | Adding a validation check |
| Unfamiliar codebase | Known codebase, known fix |
| Library migrations | Simple config changes |

### The Explore Subagent
Use for isolating verbose discovery output. Returns summaries to preserve main context.

### Practice Questions

**Q3.4:** You're assigned to add a date validation check to a single form field. The fix is straightforward. Which approach?

A) Plan mode to explore all form validation patterns first
B) Direct execution — change is simple and well-scoped
C) Create a skill with `context: fork` for the fix
D) Use Explore subagent to understand the validation layer

<details><summary>Answer</summary>
**B.** Simple, well-scoped, single-file → direct execution. Plan mode adds unnecessary overhead.
</details>

---

## 3.4 CI/CD Integration

> Shallow overview here. Full remediation in [ci-cd-remediation.md](ci-cd-remediation.md).

### Key Flags
- **`-p` (or `--print`)** — non-interactive mode. Required for CI. Without it, Claude Code hangs.
- **`--output-format json`** — machine-parseable output
- **`--json-schema`** — enforce structured output schema

### Important CI Patterns
- Include prior review findings when re-running → report only **new or unaddressed** issues
- Provide existing test files in context → avoid duplicate tests
- Document testing standards in CLAUDE.md → improves test generation
- **Session isolation:** a separate Claude instance reviews code better than the one that generated it (self-review bias)

### Practice Questions

**Q3.5:** Your CI pipeline runs `claude "Review this PR"` but the job hangs indefinitely. What's wrong?

A) Missing `-p` flag for non-interactive mode
B) Set `CLAUDE_HEADLESS=true`
C) Redirect stdin from `/dev/null`
D) Use `--batch` flag

<details><summary>Answer</summary>
**A.** `-p` is the documented non-interactive flag. B, C, D don't exist or don't address Claude Code's syntax.
</details>

---

## Domain 3 Recap

- CLAUDE.md files **concatenate**, not override.
- `~/.claude/CLAUDE.md` = user only, NOT shared. Team standards → project-level.
- `.claude/rules/` with `paths:` globs = conditional loading, saves tokens.
- `context: fork` in skills = isolated execution, preserves main context.
- Plan mode: architectural decisions, 45+ files, unfamiliar code. Direct: simple, clear scope.
- `-p` flag = non-interactive mode for CI.
- Self-review is biased → separate independent instance reviews better.
