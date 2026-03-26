# Workflow Templates

Use these as starting points. Customize test steps, bot names, and notification for each repo.

> **`no-auto-fix` label:** Add this label to any PR to skip automatic fix attempts.
> Useful when findings are false positives or require manual intervention.

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
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      # === CUSTOMIZE: Add setup + test steps per component ===
      # Each test step should:
      #   - Have a `name` and an `id` (e.g., id: frontend-tests)
      #   - Use `continue-on-error: true` so the gate step controls failure
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
            // CUSTOMIZE: Uncomment and add each test step outcome
            // const frontend = '${{ steps.frontend-tests.outcome }}';
            const icon = (s) => s === 'success' ? '✅' : s === 'failure' ? '❌' : '⚠️';
            const marker = '## 🧪 Test Results';
            const body = `${marker}\n\n` +
              `| Suite | Status |\n|-------|--------|\n` +
              // CUSTOMIZE: Add rows per test suite
              // `| Frontend | ${icon(frontend)} ${frontend} |\n` +
              `| All | ✅ passed |\n`;

            const { data: comments } = await github.rest.issues.listComments({
              owner: context.repo.owner, repo: context.repo.repo,
              issue_number: context.issue.number,
            });
            const existing = comments.find(c =>
              c.user.type === 'Bot' &&
              c.user.login === 'github-actions[bot]' &&
              c.body.includes(marker)
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
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Configure AWS credentials (OIDC)
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_OIDC_ROLE_ARN }}
          aws-region: us-east-1  # CUSTOMIZE: your Bedrock region

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.12"

      - name: Install dependencies
        run: pip install boto3

      - name: Run AI review
        id: run-review
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          PR_NUMBER: ${{ github.event.pull_request.number }}
          REPO_NAME: ${{ github.repository }}
          # CUSTOMIZE: model and limits from repo config
          MODEL_ID: "us.anthropic.claude-opus-4-6-v1"
          MAX_FILES: "15"
          MAX_CHANGED_LINES: "1500"
          MAX_OUTPUT_TOKENS: "16384"
          # CUSTOMIZE: uncomment the notification channel you want
          # NOTIFY_CHANNEL: "telegram"
          # TELEGRAM_BOT_TOKEN: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          # TELEGRAM_CHAT_ID: ${{ secrets.TELEGRAM_CHAT_ID }}
          # NOTIFY_CHANNEL: "slack"
          # SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
          # NOTIFY_CHANNEL: "whatsapp"
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
            console.log(`Fix cycle: ${cycle}/${maxCycles} (skip: ${cycle > maxCycles})`);
            console.log(`Found ${fixComments.length} existing fix comments from App ID ${appId}`);

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
            const maxCycles = 2;  // CUSTOMIZE: match maxCycles above
            const findings = process.env.FINDINGS_SUMMARY;
            const body = `@claude [ai-fix-cycle-${cycle}] Fix the following code review findings. ` +
              `Read .github/prompts/fix.md for detailed instructions.\n\n` +
              `**Available tools:** Glob, Grep, LS, Read, Bash (git add, git commit, git push, git rm). ` +
              `Use Bash to write/edit files (e.g. sed, cat, echo). Do NOT attempt to use Write or Edit tools — they are not available.\n\n` +
              `**Rules:**\n` +
              `- Read the PR description to understand the feature context before fixing\n` +
              `- Fix the source code, NOT the tests\n` +
              `- Run tests after each fix (see CLAUDE.md for commands)\n` +
              `- If a fix breaks tests, revert it\n` +
              `- Only keep fixes where all tests still pass\n` +
              `- Commit message must include [ai-fix-cycle-${cycle}]\n\n` +
              `**Findings (cycle ${cycle}/${maxCycles}):**\n\n${findings}`;
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

# NOTE: Add the 'no-auto-fix' label to a PR to disable automatic fix attempts.
# This is useful when findings are false positives or require manual intervention.
jobs:
  fix:
    name: Claude Code Fix
    runs-on: ubuntu-latest
    if: >
      github.event.issue.pull_request &&
      contains(github.event.comment.body, '@claude') &&
      (
        github.event.comment.user.login == '<app-name>[bot]' ||
        github.event.comment.user.login == 'github-actions[bot]'
      ) &&
      !contains(github.event.issue.labels.*.name, 'no-auto-fix')
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

      - name: Checkout PR branch
        uses: actions/checkout@v4
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
          # CUSTOMIZE: bot name and ID from your GitHub App
          allowed_bots: "<app-name>[bot]"
          bot_id: "<bot-user-id>"
          bot_name: "<app-name>"

      - name: Gather fix summary
        if: always()
        id: summary
        uses: actions/github-script@v7
        with:
          script: |
            const fixOutcome = '${{ steps.claude-fix.outcome }}';
            const prNumber = context.payload.issue.number;
            const prTitle = context.payload.issue.title;

            // Find the triggering @claude comment to extract cycle and findings
            const commentBody = context.payload.comment.body || '';
            const cycleMatch = commentBody.match(/\[ai-fix-cycle-(\d+)\]/);
            const cycle = cycleMatch ? cycleMatch[1] : '?';

            const findingsMatch = commentBody.match(/Findings \(cycle \d+\/\d+\):\s*\n\n([\s\S]*)/);
            const findingsText = findingsMatch ? findingsMatch[1] : '';
            const highCount = (findingsText.match(/🟠/g) || []).length;
            const mediumCount = (findingsText.match(/🟡/g) || []).length;
            const totalFindings = highCount + mediumCount;

            let filesChanged = 0;
            let commitMsg = '';
            if (fixOutcome === 'success') {
              try {
                const { data: pr } = await github.rest.pulls.get({
                  owner: context.repo.owner,
                  repo: context.repo.repo,
                  pull_number: prNumber,
                });
                const { data: commits } = await github.rest.pulls.listCommits({
                  owner: context.repo.owner,
                  repo: context.repo.repo,
                  pull_number: prNumber,
                  per_page: 5,
                });
                const latest = commits[commits.length - 1];
                if (latest) {
                  commitMsg = latest.commit.message.split('\n')[0];
                  filesChanged = pr.changed_files;
                }
              } catch (e) {
                console.log('Could not fetch PR details:', e.message);
              }
            }

            let emoji, status, details;
            if (fixOutcome === 'success') {
              emoji = '🔧';
              status = 'Auto-Fix Applied';
              details = `Cycle ${cycle}: ${totalFindings} finding${totalFindings !== 1 ? 's' : ''} addressed`;
              if (highCount > 0) details += ` (${highCount} HIGH, ${mediumCount} MEDIUM)`;
              if (commitMsg) details += `\nCommit: ${commitMsg}`;
              if (filesChanged > 0) details += `\nFiles changed: ${filesChanged}`;
            } else {
              emoji = '⚠️';
              status = 'Auto-Fix Failed';
              details = `Cycle ${cycle}: Claude could not resolve ${totalFindings} finding${totalFindings !== 1 ? 's' : ''}`;
              if (highCount > 0) details += ` (${highCount} HIGH)`;
            }

            const prUrl = context.payload.issue.html_url;
            const message = `${emoji} <b>${status}</b>: #${prNumber} — ${prTitle}\n${details}\n<a href="${prUrl}">View PR</a>`;
            core.setOutput('message', message);

      # === CUSTOMIZE: Uncomment ONE notification block below ===

      # --- Telegram ---
      # - name: Notify via Telegram
      #   if: always() && steps.summary.outputs.message
      #   uses: actions/github-script@v7
      #   env:
      #     TELEGRAM_BOT_TOKEN: ${{ secrets.TELEGRAM_BOT_TOKEN }}
      #     TELEGRAM_CHAT_ID: ${{ secrets.TELEGRAM_CHAT_ID }}
      #     NOTIFY_MESSAGE: ${{ steps.summary.outputs.message }}
      #   with:
      #     script: |
      #       const message = process.env.NOTIFY_MESSAGE;
      #       const token = process.env.TELEGRAM_BOT_TOKEN;
      #       const chatId = process.env.TELEGRAM_CHAT_ID;
      #       if (token && chatId) {
      #         const response = await fetch(`https://api.telegram.org/bot${token}/sendMessage`, {
      #           method: 'POST',
      #           headers: { 'Content-Type': 'application/json' },
      #           body: JSON.stringify({
      #             chat_id: chatId, text: message,
      #             parse_mode: 'HTML', disable_web_page_preview: true
      #           })
      #         });
      #         if (!response.ok) console.log('Telegram notification failed:', await response.text());
      #       }

      # --- Slack ---
      # - name: Notify via Slack
      #   if: always() && steps.summary.outputs.message
      #   uses: actions/github-script@v7
      #   env:
      #     SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
      #     NOTIFY_MESSAGE: ${{ steps.summary.outputs.message }}
      #   with:
      #     script: |
      #       const message = process.env.NOTIFY_MESSAGE.replace(/<[^>]+>/g, '');
      #       const url = process.env.SLACK_WEBHOOK_URL;
      #       if (url) {
      #         const response = await fetch(url, {
      #           method: 'POST',
      #           headers: { 'Content-Type': 'application/json' },
      #           body: JSON.stringify({ text: message })
      #         });
      #         if (!response.ok) console.log('Slack notification failed:', await response.text());
      #       }
```

## Customization Checklist

All values come from repo config (stored in `memory/repos/<repo-name>.md`). For each new repo, update:

- [ ] Test steps in `pr-review.yml` (languages, commands, working directories)
- [ ] `MODEL_ID`, `MAX_FILES`, `MAX_CHANGED_LINES`, `MAX_OUTPUT_TOKENS` env vars in review job
- [ ] AWS region in OIDC credential steps
- [ ] Bot name/ID in `claude-fix.yml` (`allowed_bots`, `bot_id`, `bot_name`)
- [ ] `--model` and `--max-turns` in `claude-fix.yml` `claude_args`
- [ ] `maxCycles` in trigger-fix job (update both the check and the comment)
- [ ] Notification channel — uncomment the relevant block in both `pr-review.yml` (review job env vars) and `claude-fix.yml` (notification step)
- [ ] Set the corresponding secrets: `TELEGRAM_BOT_TOKEN`/`TELEGRAM_CHAT_ID`, or `SLACK_WEBHOOK_URL`, or `WHATSAPP_TOKEN`/`WHATSAPP_PHONE_ID`/`WHATSAPP_TO`
