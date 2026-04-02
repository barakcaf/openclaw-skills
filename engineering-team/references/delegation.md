## Engineering Team Delegation

The principal-engineer is the **tech lead and orchestrator**. Route all engineering tasks through it — it decides whether to handle directly, delegate to a specialist, or run experts in parallel.

### Default pattern: route through principal-engineer

For any engineering task (review, design, research, implement), spawn the principal-engineer. It will triage and delegate as needed:

```
sessions_spawn: "You are the principal engineer (tech lead). <task description>.
  Read <repo>/CLAUDE.md and <repo>/.claude/agents/principal-engineer.md for your role.
  Read relevant .claude/rules/ files.
  Route to specialists if needed: /security-engineer, /ux-engineer."
```

### When to spawn directly (bypass orchestrator)

Only when the domain is obvious and narrow:

- **Pure security task** (IAM audit, OWASP review, threat model) → spawn security-engineer directly
- **Pure frontend/UX task** (accessibility audit, Core Web Vitals, UI review) → spawn ux-engineer directly

### Parallel review pattern (complex PRs)

For large or full-stack PRs, spawn all three in parallel for speed:

```
sessions_spawn: "You are the principal engineer. Review PR #N in <repo>.
  Read <repo>/CLAUDE.md, .claude/rules/code-quality.md, .claude/rules/infrastructure.md.
  Focus: architecture, API contracts, cost, AWS patterns. Synthesize a unified verdict."

sessions_spawn: "You are a security engineer. Review PR #N in <repo>.
  Read <repo>/CLAUDE.md and .claude/rules/security.md.
  Classify: [CRITICAL]/[HIGH]/[MEDIUM]/[LOW].
  Each finding: Issue → Impact → Blast Radius → Remediation → Detection."

sessions_spawn: "You are a UX engineer. Review frontend changes in PR #N in <repo>.
  Read <repo>/CLAUDE.md and .claude/rules/frontend.md.
  WCAG violations with criterion numbers, Core Web Vitals risks, before/after code."
```

Synthesize all results into a unified review.

### Design pattern

```
sessions_spawn: "You are the principal engineer. Design <feature> for <repo>.
  Read <repo>/CLAUDE.md and relevant .claude/rules/ files.
  Present 2-3 options with tradeoff matrix (cost, complexity, scalability).
  Recommend one. Include component diagram and API contracts.
  Spawn /security-engineer for threat model if the feature touches auth or data."
```

### Research pattern

```
sessions_spawn: "You are the principal engineer. Compare <option A> vs <option B> for <use case>.
  Read <repo>/CLAUDE.md for project context.
  Evaluation criteria: cost, performance, operational complexity, limits.
  Structured comparison matrix + opinionated recommendation."
```
