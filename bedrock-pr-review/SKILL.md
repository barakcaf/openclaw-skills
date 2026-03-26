---
name: bedrock-pr-review
description: "Set up AI-powered code review and auto-fix for GitHub repositories using Amazon Bedrock. Creates GitHub Actions workflows for: (1) PR review via Bedrock Claude models with inline comments, severity ratings, and auto-resolve on re-push, (2) automated fix agent via claude-code-action that addresses review findings. Use when: user asks to 'add AI code review', 'set up PR review', 'add auto-fix to a repo', 'set up Bedrock review', or 'add CI review workflow'. Requires: GitHub repo, AWS account with Bedrock access, OIDC federation. NOT for: manual one-off code reviews (just review the code), non-GitHub repos, non-AWS setups."
---

# Bedrock PR Review & Auto-Fix

Set up automated AI code review and auto-fix workflows for GitHub repos using Amazon Bedrock.

## References

Load these as needed during setup:

| File | When to read |
|------|-------------|
| [references/oidc-setup.md](references/oidc-setup.md) | Step 1a — setting up OIDC federation |
| [references/github-app-setup.md](references/github-app-setup.md) | Step 1b — creating or validating a GitHub App |
| [references/review-prompt-template.md](references/review-prompt-template.md) | Step 2 — generating the review prompt |
| [references/workflow-templates.md](references/workflow-templates.md) | Step 5 — deploying workflow files |

## First Run (per repo)

On first invocation, check memory for repo config. If not found, ask the **required** questions:

1. **Repo** — GitHub owner/repo?
2. **Tech stack** — Languages, frameworks, key conventions?
3. **Test commands** — How to run tests per component?

Then present **optional** settings with defaults — accept all or customize:

| Setting | Default |
|---------|---------|
| AWS region | `us-east-1` |
| Review model | `us.anthropic.claude-opus-4-6-v1` |
| Fix model | `sonnet` |
| Max fix cycles | `2` |
| Max fix turns | `25` |
| Max files per review | `15` |
| Max changed lines | `1500` |
| Max output tokens | `16384` |
| Notification channel | `none` (Telegram/Slack/WhatsApp) |
| Blocking severities | `CRITICAL, HIGH` |

GitHub App name and bot ID are collected during Step 1b.

Store all values in `memory/repos/<repo-name>.md`. Users can adjust any setting at any time.

## Architecture

```
.github/
├── workflows/
│   ├── pr-review.yml         # Tests → AI review → trigger fix
│   └── claude-fix.yml        # Auto-fix agent (@claude comment trigger)
├── scripts/
│   └── ai_review.py          # Review agent (calls Bedrock)
└── prompts/
    ├── review.md              # Review prompt (repo-specific)
    └── fix.md                 # Fix agent instructions (repo-specific)
```

## Step 1: Prerequisites (guided setup)

Walk the user through each prerequisite. Check what exists before creating.

### 1a. OIDC Federation

Check for existing secret: `gh secret list --repo OWNER/REPO | grep AWS_OIDC_ROLE_ARN`

If missing, guide through [references/oidc-setup.md](references/oidc-setup.md): create OIDC provider, IAM role with Bedrock permissions, set repo secret.

### 1b. GitHub App

Ask: **Do you already have a GitHub App, or create a new one?**

- **New** → guide through creation in [references/github-app-setup.md](references/github-app-setup.md).
- **Existing** → validate per [references/github-app-setup.md](references/github-app-setup.md) § "Validating an Existing App": check installation, permissions (Contents/Issues/PRs R/W), secrets.

Store the App name and bot ID in repo config.

### 1c. Notification Secrets

Based on configured channel, guide setting required secrets:
- **Telegram**: `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID`
- **Slack**: `SLACK_WEBHOOK_URL`
- **WhatsApp**: `WHATSAPP_TOKEN`, `WHATSAPP_PHONE_ID`, `WHATSAPP_TO`
- **none**: skip

## Step 2: Generate Review Prompt

Create `.github/prompts/review.md` using [references/review-prompt-template.md](references/review-prompt-template.md). Customize project context, review categories, and severity definitions for the repo's stack.

Do not change the JSON output format — the script depends on it.

## Step 3: Generate Fix Prompt

Create `.github/prompts/fix.md` with test commands per component, conventions reference, and rules: fix source not tests, minimal changes, revert if tests break.

## Step 4: Deploy Review Script

Copy `scripts/ai_review.py` to `.github/scripts/ai_review.py`. All behavior is controlled via workflow env vars:

| Env Var | Default |
|---------|---------|
| `MODEL_ID` | `us.anthropic.claude-opus-4-6-v1` |
| `MAX_FILES` | `15` |
| `MAX_CHANGED_LINES` | `1500` |
| `MAX_OUTPUT_TOKENS` | `16384` |
| `AWS_REGION` | `us-east-1` |
| `NOTIFY_CHANNEL` | `none` |

Notification secrets are passed via workflow env block based on configured channel.

## Step 5: Deploy Workflows

Generate from [references/workflow-templates.md](references/workflow-templates.md). Customize test steps, env vars, bot name/ID, and notification channel from repo config.

**pr-review.yml** — test → review → trigger-fix (3 jobs).
**claude-fix.yml** — triggered by `@claude` comments from the App bot.

## Step 6: Commit & PR

Branch `feat/ai-code-review`, commit all files, open PR. The workflows run on the PR itself as a smoke test.

## Design Notes

- **Auto-resolve**: on re-push, asks Bedrock if prior findings are fixed, resolves threads automatically.
- **Cycle limit**: counts `@claude` fix comments; stops after max cycles to prevent loops.
- **App token**: GITHUB_TOKEN comments don't trigger `issue_comment` — App token required.
- **Inline fallback**: findings posted as inline comments; fall back to summary if position mapping fails.
- **Unresolved threads block merge**: prior unresolved findings count as blockers until addressed.

## Rules

- **NEVER** hardcode AWS credentials — always use OIDC federation.
- **NEVER** use `GITHUB_TOKEN` for auto-fix comments — use GitHub App token.
- **NEVER** set `cancel-in-progress: true` on fix workflows — kills in-progress fixes.
- **NEVER** suppress test failures with `|| echo` — silent failures are worse than loud ones.
- Concurrency groups per PR number — prevent parallel reviews/fixes on the same PR.
