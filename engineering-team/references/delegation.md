## Engineering Team Delegation

Spawn domain experts as sub-agents for review, design, research, or implementation tasks.

### When to spawn vs handle directly
- **Spawn** for: multi-file PR reviews, architecture design, security audits, research comparisons, complex implementations
- **Handle directly** for: quick questions, single-file fixes, config changes, status checks

### How to spawn
Use `sessions_spawn` with a detailed task. Include:
1. The **role** (principal-engineer / security-engineer / ux-engineer)
2. The **task type** (review / design / research / implement)
3. The repo path
4. Which `.claude/rules/` files to read first
5. For security tasks: also read SLATS-SKILL-GLOBAL.md and SLATS-SKILL-INTERNAL.md
6. Specific deliverables expected

### Parallel review pattern
For comprehensive PR reviews, spawn focused experts:

```
sessions_spawn: "You are a security engineer. Review PR #N in <repo>.
  Read <repo>/CLAUDE.md and <repo>/.claude/rules/security.md.
  Read SLATS-SKILL-GLOBAL.md and SLATS-SKILL-INTERNAL.md from workspace.
  Classify: [CRITICAL]/[HIGH]/[MEDIUM]/[LOW].
  Each finding: Issue → Impact → Blast Radius → Remediation → Detection."

sessions_spawn: "You are a principal engineer. Review PR #N in <repo>.
  Read <repo>/CLAUDE.md, <repo>/.claude/rules/code-quality.md,
  and <repo>/.claude/rules/infrastructure.md.
  Top 3 critical issues, cost efficiency score (1-10), highest-leverage refactor."

sessions_spawn: "You are a UX engineer. Review frontend changes in PR #N in <repo>.
  Read <repo>/CLAUDE.md and <repo>/.claude/rules/frontend.md.
  WCAG violations with criterion numbers, Core Web Vitals risks, before/after code."
```

Synthesize all results into a unified review.

### Design pattern
```
sessions_spawn: "You are a principal engineer. Design <feature> for <repo>.
  Read <repo>/CLAUDE.md and relevant .claude/rules/ files.
  Present 2-3 options with tradeoff matrix (cost, complexity, scalability).
  Recommend one with justification. Include component diagram and API contracts."
```

### Research pattern
```
sessions_spawn: "You are a principal engineer. Compare <option A> vs <option B> for <use case>.
  Read <repo>/CLAUDE.md for project context.
  Evaluation criteria: cost, performance, operational complexity, limits.
  Structured comparison matrix + opinionated recommendation."
```
