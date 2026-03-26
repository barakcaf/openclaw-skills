# OIDC Setup — GitHub Actions → AWS Bedrock

Set up OIDC federation so GitHub Actions can assume an IAM role with Bedrock access. No long-lived credentials needed.

## 1. Create OIDC Identity Provider

```bash
aws iam create-open-id-connect-provider \
  --url https://token.actions.githubusercontent.com \
  --client-id-list sts.amazonaws.com \
  --thumbprint-list 6938fd4d98bab03faadb97b34396831e3780aea1
```

## 2. Create IAM Role

Trust policy — restrict to your repo and non-fork refs only:

> ⚠️ **Security:** Using `repo:<OWNER>/<REPO>:*` allows ANY ref including fork PRs to assume the role. Always restrict to specific ref patterns to prevent unauthorized Bedrock access from forks.

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "Federated": "arn:aws:iam::<ACCOUNT_ID>:oidc-provider/token.actions.githubusercontent.com"
    },
    "Action": "sts:AssumeRoleWithWebIdentity",
    "Condition": {
      "StringEquals": {
        "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
      },
      "StringLike": {
        "token.actions.githubusercontent.com:sub": "repo:<OWNER>/<REPO>:ref:refs/heads/*"
      }
    }
  }]
}
```

> For even tighter control, use `ref:refs/heads/main` to restrict to the main branch only. Use `ref:refs/heads/*` if CI needs to run on feature branches too.

```bash
aws iam create-role \
  --role-name github-actions-ai-review \
  --assume-role-policy-document file://trust-policy.json
```

## 3. Attach Bedrock Permissions

Inline policy — scoped to configured region and Anthropic models only:

> ⚠️ **Least privilege:** Scope the resource to your specific region and model family. Avoid `*` wildcards for region or model.

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": [
      "bedrock:InvokeModel",
      "bedrock:InvokeModelWithResponseStream"
    ],
    "Resource": [
      "arn:aws:bedrock:<REGION>:<ACCOUNT_ID>:inference-profile/us.anthropic.*",
      "arn:aws:bedrock:<REGION>::foundation-model/anthropic.*"
    ]
  }]
}
```

Replace `<REGION>` with your configured AWS region (e.g., `us-east-1`).

```bash
aws iam put-role-policy \
  --role-name github-actions-ai-review \
  --policy-name bedrock-invoke \
  --policy-document file://bedrock-policy.json
```

## 4. Add Repo Secret

```bash
gh secret set AWS_OIDC_ROLE_ARN --repo <OWNER>/<REPO> \
  --body "arn:aws:iam::<ACCOUNT_ID>:role/github-actions-ai-review"
```

## 5. Use in Workflow

```yaml
- name: Configure AWS credentials (OIDC)
  uses: aws-actions/configure-aws-credentials@v4
  with:
    role-to-assume: ${{ secrets.AWS_OIDC_ROLE_ARN }}
    aws-region: us-east-1
```

Requires `id-token: write` permission in the workflow.
