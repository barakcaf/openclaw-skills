---
name: agent-docs-auditor
description: "Audit and improve agent instruction files — both repo steering files (CLAUDE.md, AGENTS.md, .claude/rules/, .github/prompts/) and skill files (SKILL.md). Two modes: (1) Repo steering — audit against coding best practices. (2) Skill audit — audit against the AgentSkills spec for structure, progressive disclosure, and quality. Triggers: 'update steering docs', 'audit steering', 'review project rules', 'improve guidelines', 'audit skill', 'review skill', 'check skill quality'. NOT for: one-off code reviews, editing individual source files, non-steering config files (package.json, tsconfig, etc.)."
---

# Agent Docs Auditor

Two modes: **repo steering audit** and **skill audit**. Detect which from context, or ask.

## Mode Detection

- User mentions CLAUDE.md, steering docs, project rules, guidelines → **Repo steering mode**
- User mentions SKILL.md, skill quality, skill audit, or points to a skill directory → **Skill audit mode**
- Ambiguous → ask: "Are you auditing repo steering files or a skill?"

---

## Repo Steering Mode

Audit and improve project steering files against curated best practices.

### First Run (per repo)

On first invocation, check memory for repo config. If not found, ask:

1. **Repo** — GitHub owner/repo?
2. **Stack** — Primary languages/frameworks?
3. **Agent tools in use** — Claude Code, Codex, Cursor, other?
4. **Existing steering files** — or let me scan and discover them?

Store in `memory/skills/agent-docs-auditor/<repo-name>.md`.

### Step 1: Check Freshness

Read `references/recommendations.md` and check "Last researched" date.

- **>14 days stale** → run Deep Research before continuing.
- **User explicitly asks to refresh** → run Deep Research regardless.
- Otherwise → Step 2.

### Step 2: Discover Steering Files

Scan for steering files:

| File | Scope | Audience |
|------|-------|----------|
| CLAUDE.md / .claude/rules/ | Code rules, testing, commit conventions | Coding agents |
| AGENTS.md | Workspace rules, safety, operating boundaries | All agents |
| WORKFLOW.md | Dev process, PR flow, merge gates, notifications | Orchestrator agent |
| .cursor/rules/ | Cursor-specific rules | Cursor |
| .github/prompts/ | AI review and auto-fix instructions | GitHub Actions AI |

A rule lives in exactly one file — cross-reference, don't duplicate.

### Step 3: Audit

For each recommendation in `references/recommendations.md`:
1. Check whether a matching rule exists.
2. If it exists — verify it's specific, correctly placed, and not duplicated.
3. If missing — flag as a gap.

Categorize: **Gaps**, **Conflicts**, **Redundancies**, **Structural issues** (verbosity, files over 200 lines).

### Step 4: Propose & Apply

1. Present specific edits to the user.
2. Iterate until approved.
3. Apply via branch + PR:
   - Branch: `docs/steering-update-YYYY-MM-DD`
   - Scan for duplicates across ALL steering files before committing.
   - Commit: `docs: update steering — [brief summary]`

---

## Skill Audit Mode

Audit SKILL.md files against the AgentSkills spec and quality recommendations.

### Target

Accept a skill directory path, or scan workspace `skills/` for custom skills to audit.

### Step 1: Check Freshness

Read `references/skill-recommendations.md` and check "Last researched" date. If >14 days stale or user asks to refresh, run Deep Research (skills) first.

### Step 2: Audit

Read the SKILL.md and all reference/script files. Score against:
- [references/skill-checklist.md](references/skill-checklist.md) — structural requirements
- [references/skill-recommendations.md](references/skill-recommendations.md) — best practice recommendations

Score each item: ✅ pass, ⚠️ warning, ❌ fail. Present findings grouped by category with specific fix suggestions.

### Step 3: Apply

Apply fixes after user approval.

---

## Deep Research

Refresh recommendation files from external sources. Runs automatically when stale (>14 days), or on user request.

| Mode | Target file | Sources |
|------|------------|---------|
| Repo steering | `references/recommendations.md` | Anthropic docs, community blogs, CI/CD best practices, project history |
| Skills | `references/skill-recommendations.md` + `references/skill-checklist.md` | AgentSkills spec, Anthropic skills docs, OpenClaw docs, tool ecosystems (Cursor/Gemini/OpenHands/Goose), ClawHub |

### Process

1. Research across the sources for the active mode.
2. Require at least 2 independent sources per new recommendation.
3. Update the target file(s) — add new findings, update existing, remove stale advice, update "Last researched" date.
4. Present changes to user — show what was added/updated/removed.
5. Wait for user approval before continuing to the audit.
