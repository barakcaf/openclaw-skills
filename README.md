# openclaw-skills

A collection of skills for [OpenClaw](https://github.com/openclaw/openclaw) — AI agent capability packages that give your agent specialized knowledge and workflows.

## What's a skill?

A skill is a directory containing a `SKILL.md` (instructions + context for the agent) and optional reference material. OpenClaw loads skills on demand when a task matches their description. Think of them as domain-expert modules you bolt onto your agent.

## Installation

```bash
# Clone into your workspace skills directory
git clone https://github.com/barakcaf/openclaw-skills ~/.openclaw/workspace/skills
```

Skills are auto-discovered from `~/.openclaw/workspace/skills/` — no config registration needed. They appear in the agent's `<available_skills>` context automatically.

---

## Skills

### Custom Skills

Built for OpenClaw from scratch:

| Skill | What it does |
|-------|-------------|
| `agent-docs-auditor` | Audit and improve CLAUDE.md, AGENTS.md, and SKILL.md files |
| `claude-agent-sdk` | Build AI agents with the Claude Agent SDK (Python & TypeScript) |
| `cross-agent-test` | Zero-context cross-agent CLI testing via Claude Code |
| `engineering-team` | Scaffold a 3-agent team (principal, security, UX engineers) into any repo |
| `feature-workflow` | End-to-end feature/bug workflow: design → branch → PR → merge |
| `outline` | Interact with the Outline wiki API — create, search, manage docs |
| `reposwarm` | Manage RepoSwarm investigations, workflows, and architecture analysis |

### AWS & Cloud

| Skill | What it does |
|-------|-------------|
| `aws-agentcore` | Build and deploy agents with Amazon Bedrock AgentCore |
| `aws-amplify` | Full-stack apps with AWS Amplify Gen 2 |
| `aws-graviton-migration` | Analyze code compatibility with Graviton/Arm64 |
| `aws-healthomics` | Genomics workflows in AWS HealthOmics |
| `aws-infrastructure-as-code` | CDK + CloudFormation best practices |
| `aws-mcp` | Multi-step AWS tasks using docs, APIs, and Agent SOPs |
| `aws-observability` | CloudWatch Logs, Metrics, Alarms, Application Signals, CloudTrail |
| `bedrock-pr-review` | AI-powered PR review via GitHub Actions + Bedrock Claude |
| `cloud-architect` | AWS CDK in Python following Well-Architected framework |
| `lambda-durable` | Resilient multi-step apps with Lambda durable functions |
| `spark-troubleshooting-agent` | Troubleshoot Spark on EMR, Glue, and SageMaker |

### Infrastructure & DevOps

| Skill | What it does |
|-------|-------------|
| `arm-soc-migration` | Migrate code between Arm SoC architectures |
| `terraform` | Infrastructure as Code with Terraform — registry, modules, policies |

### Data & Observability

| Skill | What it does |
|-------|-------------|
| `datadog` | Query logs, metrics, traces, and incidents from Datadog |
| `dynatrace` | Query logs, metrics, traces, and problems from Dynatrace via DQL |

### Databases & Backend

| Skill | What it does |
|-------|-------------|
| `neon` | Serverless Postgres with branching, autoscaling, scale-to-zero |
| `saas-builder` | Multi-tenant SaaS with serverless, billing, and security |
| `strands` | Build AI agents with Strands SDK (Bedrock, Anthropic, OpenAI, etc.) |

### Payments & APIs

| Skill | What it does |
|-------|-------------|
| `checkout` | Checkout.com API docs and payment operations |
| `postman` | Automate API testing and collection management |
| `stripe` | Payments, subscriptions, billing, refunds |

### Design & Frontend

| Skill | What it does |
|-------|-------------|
| `figma` | Connect Figma designs to code components |

> **Note:** `cloudwatch-application-signals` is deprecated — use `aws-observability` instead.

---

## Contributing

PRs welcome. To add a skill:

1. Create a directory: `skills/your-skill-name/`
2. Add a `SKILL.md` — see existing skills for format
3. Add reference material in `references/` if needed
4. Update this README

For the `SKILL.md` format and best practices, see the `agent-docs-auditor` skill.

## Credits

Many skills here are adapted from [Kiro Powers](https://kiro.dev/powers/) — reusable capability packages by AWS, partners, and the community for the [Kiro IDE](https://kiro.dev). The adaptation converts Kiro's format (MCP servers, hooks) into OpenClaw's skill format (SKILL.md + references). Core guidance and best practices stay faithful to the originals.

## License

Skills adapted from Kiro Powers retain their original licensing terms. Custom skills are provided as-is.
