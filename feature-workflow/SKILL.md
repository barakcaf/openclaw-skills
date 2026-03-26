---
name: feature-workflow
description: "End-to-end workflow for implementing features and bug fixes in a project repo. Use when: (1) user asks to build, add, implement, or create a feature, (2) user asks to fix a bug, (3) user says 'new feature', 'implement X', 'build X', 'add X to the app', 'fix bug X'. Handles the full lifecycle: complexity assessment, research, design doc, GitHub issue, branching, coding agent spawn, PR creation, review handling, and merge. NOT for simple one-line edits (just edit directly), config changes, docs-only updates, or infrastructure changes."
---

# Feature Workflow

Assess → research → design → issue → branch → implement → PR → review → merge.

## First Run (per repo)

On first invocation against a repo, check memory for its config. If not found, ask:

1. **Repo** — GitHub owner/repo?
2. **Coding agent** — What tool? (Claude Code, Codex, other)
3. **CI** — Provider? (GitHub Actions, CodePipeline, CircleCI, none)
4. **AI code review** — Set up? What severity levels block merge?
5. **Notifications** — Where to send updates? (Telegram, Slack, Discord, none)
6. **Security constraints** — Any security rules file to check? (path or none)
7. **Merge strategy** — Squash, merge, or rebase?

Store answers in `memory/repos/<repo-name>.md`. Read this file at the start of every subsequent run.

On first run, also verify branch protection on the default branch:
```bash
gh api repos/OWNER/REPO/branches/main/protection --jq '.required_pull_request_reviews, .required_status_checks' 2>/dev/null
```
If not configured, warn: "Branch protection is not enabled. Direct pushes to main are possible. Recommend enabling required PR reviews and status checks."

## Step 1: Assess Complexity

Evaluate what's being asked:

**Simple** (single file/component, obvious implementation):
- Skip Steps 2-3, go directly to Step 4.
- Examples: button label change, env var addition, copy fix.

**Complex** (multiple approaches, unclear scope, architectural decisions):
- Proceed to Step 2.
- Ask clarifying questions if too vague to even research.

Complexity signals: multiple components affected, new patterns, vague request, security/data model implications, external service integration.

## Step 2: Research (Opus)

Investigate before designing:

1. Read relevant source files — understand current architecture and patterns.
2. Identify affected components, shared code, downstream impacts.
3. Check for prior art — similar features, related solutions.
4. Fetch external docs if third-party APIs/libraries are involved.
5. If a security constraints file is configured, check it for relevant rules.

Output: understanding sufficient to write a design doc. Do NOT implement.

## Step 3: Design Doc (Opus)

Produce a design document using the template in [references/design-doc-template.md](references/design-doc-template.md). Present it to the user for approval.

> "Here's the design for <feature>. Approve to proceed, or flag anything to change?"

- **Approved** → Continue to Step 4.
- **Changes requested** → Revise and re-present.
- **Rejected** → Log decision in daily notes. Stop.

For simple features that skipped this step, the issue body serves as the lightweight spec.

## Step 4: Open GitHub Issue

Create a GitHub issue:
- Title: `feat: <description>` or `fix: <description>`
- Labels: `enhancement` or `bug`
- Body: description, scope, approved design (inline or summary), acceptance criteria, technical notes.

For complex features, include the full approved design doc in the body.

## Step 5: Confirm with User

> "Issue #N opened. Start working on it now, or save for later?"

- **Later** → Log as TODO. Stop.
- **Now** → Continue to Step 6.

## Step 6: Create Branch and Implement

Branch naming per configured prefixes (default: `feat/issue-N-short-desc`, `fix/issue-N-short-desc`).

Create a worktree and spawn the configured coding agent. The task prompt must include:
- Issue number and full description.
- The approved design doc (Step 3), or the issue body for simple features.
- Files to modify (from research).
- Testing requirements.
- Security requirements: "Do not introduce hardcoded secrets, public resources, or overly permissive IAM/security rules. If a security constraints file is configured, verify changes comply."
- Commit format: `feat: description (#N)` or `fix: description (#N)`.
- Push the branch when done. Do NOT open a PR.

## Step 7: Open PR

After the agent pushes, open the PR:
- Title matches commit format.
- Body: problem/motivation, what changed, `Closes #N`, reviewer notes.

## Step 8: Handle Review

Do NOT poll for CI. Wait for notification (via configured channel) or user.

When CI completes:
1. Check status: `gh pr view N --json statusCheckRollup`
2. If AI code review is configured: check for findings at configured blocking severities.
3. **All green** → Step 9.
4. **Findings need fixing** → re-spawn coding agent on the same branch with findings.
5. **Tests failed** → investigate, re-spawn if code fix needed.

If no AI code review is configured, rely on CI status and manual review.

## Step 9: Merge

Verify: all CI green/skipped, no unresolved blocking findings (if AI review is configured).

Merge using the configured strategy (default: squash, delete branch).

If not up to date with base, update branch first and wait for CI re-run.

Notify the user when merged.

## Model Selection

The core principle: **invest Opus in research and design (Steps 2-3) to eliminate ambiguity, then execute cheaply with Sonnet downstream.**

| Step | Default | Use Opus when... |
|------|---------|-----------------|
| 1. Assess | Sonnet | Deeply ambiguous requirements |
| 2. Research | **Opus** | Always |
| 3. Design doc | **Opus** | Always |
| 4. Create issue | Sonnet | — |
| 5. Confirm | — | — |
| 6. Implement | Sonnet | Design doc is lightweight/skipped, open questions remain, or security-sensitive code |
| 7. Open PR | Sonnet | — |
| 8. Handle review | Sonnet | Ambiguous findings that challenge the design |
| 9. Merge | Sonnet | — |

**Implementation model depends on design doc quality.** If the design specifies exact files, clear interfaces, concrete acceptance criteria, and no open questions → Sonnet. If ambiguity remains → Opus.

### Cost guardrails
- Default to Sonnet unless there's a clear reason to upgrade.
- Never use Opus for mechanical steps (issue creation, PR, merge).
- One Opus call that gets it right > three Sonnet retries.

## Rules

- **NEVER** push directly to main — always branch + PR.
- **NEVER** merge with unresolved blocking findings — fix first.
- **NEVER** skip the issue step — every change gets tracked.
- **NEVER** use `--admin` to bypass merge gates.
- Read the repo's steering docs before spawning agents.
