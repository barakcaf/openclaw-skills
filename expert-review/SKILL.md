---
name: expert-review
description: "Add expert engineering agents and domain rules to any project repository using .claude/rules/ and .claude/agents/ conventions. Creates 3 role-based agents (principal-engineer, security-engineer, ux-engineer) that can review, design, research, and implement. Use when: (1) setting up expert agents for a new or existing repo, (2) adding security/UX/architecture/infrastructure expertise, (3) bootstrapping .claude/rules/ and .claude/agents/ from proven templates, (4) configuring OpenClaw delegation for parallel sub-agent work. NOT for: running actual reviews (just ask), one-off code fixes, or repos without Claude Code or OpenClaw."
---

# Expert Team Setup

Scaffold role-based engineering agents and domain expertise rules into any repo.

## Outputs

### Agents → `<repo>/.claude/agents/`

Copied from [assets/agents/](assets/agents/). Each handles review, design, research, and implement tasks.

| Agent | Domain |
|-------|--------|
| [principal-engineer](assets/agents/principal-engineer.md) | Architecture, code quality, infra, system design |
| [security-engineer](assets/agents/security-engineer.md) | AppSec, IAM, OWASP, threat modeling, compliance |
| [ux-engineer](assets/agents/ux-engineer.md) | Accessibility, performance, design systems, i18n |

### Rules → `<repo>/.claude/rules/`

Adapted from [references/](references/). Auto-activate by file glob (`alwaysApply: false`).

| Template | Expertise |
|----------|-----------|
| [security.md](references/security.md) | AWS security, OWASP Top 10, LLM Top 10, STRIDE |
| [frontend.md](references/frontend.md) | WCAG 2.2, Core Web Vitals, Nielsen's heuristics |
| [code-quality.md](references/code-quality.md) | Architecture patterns, AWS limits, DDD, observability |
| [infrastructure.md](references/infrastructure.md) | CI/CD, deployment strategies, IaC, cost optimization |

## Workflow

### 1. Gather context

Ask for:
1. **Repo path** — local path or `owner/repo` to clone
2. **Stack** — frontend framework, backend language, IaC tool, CI/CD

### 2. Scan existing files

```bash
ls <repo>/CLAUDE.md <repo>/.claude/rules/ <repo>/.claude/agents/ 2>/dev/null
```

- `CLAUDE.md` exists → preserve, never overwrite
- `.claude/rules/` exists → offer **merge** (append missing expertise) or **replace**
- `.claude/agents/` exists → offer update or skip

### 3. Create/update rules

For each rule template in `references/`:

1. Read the template
2. Replace `## Project-Specific` section with rules matching the repo's actual stack
3. Adjust `globs` in frontmatter to match the repo's file structure
4. Keep all deep expertise sections unchanged — they're stack-agnostic
5. Write to `<repo>/.claude/rules/<name>.md`

**Merge mode** (rule file already exists): read existing, identify missing deep expertise sections, append under `## Deep Expertise`. Never remove existing project-specific content.

### 4. Create agents

Copy agent files from `assets/agents/` to `<repo>/.claude/agents/`. No adaptation needed — agents reference `.claude/rules/` by relative path.

### 5. OpenClaw delegation (optional, one-time)

If the user wants parallel sub-agent delegation via OpenClaw, append the section from [references/delegation.md](references/delegation.md) to `~/.openclaw/workspace/AGENTS.md`.

Check first: `grep -c "Expert Team Delegation" ~/.openclaw/workspace/AGENTS.md`

### 6. Commit

```bash
cd <repo>
git add .claude/rules/ .claude/agents/
git commit -m "chore: add expert engineering agents and domain rules"
git push
```

### 7. Report

```
✅ Expert team for <repo>

Rules (.claude/rules/ — auto-activate by glob):
  security.md        — [created|merged]
  frontend.md        — [created|merged]
  code-quality.md    — [created|merged]
  infrastructure.md  — [created|merged]

Agents (.claude/agents/ — invoke with /agent-name):
  principal-engineer.md  — review | design | research | implement
  security-engineer.md   — review | design | research | implement
  ux-engineer.md         — review | design | research | implement

OpenClaw delegation: [configured|already present|skipped]
```

Store setup config in `memory/skills/expert-review/<repo-name>.md`.
