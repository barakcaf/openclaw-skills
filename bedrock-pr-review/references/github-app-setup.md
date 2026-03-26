# GitHub App Setup — Auto-Fix Bot

A GitHub App is required because comments posted with `GITHUB_TOKEN` don't trigger `issue_comment` workflows. The App posts `@claude` comments that trigger `claude-fix.yml`.

## Creating a New App

Go to: `https://github.com/settings/apps/new`

Settings:
- **Name**: `<repo-name>-autofix` (must be globally unique)
- **Homepage URL**: your repo URL
- **Webhook**: uncheck "Active" (not needed)
- **Permissions**:
  - Repository: Contents (Read & Write), Issues (Read & Write), Pull Requests (Read & Write)
- **Where can this app be installed?**: Only on this account

After creation:
1. Install on repo: Settings → Install App → select your repo.
2. Generate private key: App settings → General → Private keys → Generate. Download the `.pem` file.
3. Add repo secrets:
   ```bash
   gh secret set APP_ID --repo <OWNER>/<REPO> --body "<app-id>"
   gh secret set APP_PRIVATE_KEY --repo <OWNER>/<REPO> < path/to/private-key.pem
   ```
4. Look up bot user ID:
   ```bash
   curl -s https://api.github.com/users/<app-name>%5Bbot%5D | jq .id
   ```

The bot login format is `<app-name>[bot]` — must match exactly in workflow `if` conditions and `allowed_bots`.

## Validating an Existing App

If the user already has a GitHub App, validate it meets requirements:

1. **Verify installation on repo:**
   ```bash
   gh api repos/OWNER/REPO/installation --jq '.app_id'
   ```

2. **Check required permissions** (Contents R/W, Issues R/W, Pull Requests R/W):
   ```bash
   gh api apps/<app-slug> --jq '.permissions | {contents, issues, pull_requests}'
   ```
   If any permission is missing, direct user to: App settings → Permissions & events.

3. **Verify secrets exist:**
   ```bash
   gh secret list --repo OWNER/REPO | grep -E 'APP_ID|APP_PRIVATE_KEY'
   ```
   If missing, guide setting them (see step 3 above).

4. **Look up bot user ID:**
   ```bash
   curl -s https://api.github.com/users/<app-name>%5Bbot%5D | jq .id
   ```

## Common Issues

- **Comments don't trigger fix workflow**: Check that the comment is posted with the App token, not GITHUB_TOKEN.
- **Wrong bot login in workflow**: Must be `<app-name>[bot]` exactly, including `[bot]` suffix.
- **App not installed on repo**: Install via Settings → GitHub Apps → Configure.

## Security Best Practices

- **Rotate private keys periodically** — generate a new key in App settings, update the `APP_PRIVATE_KEY` repo secret, then delete the old key.
- **Limit App installation scope** — install only on the specific repo that needs it, not org-wide. This limits blast radius if the key is compromised.
- **Minimum permissions** — only Contents (R/W), Issues (R/W), Pull Requests (R/W). Do not add admin, org, or other permissions.
- **Webhook disabled** — the App doesn't need inbound webhooks. Keep "Active" unchecked to reduce attack surface.
