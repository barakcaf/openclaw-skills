# Workflow Templates

Use these as starting points. Customize test steps, bot names, and notification for each repo.

## pr-review.yml

```yaml
name: PR Tests & Review

on:
  pull_request:
    types: [opened, synchronize, reopened]

concurrency:
  group: pr-review-${{ github.event.pull_request.number }}
  cancel-in-progress: true

permissions:
  contents: read
  pull-requests: write
  checks: write
  id-token: write

jobs:
  test:
    name: Run Tests
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      # === CUSTOMIZE: Add setup + test steps per component ===
      # Each test step should:
      #   - Have an id (e.g., steps.frontend-tests)
      #   - Use continue-on-error: true
      #
      # Example:
      # - name: Run frontend tests
      #   id: frontend-tests
      #   working-directory: frontend
      #   run: npm ci && npm test -- --run
      #   continue-on-error: true

      - name: Post test results
        if: always()
        uses: actions/github-script@v7
        with:
          script: |
            // CUSTOMIZE: Add each test step outcome
            // const frontend = '${{ steps.frontend-tests.outcome }}';
            // const icon = (s) => s === 'success' ? '✅' : s === 'failure' ? '❌' : '⚠️';
            const marker = '## 🧪 Test Results';
            const body = `${marker}\n\n` +
              `| Suite | Status |\n|-------|--------|\n` +
              // CUSTOMIZE: Add rows per test suite
              // `| Frontend | ${icon(frontend)} ${frontend} |\n` +
              `| All | ✅ passed |\n`;

            // Find and update existing comment, or create new
            const { data: comments } = await github.rest.issues.listComments({
              owner: context.repo.owner, repo: context.repo.repo,
              issue_number: context.issue.number,
            });
            const existing = comments.find(c =>
              c.user.login === 'github-actions[bot]' && c.body.includes(marker)
            );
            if (existing) {
              await github.rest.issues.updateComment({
                owner: context.repo.owner, repo: context.repo.repo,
                comment_id: existing.id, body: body,
              });
            } else {
              await github.rest.issues.createComment({
                owner: context.repo.owner, repo: context.repo.repo,
                issue_number: context.issue.number, body: body,
              });
            }

      - name: Gate — fail if any tests failed
        if: always()
        run: |
          # === CUSTOMIZE: Check each test step outcome ===
          # if [ "${{ steps.frontend-tests.outcome }}" = "failure" ]; then
          #   echo "❌ Tests failed"; exit 1
          # fi
          echo "✅ All tests passed"

  review:
    name: AI Code Review
    needs: test
    runs-on: ubuntu-latest
    if: github.actor != 'dependabot[bot]'
    outputs:
      has_findings: ${{ steps.run-review.outputs.has_findings }}
      findings_json: ${{ steps.run-review.outputs.findings_json }}
      findings_summary: ${{ steps.run-review.outputs.findings_summary }}

    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Configure AWS credentials (OIDC)
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_OIDC_ROLE_ARN }}
          aws-region: us-east-1  # CUSTOMIZE: your Bedrock region

      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - run: pip install boto3

      - name: Run AI review
        id: run-review
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          PR_NUMBER: ${{ github.event.pull_request.number }}
          REPO_NAME: ${{ github.repository }}
          # CUSTOMIZE: from repo config (defaults shown)
          MODEL_ID: "us.anthropic.claude-opus-4-6-v1"
          MAX_FILES: "15"
          MAX_CHANGED_LINES: "1500"
          MAX_OUTPUT_TOKENS: "16384"
          # CUSTOMIZE: notification channel from repo config
          # NOTIFY_CHANNEL: "telegram"  # telegram | slack | whatsapp | none
          # TELEGRAM_BOT_TOKEN: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          # TELEGRAM_CHAT_ID: ${{ secrets.TELEGRAM_CHAT_ID }}
          # SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
          # WHATSAPP_TOKEN: ${{ secrets.WHATSAPP_TOKEN }}
          # WHATSAPP_PHONE_ID: ${{ secrets.WHATSAPP_PHONE_ID }}
          # WHATSAPP_TO: ${{ secrets.WHATSAPP_TO }}
        run: python .github/scripts/ai_review.py

  trigger-fix:
    name: Trigger Auto-Fix
    needs: review
    runs-on: ubuntu-latest
    if: >
      needs.review.outputs.has_findings == 'true' &&
      github.actor != 'dependabot[bot]' &&
      !contains(github.event.pull_request.labels.*.name, 'no-auto-fix')

    steps:
      - name: Generate GitHub App token
        id: app-token
        uses: actions/create-github-app-token@v2
        with:
          app-id: ${{ secrets.APP_ID }}
          private-key: ${{ secrets.APP_PRIVATE_KEY }}

      - name: Check cycle count
        id: cycle
        uses: actions/github-script@v7
        with:
          github-token: ${{ steps.app-token.outputs.token }}
          script: |
            const appId = '${{ secrets.APP_ID }}';
            const comments = await github.paginate(
              github.rest.issues.listComments,
              { owner: context.repo.owner, repo: context.repo.repo, issue_number: context.issue.number }
            );
            const fixComments = comments.filter(c =>
              c.user.type === 'Bot' &&
              c.performed_via_github_app?.id === Number(appId) &&
              c.body.includes('@claude') &&
              c.body.includes('[ai-fix-cycle-')
            );
            const cycle = fixComments.length + 1;
            // CUSTOMIZE: max cycles from repo config (default 2)
            const maxCycles = 2;
            core.setOutput('cycle', cycle);
            core.setOutput('skip', cycle > maxCycles ? 'true' : 'false');

      - name: Post max-cycles comment
        if: steps.cycle.outputs.skip == 'true'
        uses: actions/github-script@v7
        with:
          github-token: ${{ steps.app-token.outputs.token }}
          script: |
            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              body: '⚠️ **Auto-Fix Limit Reached**\n\nMax fix cycles completed. Remaining findings require manual intervention.',
            });

      - name: Post @claude fix comment
        if: steps.cycle.outputs.skip != 'true'
        uses: actions/github-script@v7
        env:
          FINDINGS_SUMMARY: ${{ needs.review.outputs.findings_summary }}
        with:
          github-token: ${{ steps.app-token.outputs.token }}
          script: |
            const cycle = '${{ steps.cycle.outputs.cycle }}';
            const findings = process.env.FINDINGS_SUMMARY;
            const body = `@claude [ai-fix-cycle-${cycle}] Fix the following code review findings. ` +
              `Read .github/prompts/fix.md for detailed instructions.\n\n` +
              `**Available tools:** Glob, Grep, LS, Read, Bash (git add, git commit, git push, git rm). ` +
              `Use Bash to write/edit files (e.g. sed, cat, echo). Do NOT attempt to use Write or Edit tools — they are not available.\n\n` +
              `**Findings (cycle ${cycle}):**\n\n${findings}`;
            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              body: body,
            });
```

## claude-fix.yml

```yaml
name: Auto-Fix

on:
  issue_comment:
    types: [created]

concurrency:
  group: auto-fix-${{ github.event.issue.number }}
  cancel-in-progress: false  # NEVER cancel in-progress fixes

permissions:
  contents: write
  pull-requests: write
  issues: write
  id-token: write

jobs:
  fix:
    name: Claude Code Fix
    runs-on: ubuntu-latest
    if: >
      github.event.issue.pull_request &&
      contains(github.event.comment.body, '@claude') &&
      !contains(github.event.issue.labels.*.name, 'no-auto-fix') &&
      (
        github.event.comment.user.login == '<app-name>[bot]' ||
        github.event.comment.user.login == 'github-actions[bot]'
      )
    # CUSTOMIZE: Replace <app-name>[bot] with your GitHub App's bot login

    steps:
      - name: Configure AWS credentials (OIDC)
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_OIDC_ROLE_ARN }}
          aws-region: us-east-1  # CUSTOMIZE: your Bedrock region

      - name: Generate GitHub App token
        id: app-token
        uses: actions/create-github-app-token@v2
        with:
          app-id: ${{ secrets.APP_ID }}
          private-key: ${{ secrets.APP_PRIVATE_KEY }}

      - uses: actions/checkout@v4
        with:
          token: ${{ steps.app-token.outputs.token }}
          fetch-depth: 0

      - name: Run Claude Code
        id: claude-fix
        uses: anthropics/claude-code-action@v1
        with:
          use_bedrock: "true"
          claude_args: "--model sonnet --max-turns 25"  # CUSTOMIZE: model + max-turns from repo config
          github_token: ${{ steps.app-token.outputs.token }}
          trigger_phrase: "@claude"
          # CUSTOMIZE: bot name and ID
          allowed_bots: "<app-name>[bot]"
          bot_id: "<bot-user-id>"
          bot_name: "<app-name>"

      # CUSTOMIZE: Add notification step based on configured channel
      # - name: Notify on completion
      #   if: always()
      #   uses: actions/github-script@v7
      #   env:
      #     NOTIFY_CHANNEL: "telegram"  # from repo config
      #     TELEGRAM_BOT_TOKEN: ${{ secrets.TELEGRAM_BOT_TOKEN }}
      #     TELEGRAM_CHAT_ID: ${{ secrets.TELEGRAM_CHAT_ID }}
      #   with:
      #     script: |
      #       const outcome = '${{ steps.claude-fix.outcome }}';
      #       const prNumber = context.payload.issue.number;
      #       const prTitle = context.payload.issue.title;
      #       const emoji = outcome === 'success' ? '🔧' : '⚠️';
      #       const status = outcome === 'success' ? 'Auto-Fix Applied' : 'Auto-Fix Failed';
      #       const msg = `${emoji} ${status}: #${prNumber} — ${prTitle}`;
      #       // Send via configured channel (Telegram/Slack/WhatsApp)
```

## Customization Checklist

All values come from repo config (stored in `memory/repos/<repo-name>.md`). For each new repo, update:

- [ ] Test steps in `pr-review.yml` (languages, commands, working directories)
- [ ] `MODEL_ID`, `MAX_FILES`, `MAX_CHANGED_LINES`, `MAX_OUTPUT_TOKENS` env vars in review job
- [ ] AWS region in OIDC credential steps
- [ ] Bot name/ID in `claude-fix.yml`
- [ ] `--model` and `--max-turns` in `claude-fix.yml` `claude_args`
- [ ] `maxCycles` in trigger-fix job
- [ ] Notification steps (add Telegram/Slack or remove)
