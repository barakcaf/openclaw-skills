---
name: aws
description: "Router for all AWS skills — architecture, deployment, serverless, databases, SageMaker, observability, and more. Use when the task involves any AWS service, infrastructure, or cloud operation. This skill selects and loads the appropriate sub-skill."
---

# AWS Skills Router

When this skill is triggered, **do not execute blindly** — scan the sub-skill index below, pick the best match, then `read` its SKILL.md and follow it.

If multiple sub-skills could apply, pick the most specific one.
If none clearly match, use `aws-mcp` (general-purpose AWS tasks) or `deploy-on-aws` (architecture + deployment).

## Sub-Skill Index

| Sub-Skill | When to Use |
|---|---|
| **deploy-on-aws** | Deploy apps to AWS, architecture recommendations, cost estimates, IaC generation |
| **aws-architecture-diagram** | Generate validated AWS architecture diagrams as draw.io XML |
| **aws-infrastructure-as-code** | CDK/CloudFormation — build, validate, troubleshoot IaC templates |
| **cloud-architect** | AWS CDK in Python, Well-Architected framework best practices |
| **aws-mcp** | General multi-step AWS tasks, documentation lookup, API calls, Agent SOPs |
| **aws-amplify** | Full-stack apps with Amplify Gen 2 — auth, data, storage, functions |
| **aws-serverless-deployment** | SAM/CDK deployment for serverless apps |
| **aws-lambda** | Design, build, deploy, test Lambda functions |
| **aws-lambda-durable-functions** | Durable Lambda functions — state persistence, retry, saga pattern, callbacks |
| **lambda-durable** | (Legacy) Durable Lambda functions — same domain as above |
| **api-gateway** | Build and manage REST, HTTP, WebSocket APIs with API Gateway |
| **databases-on-aws** | Aurora DSQL — schemas, queries, migrations, multi-tenant patterns |
| **aws-observability** | CloudWatch Logs/Metrics/Alarms, Application Signals APM, CloudTrail |
| **cloudwatch-application-signals** | ⚠️ DEPRECATED — use aws-observability instead |
| **sagemaker-planning** | SageMaker orchestrator — discovers intent, generates step-by-step plan |
| **sagemaker-use-case-specification** | Define business problem, stakeholders, success criteria for ML projects |
| **sagemaker-dataset-evaluation** | Validate dataset formatting/quality for SageMaker fine-tuning |
| **sagemaker-dataset-transformation** | Transform datasets between ML schemas (Jupyter notebook) |
| **sagemaker-finetuning-setup** | Select base model and fine-tuning technique (SFT, DPO, RLVR) |
| **sagemaker-finetuning** | Fine-tune models with SageMaker serverless training (Jupyter notebook) |
| **sagemaker-model-deployment** | Deploy fine-tuned models to SageMaker endpoints (Jupyter notebook) |
| **sagemaker-model-evaluation** | Evaluate fine-tuned models with LLM-as-a-Judge (Jupyter notebook) |
| **sagemaker-directory-management** | Project directory setup and artifact organization for SageMaker |
| **sagemaker-hyperpod-issue-report** | Diagnostic reports from HyperPod clusters (EKS/Slurm) |
| **sagemaker-hyperpod-ssm** | Remote command execution on HyperPod nodes via SSM |
| **sagemaker-hyperpod-version-checker** | Check software versions on HyperPod nodes (NVIDIA, CUDA, etc.) |
| **aws-agentcore** | Build/deploy AI agents with Bedrock AgentCore |
| **bedrock-pr-review** | Set up AI-powered PR review with Bedrock + GitHub Actions |
| **aws-healthomics** | Genomics workflows in AWS HealthOmics |
| **aws-graviton-migration** | Analyze code compatibility with Graviton/Arm64 processors |
| **amazon-location-service** | Maps, geocoding, routing, places search with Amazon Location Service |
| **spark-troubleshooting-agent** | Troubleshoot Spark on EMR, Glue, SageMaker |

## How to Load a Sub-Skill

```
read ~/.openclaw/workspace/skills/aws/skills/<sub-skill-name>/SKILL.md
```

For SageMaker tasks: start with **sagemaker-planning** — it orchestrates the other sagemaker-* skills.
