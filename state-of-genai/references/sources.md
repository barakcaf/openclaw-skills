# GenAI Report — Source Registry

*Updated by the agent after each run. Add new sources with date discovered and rationale.*
*All RSS feeds verified as of 2026-04-03. Re-verify if a feed stops returning results.*

---

## Tier 1 — Must Check Every Run

### Lab Announcements & Primary Sources

| Source | URL | RSS / Monitor | Tavily Query |
|--------|-----|---------------|-------------|
| OpenAI News | `openai.com/news` | `openai.com/news/rss.xml` ✅ | `site:openai.com/news model release OR API after:{SEARCH_FROM}` |
| Anthropic News | `anthropic.com/news` | No RSS — check directly | `site:anthropic.com/news model OR API OR pricing after:{SEARCH_FROM}` |
| Google DeepMind Blog | `deepmind.google/blog` | `deepmind.google/blog/rss.xml` ✅ | `site:deepmind.google new model OR Gemini OR benchmark after:{SEARCH_FROM}` |
| Google Blog (The Keyword) | `blog.google` | `blog.google/rss/` ✅ | `site:blog.google AI OR Gemini OR model after:{SEARCH_FROM}` |
| Hugging Face Blog | `huggingface.co/blog` | `huggingface.co/blog/feed.xml` ✅ | `site:huggingface.co/blog model release OR benchmark OR open source after:{SEARCH_FROM}` |

### Independent Benchmarks & Rankings (Ground Truth)

| Source | URL | RSS / Monitor | Notes |
|--------|-----|---------------|-------|
| OpenRouter Rankings | `openrouter.ai/rankings` | Live dashboard — check directly | Top models by token volume. Week-over-week shifts = usage reality. No RSS. |
| LMSYS / Chatbot Arena | `arena.ai` | Live dashboard — check directly | ELO rankings by human preference. Check top-20 shifts. |
| Artificial Analysis | `artificialanalysis.ai/leaderboards/models` | No RSS (✅ site verified) | Independent benchmarks: quality, speed, pricing per 1M tokens across providers. Tier 1 for cross-provider comparisons. |
| Hugging Face Open LLM Leaderboard | `huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard` | Live — check directly | Open-weight model benchmarks. Best source for OSS frontier tracking. |
| Simon Willison's Weblog | `simonwillison.net` | `simonwillison.net/atom/everything/` ✅ | Daily posts covering every significant LLM release, benchmark, and tool update. Prolific and reliable. |
| Epoch AI | `epochai.org` | `epochai.substack.com/feed` ✅ (Gradient Updates newsletter) | Non-profit tracking AI compute trends, training costs, model database, FrontierMath benchmark. Research-backed analysis of AI progress trajectory. Key for compute economics and long-term trend data. |

### Analysis & Commentary

| Source | URL | RSS / Monitor | Tavily Query |
|--------|-----|---------------|-------------|
| SemiAnalysis | `semianalysis.com` | `semianalysis.com/feed/` ✅ | `site:semianalysis.com AI GPU compute training cost inference after:{SEARCH_FROM}` |
| Latent Space | `latent.space` | `latent.space/feed` ✅ | `site:latent.space model release OR AI engineering OR benchmark after:{SEARCH_FROM}` |
| Import AI (Jack Clark) | `importai.substack.com` | `importai.substack.com/feed` ✅ | `site:importai.substack.com model release OR AI policy OR safety OR benchmark after:{SEARCH_FROM}` |
| One Useful Thing (Ethan Mollick) | `oneusefulthing.org` | `oneusefulthing.org/feed` ✅ | `site:oneusefulthing.org AI model OR adoption OR research after:{SEARCH_FROM}` |
| Don't Worry About the Vase (Zvi Mowshowitz) | `thezvi.substack.com` | `thezvi.substack.com/feed` ✅ | `site:thezvi.substack.com AI model OR AGI OR safety OR OpenAI after:{SEARCH_FROM}` |
| The Next Platform | `nextplatform.com` | `nextplatform.com/feed/` ✅ | `site:nextplatform.com AI OR GPU OR datacenter after:{SEARCH_FROM}`. Deep HPC/infrastructure analysis distinct from SemiAnalysis: server market, datacenter power, GPU/TPU architecture. Soft paywall. |
| AI Alignment Forum | `alignmentforum.org` | `alignmentforum.org/feed.xml` ✅ | `site:alignmentforum.org interpretability OR alignment OR safety after:{SEARCH_FROM}`. Canonical primary venue for technical AI safety research. Community-run. |

### Independent Benchmarks & Rankings (Coding / Contamination-Free)

| Source | URL | RSS / Monitor | Notes |
|--------|-----|---------------|-------|
| Aider LLM Leaderboards | `aider.chat/docs/leaderboards/` | Live dashboard — check directly | Real-world coding benchmark across all frontier models. Key for code generation comparison. No RSS. |
| LiveBench | `livebench.ai` | Live dashboard — check directly | Contamination-free benchmark updated monthly with new questions. No RSS. |

---

## Tier 2 — Check on Signal

### Analysts & Commentators

| Source | URL | RSS | Notes |
|--------|-----|-----|-------|
| Stratechery (Ben Thompson) | `stratechery.com` | `stratechery.com/feed/` ✅ (free posts only) | AI strategy, business model analysis, big tech. Most pieces paywalled. Free articles include major AI strategy takes. |
| Benedict Evans | `ben-evans.com` | `ben-evans.com/feed` ✅ | Former a16z partner. Deep essays on AI business models, platform shifts, adoption curves. Annual "Big Presentation" is essential macro framing. |
| Marcus on AI (Gary Marcus) | `garymarcus.substack.com` | `garymarcus.substack.com/feed` ✅ | AI skeptic/critic. Essential counterpoint. Covers hype vs. reality, benchmarking issues, reliability failures. |
| State of AI Newsletter (Nathan Benaich) | `nathanbenaich.substack.com` | `nathanbenaich.substack.com/feed` ✅ | Annual State of AI Report + newsletter. Air Street Capital. Deep annual report is must-read. |
| Dwarkesh Podcast | `dwarkesh.com` | `dwarkesh.com/feed` ✅ (redirects from dwarkeshpatel.com) | Deep long-form interviews with AI researchers and lab founders. High signal when posted. |
| Epoch AI Newsletter (Gradient Updates) | `epochai.substack.com` | `epochai.substack.com/feed` ✅ | Research-backed analysis of compute trends, training dynamics, AI progress trajectory. |
| TLDR AI | `tldr.tech/ai` | `tldr.tech/api/rss/ai` ✅ | 920K+ readers. Daily digest of top AI papers, releases, tools. Good signal-to-noise for catching things missed. |
| Ben's Bites | `bensbites.com` | `bensbites.com/feed` ✅ | AI startup moves, investments, real-world use cases, product releases. 120K+ readers. |
| The Neuron | `theneuron.ai` | No RSS (✅ site verified) | 675K+ readers. Accessible AI tool coverage and trend analysis. Helpful for adoption signals. |
| AlphaSignal | `alphasignal.ai` | No RSS (✅ site verified) | 250K+ AI engineers. Daily digest of frontier papers, repos, models. |
| ChinAI Newsletter (Jeff Ding) | `chinai.substack.com` | `chinai.substack.com/feed` ✅ | Best English-language source for Chinese AI policy, research, and ecosystem analysis. Jeff Ding, GWU. Active 300+ issues. |
| The Batch (DeepLearning.AI / Andrew Ng) | `deeplearning.ai/the-batch` | No RSS (✅ site verified) | Weekly AI news curated by Andrew Ng. Bridges research to business implications. |
| BAIR Blog (Berkeley AI Research) | `bair.berkeley.edu/blog` | `bair.berkeley.edu/blog/feed.xml` ✅ | University research blog. Technical depth on RL, LLMs, diffusion models. |
| AI Tidbits | `aitidbits.substack.com` | `aitidbits.substack.com/feed` ✅ | Dedicated AI agents + coding tools sections. |
| Interconnects (Nathan Lambert) | `interconnects.ai` | Substack RSS ✅ | Open models, reasoning, agents. Ex-HuggingFace/Meta. |
| Ahead of AI (Sebastian Raschka) | `sebastianraschka.com/blog` | `sebastianraschka.com/rss_feed.xml` ✅ | LLMs, coding agents, ML research. |
| Normal Technology (fka AI Snake Oil) | `normaltech.ai` | Substack RSS ✅ | AI reliability, agent critique. 73K subscribers. Arvind Narayanan. |
| MIT Technology Review — AI | `technologyreview.com/topic/artificial-intelligence` | `technologyreview.com/topic/artificial-intelligence/feed` ✅ | Rigorous AI journalism on science, medicine, and societal implications. Soft paywall — free articles accessible. Added 2026-04-26. |
| The Pragmatic Engineer | `newsletter.pragmaticengineer.com` | `newsletter.pragmaticengineer.com/feed` ✅ | Gergely Orosz — how AI tools actually change engineering workflows inside Big Tech. 600K+ engineers. Soft paywall. Added 2026-04-26. |
| The Gradient | `thegradient.pub` | `thegradient.pub/rss/` ✅ | Long-form ML journalism bridging research and practice. Interpretability, alignment, ML systems. No paywall. Added 2026-04-26. |
| TechTalks (Ben Dickson) | `bdtechtalks.substack.com` | `bdtechtalks.substack.com/feed` ✅ | Skeptical analysis of AI claims, business model viability, reliability. Counterpoint alongside Gary Marcus. Added 2026-04-26. |
| The AiEdge Newsletter (Damien Benveniste) | `newsletter.theaiedge.io` | `newsletter.theaiedge.io/feed` ✅ | ML system design, MLOps, LLM application architecture — the engineering practitioner layer. Added 2026-04-26. |
| AI Supremacy (Michael Spencer) | `ai-supremacy.com` | `ai-supremacy.com/feed` ✅ | High-velocity AI business/startup intelligence: M&A, funding, partnerships, market positioning. Added 2026-04-26. |
| Andrej Karpathy Blog | `karpathy.github.io` | `karpathy.github.io/feed.xml` ✅ | Monthly-ish. Each post shapes community focus for weeks. Added 2026-04-26. |
| NLP News (Sebastian Ruder, DeepMind) | `newsletter.ruder.io` | `newsletter.ruder.io/feed` ✅ | Monthly NLP/multilingual model research highlights with commentary. Added 2026-04-26. |
| LessWrong (AI content, high-karma filter) | `lesswrong.com` | Use Tavily: `site:lesswrong.com AI safety OR LLM capabilities after:{SEARCH_FROM}` | Daily community research. Filter via Tavily (full RSS is too noisy). Added 2026-04-26. |
| UK AI Security Institute Blog | `aisi.gov.uk/blog` | No RSS — check directly + Tavily: `site:aisi.gov.uk after:{SEARCH_FROM}` | UK government primary source for frontier AI evaluations. Only gov source publishing model evals pre-deployment. Added 2026-04-26. |
| Stanford HAI | `hai.stanford.edu/news` | No RSS — Tavily: `site:hai.stanford.edu after:{SEARCH_FROM}` | Stanford Human-Centered AI: annual AI Index Report, policy briefs, governance commentary. Academic primary. Added 2026-04-26. |
| NVIDIA Technical Blog | `developer.nvidia.com/blog` | `developer.nvidia.com/blog/feed/` ✅ | Developer-facing: new GPU features, inference optimization, model partnership integrations. Distinct from NVIDIA Newsroom. Added 2026-04-26. |
| Microsoft Research Blog | `microsoft.com/en-us/research/blog` | `microsoft.com/en-us/research/blog/feed/` ✅ | AutoAdapt, Phi models, reasoning research, multi-agent work. Feeds Copilot/Azure AI. Added 2026-04-26. |
| EleutherAI Blog | `blog.eleuther.ai` | No RSS — check directly | OSS safety/alignment research: Common Pile, reward hacking, mechanistic interpretability. More research-focused than HF blog. Added 2026-04-26. |
| a16z AI | `a16z.com/ai` | No RSS — Tavily: `site:a16z.com AI after:{SEARCH_FROM}` | a16z AI-specific market theses, portfolio analyses, infra investment rationale. Added 2026-04-26. |
| TechNode | `technode.com` | `technode.com/feed/` ✅ | Premier English-language source for Chinese tech ecosystem AI coverage. News-speed. Added 2026-04-26. |

### Benchmarks & Evaluations

| Source | URL | RSS / Monitor | Notes |
|--------|-----|---------------|-------|
| Scale AI Leaderboards | `labs.scale.com/leaderboard` | Live dashboard — check directly | Independent frontier model evaluations from Scale AI. Task-specific performance. No RSS. |
| Vellum AI LLM Leaderboard | `vellum.ai/llm-leaderboard` | Live dashboard — check directly | Aggregated model quality scores across tasks: reasoning, math, code. Updated continuously. No RSS. |

### News & Media

| Source | URL | RSS | Tavily Query |
|--------|-----|-----|-------------|
| The Verge (AI) | `theverge.com/ai-artificial-intelligence` | `theverge.com/rss/ai-artificial-intelligence/index.xml` ✅ | `site:theverge.com AI model release OR funding OR regulation after:{SEARCH_FROM}` |
| VentureBeat (AI) | `venturebeat.com/category/ai` | `venturebeat.com/category/ai/feed/` ✅ | `site:venturebeat.com AI model release OR infrastructure OR funding after:{SEARCH_FROM}` |
| TechCrunch (AI) | `techcrunch.com/tag/artificial-intelligence` | `techcrunch.com/tag/artificial-intelligence/feed/` ✅ | `site:techcrunch.com AI model OR startup OR funding after:{SEARCH_FROM}` |
| Wired (AI) | `wired.com/feed/tag/ai/latest/rss` | `wired.com/feed/tag/ai/latest/rss` ✅ | `site:wired.com AI OR LLM OR generative after:{SEARCH_FROM}` |
| The Algorithmic Bridge | `thealgorithmicbridge.com` | `thealgorithmicbridge.substack.com/feed` ✅ | `site:thealgorithmicbridge.com model release OR AI analysis after:{SEARCH_FROM}` |
| The Information | `theinformation.com` | Paywalled — Tavily only | `site:theinformation.com AI OR OpenAI OR Anthropic OR model after:{SEARCH_FROM}` |

### Neo-Cloud & Infrastructure Providers

| Source | URL | RSS | Notes |
|--------|-----|-----|-------|
| Together AI Blog | `together.ai/blog` | `together.ai/blog/rss.xml` ✅ | Model availability, pricing, benchmarks across hosted models |
| Lambda (lambdalabs.com) | `lambda.ai/blog` | No RSS (✅ site verified, now lambda.ai) | GPU benchmarks, MLPerf results, inference pricing |
| Fireworks AI Blog | `fireworks.ai/blog` | No RSS (✅ site verified) | Inference performance, model availability, pricing |
| Groq Blog | `groq.com/blog` | No RSS (✅ site verified) | Speed benchmarks, LPU performance, model pricing |
| Cerebras Blog | `cerebras.ai/blog` | No RSS (✅ site verified) | Inference speed records, wafer-scale chip updates |
| Nebius Blog | `nebius.com/blog` | No RSS (✅ site verified) | EU/EMEA AI cloud, model hosting, pricing |
| CoreWeave | `coreweave.com` | No RSS — Tavily only | `"CoreWeave" GPU pricing OR capacity OR model serving after:{SEARCH_FROM}` |

### Open Source & Community

| Source | URL | RSS | Notes |
|--------|-----|-----|-------|
| arXiv (cs.AI / cs.LG / cs.CL) | `arxiv.org` | No direct RSS by subcategory | Query: `site:arxiv.org LLM benchmark model release 2026 after:{SEARCH_FROM}` |

### AWS & Bedrock

| Source | URL | RSS / Monitor | Notes |
|--------|-----|---------------|-------|
| AWS Machine Learning Blog | `aws.amazon.com/blogs/machine-learning` | `aws.amazon.com/blogs/machine-learning/feed/` ✅ | Official AWS ML/AI blog. Bedrock launches, SageMaker features, architecture patterns. |
| AWS What's New (AI) | `aws.amazon.com/about-aws/whats-new` | RSS ✅ (filtered) | Service-level announcements. Filter to Bedrock, SageMaker, Q Developer. |
| Bedrock Pricing | `aws.amazon.com/bedrock/pricing` | No RSS — check directly | Official pricing page. Compare on-demand vs provisioned throughput. |
| Amazon Q Developer | `aws.amazon.com/q/developer` | No RSS — AWS blog | Coding agent features, IDE integrations, CLI updates. |
| Amazon QuickSight Q | `aws.amazon.com/quicksight` | No RSS — AWS blog | AI-powered BI and natural language querying. Track GenAI features. |
| Kiro IDE | `kiro.dev` | No RSS — Tavily | AWS AI-native IDE. Track releases, spec-driven development features. |
| AWS Neuron SDK | `github.com/aws-neuron` | GitHub releases | Trainium/Inferentia compiler + runtime. Track for self-hosting model support. |
| AWS Open Source Blog | `aws.amazon.com/blogs/opensource` | RSS ✅ | Strands SDK, OSS contributions, framework integrations. |
| Jeff Barr's Blog | `jeff-barr.com` | No RSS — Tavily | Former AWS Chief Evangelist. Still covers major AWS AI launches. |

### Agentic Tools & Frameworks

| Source | URL | RSS / Monitor | Notes |
|--------|-----|---------------|-------|
| **Coding Agents** | | | |
| Claude Code (Anthropic) | `docs.anthropic.com/en/docs/claude-code` | No RSS — Anthropic news + npm | Anthropic's coding agent CLI. |
| Codex CLI (OpenAI) | `github.com/openai/codex` | `github.com/openai/codex/releases.atom` ✅ | ~73K stars. OpenAI's terminal coding agent. |
| Cursor | `cursor.com/blog` | No RSS — Tavily | AI-native IDE. Track releases + changelog. |
| Windsurf (Codeium) | `codeium.com/blog` | No RSS — Tavily | AI IDE. Track releases and pricing. |
| Aider | `aider.chat` | `github.com/Aider-AI/aider/releases.atom` ✅ | ~43K stars. OSS coding agent + LLM Leaderboard. |
| Cline | `github.com/cline/cline` | `github.com/cline/cline/releases.atom` ✅ | ~60K stars. OSS VS Code agent extension. |
| Devin (Cognition) | `cognition.ai` | No RSS — `cognition.ai/blog` | Commercial autonomous coding agent. |
| OpenHands | `all-hands.dev` | `github.com/OpenHands/OpenHands/releases.atom` ✅ | ~71K stars. OSS autonomous agent. SWE-bench contender. |
| SWE-agent (Princeton) | `swe-agent.com` | `github.com/SWE-agent/SWE-agent/releases.atom` ✅ | ~19K stars. Research agent. |
| Amazon Q Developer | `aws.amazon.com/q/developer` | No RSS — AWS blog | AWS coding agent. |
| GitHub Copilot | `github.blog` | `github.blog/tag/github-copilot/feed/` ✅ | Copilot agent mode + Workspace. |
| Goose (Block) | `block.github.io/goose` | `github.com/block/goose/releases.atom` ✅ | ~36K stars. OSS extensible AI agent. |
| Continue | `continue.dev` | `github.com/continuedev/continue/releases.atom` ✅ | ~32K stars. OSS IDE extension. |
| Augment Code | `augmentcode.com` | No RSS — `augmentcode.com/blog` | IDE agents + benchmarks. |
| Tabby | `tabby.tabbyml.com` | `github.com/TabbyML/tabby/releases.atom` ✅ | ~33K stars. Self-hosted coding assistant. |
| **Orchestration & Platforms** | | | |
| OpenClaw | `github.com/openclaw/openclaw` | GitHub releases | ~348K stars. Personal AI agent orchestration. |
| n8n | `n8n.io` | `github.com/n8n-io/n8n/releases.atom` ✅ | ~182K stars. Workflow automation + AI agents. |
| Dify | `dify.ai` | `github.com/langgenius/dify/releases.atom` ✅ | ~136K stars. Agentic workflow platform. |
| LangGraph (LangChain) | `blog.langchain.dev` | `blog.langchain.dev/rss/` ✅ | ~28K stars. Graph-based agent orchestration. |
| CrewAI | `crewai.com` | `github.com/crewAIInc/crewAI/releases.atom` ✅ | ~48K stars. Multi-agent orchestration. |
| AutoGen (Microsoft) | `github.com/microsoft/autogen` | `github.com/microsoft/autogen/releases.atom` ✅ | ~57K stars. Multi-agent framework (AG2, Magentic-One). |
| smolagents (HuggingFace) | `huggingface.co/docs/smolagents` | `github.com/huggingface/smolagents/releases.atom` ✅ | ~26K stars. Code-first agent framework. |
| Strands SDK (AWS) | `github.com/strands-agents/sdk-python` | GitHub releases | ~6K stars. AWS open-source agent SDK. |
| Google ADK | `google.github.io/adk-docs` | `github.com/google/adk-python/releases.atom` ✅ | ~19K stars. Google's agent framework + A2A. |
| Semantic Kernel (Microsoft) | `learn.microsoft.com/semantic-kernel` | `github.com/microsoft/semantic-kernel/releases.atom` ✅ | ~28K stars. Enterprise agent SDK. |
| FlowiseAI | `flowiseai.com` | `github.com/FlowiseAI/Flowise/releases.atom` ✅ | ~52K stars. Visual AI agent builder. Acquired by Workday. |
| **Protocols** | | | |
| MCP (Model Context Protocol) | `modelcontextprotocol.io` | No RSS — track spec + servers repo (~83K stars) | De facto standard for agent-tool integration. |
| A2A (Google) | `google.github.io/A2A` | GitHub — track spec changes | ~23K stars. Agent-to-Agent protocol. v1.0.0 March 2026. |
| **Benchmarks** | | | |
| SWE-bench | `swebench.com` | No RSS — check directly | Ground truth for coding agent eval. Verified/Multilingual/Multimodal variants. |
| WebArena | `webarena.dev` | GitHub | Web agent benchmark. Realistic browser environments. |
| AgentBench | `github.com/THUDM/AgentBench` | GitHub | ~3.3K stars. LLM-as-agent across 8 environments. ICLR'24. |
| tau-bench | `github.com/sierra-research/tau-bench` | GitHub | ~1.2K stars. Task-specific agent eval. Sierra Research. |
| **Infra & Observability** | | | |
| E2B | `e2b.dev` | `github.com/e2b-dev/e2b/releases.atom` ✅ | ~12K stars. Agent sandboxing (Firecracker microVMs). |
| Daytona | `daytona.io` | `github.com/daytonaio/daytona/releases.atom` ✅ | ~71K stars. AI-ready dev environments. |
| Stagehand (Browserbase) | `stagehand.dev` | `github.com/browserbase/stagehand/releases.atom` ✅ | ~22K stars. Browser automation for AI. |
| Langfuse | `langfuse.com` | `github.com/langfuse/langfuse/releases.atom` ✅ | ~24K stars. OSS LLM observability + tracing. |
| Arize Phoenix | `arize.com` | `github.com/Arize-ai/phoenix/releases.atom` ✅ | ~9K stars. OSS LLM observability. OpenTelemetry. |
| Helicone | `helicone.ai` | `github.com/Helicone/helicone/releases.atom` ✅ | ~5K stars. AI gateway + agent tracing. |

### Enterprise AI Platforms & Dev Infra (Added 2026-04-26 Round 2)

| Source | URL | RSS / Monitor | Notes |
|--------|-----|---------------|-------|
| Databricks Blog | `databricks.com/blog` | `databricks.com/feed` ✅ | Daily-active. Unity AI Gateway, Mosaic AI, DBRX, LLM agent optimization. Enterprise AI data platform patterns. |
| Azure AI Blog | `azure.microsoft.com/en-us/blog/tag/ai` | `azure.microsoft.com/en-us/blog/tag/ai/feed/` ✅ | Azure AI Studio, Copilot enterprise deployment, AI governance. Distinct from Microsoft Research Blog. |
| Google Cloud AI Blog | `cloud.google.com/blog/topics/ai-machine-learning` | No RSS — Tavily: `site:cloud.google.com/blog AI machine-learning Vertex Gemini after:{SEARCH_FROM}` | Vertex AI releases, TPU availability, enterprise Gemini API patterns. Distinct from DeepMind (research) and Google Blog (consumer). |
| Replit Blog | `blog.replit.com` | `blog.replit.com/feed.xml` ✅ | Daily. Replit Agent 4, vibe-coding architecture, $400M raise at $9B (Mar 2026). 50M+ users. Engineering depth, not PR. |
| LlamaIndex Blog | `llamaindex.ai/blog` | No RSS — Tavily: `LlamaIndex LlamaParse OR LlamaAgents OR RAG release after:{SEARCH_FROM}` | Pivoted to agentic document AI (ParseBench, LiteParse). LangChain competitor; distinct angle on document-AI. |
| Sakana AI | `sakana.ai` | No RSS — check directly | Ex-Google Brain research lab (Japan). Fugu multi-agent FM, AI Scientist (Nature peer-reviewed), Darwin Gödel Machine. Strong English content. |
| ElevenLabs Blog | `elevenlabs.io/blog` | No RSS — Tavily: `ElevenLabs voice model OR agent OR API after:{SEARCH_FROM}` | Eleven v3, ElevenAgents enterprise deployments. Series D at $11B. Voice AI application layer. |
| Weaviate Blog | `weaviate.io/blog` | `weaviate.io/blog/rss.xml` ✅ | v1.37 ships built-in MCP server. MMR search, vector DB architecture, hybrid search. OSS. |
| Pinecone Blog | `pinecone.io/blog` | No RSS — Tavily: `site:pinecone.io/blog after:{SEARCH_FROM}` | Dedicated Read Nodes GA, load balancing AI services. Vector DB architecture + scalability. |

### Analysts, Investors & Podcasts (Added 2026-04-26 Round 3)

| Source | URL | RSS / Monitor | Notes |
|--------|-----|---------------|-------|
| Exponential View (Azeem Azhar) | `exponentialview.co` | `exponentialview.substack.com/feed` ✅ | 152K subscribers. Weekly macro-exponential-tech analysis. Fills global/societal macro-dynamics gap between Benedict Evans (adoption curves) and Stratechery (platform economics). Posted Apr 26. |
| Cognitive Revolution (Labenz + Torenberg) | `cognitiverevolution.substack.com` | `cognitiverevolution.substack.com/feed` ✅ | Weekly AI podcast + newsletter. Builder interviews + model capability deep-dives. Labenz on capabilities/deployment; Torenberg (Turpentine VC). Apr 26 episode: "Looking Up The AI Exponential with Azeem Azhar". |
| From the Trenches (Denis Stetskov / @razoorka) | `techtrenches.dev` | `techtrenches.dev/feed` ✅ | **Added 2026-05-10 by operator.** Engineering Manager, 2.1K+ subs. Weekly. Skeptical/pragmatist voice on AI hype vs. reality, business model viability, and lab claims-vs-data. GenAI-relevant posts: "I Was Wrong About Anthropic" (Apr 28 — Anthropic CPO Figma incident, Claude target-picking in Israel), "Big Tech's $364B Hypothesis Meets the $650B Reality" (Feb 9 — capex curve, Amazon debt raise prospect), "The Country of Geniuses That Doesn't Exist" (Feb 17 — Anthropic CEO 50% white-collar prediction vs. internal team survey 0/16 believing Claude can replace entry-level researchers), "The Comprehension Extinction" (Mar 2 — 54K AI-cited layoffs analysis), "AI's Announcement Problem" (Mar 16 — conference claims vs. production data). Counterpoint sourced for Lab Critique / Industry Moves / Tension sections. Tier 2 candidate — promote to T2 confirmed after first edition cites him. Query: `site:techtrenches.dev after:{SEARCH_FROM}`. |

---

## Tier 3 — Quarterly / Major Releases Only

### Compute & Infrastructure Analysts

| Source | URL | Notes |
|--------|-----|-------|
| Weights & Biases (W&B) Blog | `wandb.ai/fully-connected` | No RSS confirmed. Model training insights, MLOps, evaluation patterns. Useful for tooling signals. |

### Video / Podcast

| Source | URL | Notes |
|--------|-----|-------|
| AI Explained (YouTube) | `youtube.com/@aiexplained-official` | Video analysis of major releases. No RSS. Check when major model drops. |
| Matthew Berman (YouTube) | `youtube.com/@matthew_berman` | Model release demos, reviews, feature breakdowns. No RSS. Check when major model drops. |
| TheAIGRID (YouTube) | `youtube.com/@TheAIGRID` | High-frequency AI news video summaries. Broad coverage, good for catching things fast. No RSS. |
| Situational Awareness (Leopold Aschenbrenner) | `situational-awareness.ai` | RSS ✅ `situational-awareness.ai/feed/`. Long-form essay-style analysis on AGI timelines, national security, AI race dynamics. Infrequent but highly cited. |

### Safety Orgs & Policy Primaries (Added 2026-04-26)

| Source | URL | Notes |
|--------|-----|-------|
| Center for AI Safety Blog | `safe.ai/blog` | No RSS — check directly. Dan Hendrycks. Research + landmark surveys (e.g., "Humanity's Last Exam"). Infrequent but high-impact. |
| OECD AI Policy Observatory | `oecd.ai` | No RSS — Tavily: `site:oecd.ai policy brief OR report after:{SEARCH_FROM}`. International AI policy across 46 member countries. |
| US AI.gov | `ai.gov` | No RSS — check directly. Official US admin AI strategy. Low frequency but essential for US regulatory direction. |

### Practitioner Deep-Dives (Added 2026-04-26)

| Source | URL | Notes |
|--------|-----|-------|
| Hamel Husain Blog | `hamel.dev` | No RSS — check directly. LLM evaluation engineering in practice: what evals work, which tools, when RAG is/isn't dead. Practitioner angle. |
| Anthropic Transformer Circuits | `transformer-circuits.pub` | No RSS — check directly. Canonical mech-interp research from Anthropic. Infrequent, each release redefines interpretability conversations. |

### Open Model Hubs & Infra (Added 2026-04-26)

| Source | URL | Notes |
|--------|-----|-------|
| vLLM Blog | `vllm.ai/blog` | No RSS — check directly. GitHub releases: `github.com/vllm-project/vllm/releases.atom` ✅. Dominant OSS inference engine. Performance benchmarks, speculative decoding, multi-modal serving. |
| PapersWithCode | `paperswithcode.com` | No RSS — Tavily: `site:paperswithcode.com benchmark OR state-of-the-art after:{SEARCH_FROM}`. SOTA tracking across all ML tasks. |
| ModelScope (Alibaba hub) | `modelscope.cn` | No RSS — Tavily: `site:modelscope.cn model release OR benchmark after:{SEARCH_FROM}`. Chinese-market HF equivalent. Qwen + Asia-Pacific releases. Partial English. |

### Regional Coverage (Experimental — Added 2026-04-26)

Run 2 editions with all 4 sources, then cull the 2 that generate fewest unique stories.

| Source | URL | Notes |
|--------|-----|-------|
| Analytics India Magazine | `analyticsindiamag.com` | Tavily: `site:analyticsindiamag.com AI model OR LLM after:{SEARCH_FROM}`. India AI ecosystem: Sarvam AI, Krutrim, TCS AI, IndiaAI Mission. |
| Geektime | `geektime.com` | Tavily: `site:geektime.com AI after:{SEARCH_FROM}`. English-language Israeli tech press. AI21 Labs, Lightricks, Mobileye AI. |
| e27 | `e27.co` | Tavily: `site:e27.co AI after:{SEARCH_FROM}`. Southeast Asia/Singapore AI: sovereign wealth funds, PDPC/IMDA policy. |
| Synced Global | `syncedreview.com` | No RSS confirmed — verify freshness. English ML/AI journalism focused on China/global research. |

### Enterprise Data Platforms & Investor Essays (Added 2026-04-26 Round 2)

| Source | URL | Notes |
|--------|-----|-------|
| Snowflake AI Blog | `snowflake.com/en/blog` | No RSS — Tavily: `Snowflake Cortex OR Arctic OR "Snowflake AI" model after:{SEARCH_FROM}`. Cortex AI + Arctic model family. Lower cadence than Databricks but strategically important. |
| Eugene Yan | `eugeneyan.com` | RSS ✅ `eugeneyan.com/rss/`. Anthropic MTS. LLM evals, LLM-as-judge, product evals. Bi-monthly but deep. |
| Every (Dan Shipper et al.) | `every.to` | No RSS — Tavily: `site:every.to AI after:{SEARCH_FROM}`. AI strategy + building with LLMs essays. Soft paywall. |
| Sequoia Capital AI | `sequoiacap.com/stories` | No RSS — Tavily: `"Sequoia" AI essay OR report after:{SEARCH_FROM}`. AI market theses, revenue benchmarks. Quarterly, high-impact. |
| Perplexity AI Blog | `perplexity.ai/hub/blog` | Cloudflare-blocked — Tavily-only: `site:perplexity.ai/hub after:{SEARCH_FROM}` + `"Perplexity AI" (search OR agent OR pricing OR model OR Sonar) after:{SEARCH_FROM}`. Answer-engine patterns, Sonar API, enterprise deployments. |
| Runway Research | `runwayml.com/news` | No RSS — Tavily: `Runway ML Gen OR GWM model release after:{SEARCH_FROM}`. Video AI / world model research. Quarterly cadence, major release only. |
| AMD ROCm / Instinct | `amd.com/en/developer/resources/amd-ai.html` | No RSS — Tavily: `AMD Instinct OR ROCm LLM OR inference benchmark after:{SEARCH_FROM}`. MI300X + ROCm 6.x traction. #2 AI GPU after NVIDIA. |

### VC Primary Research & Case Studies (Added 2026-04-26 Round 3)

| Source | URL | Notes |
|--------|-----|-------|
| Menlo Ventures Perspective | `menlovc.com/perspective` | RSS ✅ `menlovc.com/feed/`. Monthly perspectives + annual **State of Generative AI in the Enterprise** (500+ decision-makers, $37B enterprise AI spend 2025, 3.2x YoY). Also State of Consumer AI + State of AI in Healthcare. Anthology Fund = Anthropic investor. Data-driven, not pitch-deck. |
| Bessemer Atlas (AI & ML) | `bvp.com/atlas` | No RSS — Tavily: `site:bvp.com/atlas AI OR "machine learning" after:{SEARCH_FROM}`. Weekly AI content. "Bessemer Predicts: Robotics and Physical AI" (partner forecasts + market sizing). Portco case studies. **Round 2 rejection reversed** — Round 2 checked wrong URL (bvp.com main site, not /atlas). |

### Chinese Labs (English via HuggingFace + Tavily)

| Source | URL | Notes |
|--------|-----|-------|
| Qwen Blog (Alibaba) | `qwen.ai/blog` | No RSS confirmed. Chinese model releases, technical reports. |
| DeepSeek | `deepseek.com` | No blog RSS. Releases via HuggingFace and arXiv. |
| GLM / Zhipu AI | Via HuggingFace + Tavily | `"Zhipu" OR "GLM" model release benchmark after:{SEARCH_FROM}` |
| MiniMax | Via HuggingFace + Tavily | `"MiniMax" OR "MiniMax-AI" model release after:{SEARCH_FROM}` |
| Yi / 01.AI | Via HuggingFace + Tavily | `"01.AI" OR "Yi model" release after:{SEARCH_FROM}` |
| Baichuan | Via HuggingFace + Tavily | `"Baichuan" model release after:{SEARCH_FROM}` |
| SenseTime | Via Tavily | `"SenseTime" OR "SenseNova" AI model after:{SEARCH_FROM}` |
| Moonshot AI (Kimi) | `kimi.com/blog` | No RSS confirmed — check directly + Tavily: `site:kimi.com/blog OR "Moonshot AI" model release after:{SEARCH_FROM}`. Verified active 2026-04-20. English content. |
| Mistral Blog | `mistral.ai/news` | No RSS. Direct Tavily: `site:mistral.ai model release after:{SEARCH_FROM}` |
| Cohere Blog | `cohere.com/blog` | No RSS. Tavily: `site:cohere.com model OR API OR pricing after:{SEARCH_FROM}` |

---

## Tavily Query Bank

### Model Releases (Run Every Week)
```
new AI model release announcement after:{SEARCH_FROM}
Claude OR GPT OR Gemini OR Llama new model release after:{SEARCH_FROM}
Qwen OR DeepSeek OR Mistral OR Cohere model release after:{SEARCH_FROM}
Chinese AI model release benchmark English after:{SEARCH_FROM}
site:huggingface.co/blog model release after:{SEARCH_FROM}
```

### Benchmarks & Rankings
```
LMSYS chatbot arena leaderboard ELO ranking update after:{SEARCH_FROM}
SWE-bench OR HumanEval OR MMLU OR GPQA benchmark results after:{SEARCH_FROM}
openrouter.ai rankings most used models after:{SEARCH_FROM}
artificial analysis LLM benchmark speed pricing after:{SEARCH_FROM}
open LLM leaderboard huggingface update after:{SEARCH_FROM}
site:epochai.org compute trend training cost model database FrontierMath after:{SEARCH_FROM}
```

### Multi-Modality
```
multimodal AI model vision audio video release after:{SEARCH_FROM}
image understanding video generation voice model release after:{SEARCH_FROM}
"unified multimodal" OR "vision language" model release after:{SEARCH_FROM}
```

### Specialized / Domain Models
```
code model medical AI legal AI finance model release after:{SEARCH_FROM}
domain-specific LLM fine-tuned vertical release after:{SEARCH_FROM}
```

### Pricing & Infrastructure
```
AI API pricing cut OR price reduction OR cheaper inference after:{SEARCH_FROM}
GPU compute pricing Nvidia H100 OR H200 OR B200 OR Blackwell availability after:{SEARCH_FROM}
vLLM OR Ollama OR SGLang inference serving update after:{SEARCH_FROM}
self-hosting LLM cost economics local inference after:{SEARCH_FROM}
CoreWeave OR Nebius OR Lambda OR Together OR Fireworks OR Groq OR Cerebras pricing model after:{SEARCH_FROM}
```

### Industry Moves
```
AI startup funding acquisition partnership after:{SEARCH_FROM}
OpenAI OR Anthropic OR Google OR Meta OR Microsoft AI strategy announcement after:{SEARCH_FROM}
AI open source licensing change OR model weights released after:{SEARCH_FROM}
```

### Chinese Ecosystem (English-language sources)
```
DeepSeek OR Qwen OR "GLM" OR MiniMax AI model release English after:{SEARCH_FROM}
Chinese AI model benchmark Western comparison after:{SEARCH_FROM}
Alibaba OR ByteDance OR Baidu AI model announcement English after:{SEARCH_FROM}
site:huggingface.co DeepSeek OR Qwen OR GLM OR MiniMax after:{SEARCH_FROM}
```

### Regulation & Safety
```
EU AI Act OR US AI regulation OR China AI policy development after:{SEARCH_FROM}
AI safety research alignment interpretability paper after:{SEARCH_FROM}
AI incident failure copyright lawsuit after:{SEARCH_FROM}
site:alignmentforum.org interpretability OR alignment OR safety after:{SEARCH_FROM}
site:lesswrong.com AI safety OR LLM capabilities after:{SEARCH_FROM}
site:aisi.gov.uk after:{SEARCH_FROM}
site:hai.stanford.edu after:{SEARCH_FROM}
site:oecd.ai policy brief OR report after:{SEARCH_FROM}
```

### Open Source & Community
```
LangChain OR LlamaIndex OR CrewAI OR Strands OR smolagents release update after:{SEARCH_FROM}
open source LLM model release weights Apache MIT license after:{SEARCH_FROM}
AI developer community sentiment tools workflow after:{SEARCH_FROM}
```

### Agentic Tools & Frameworks
```
Claude Code OR Codex CLI OR Cursor OR Windsurf OR Aider new release update after:{SEARCH_FROM}
AI coding agent benchmark SWE-bench leaderboard results after:{SEARCH_FROM}
MCP "Model Context Protocol" OR A2A "Agent-to-Agent" protocol update after:{SEARCH_FROM}
LangGraph OR CrewAI OR AutoGen OR Strands SDK OR smolagents agent framework after:{SEARCH_FROM}
Devin OR OpenHands OR SWE-agent OR "Amazon Q Developer" coding agent after:{SEARCH_FROM}
OpenClaw OR n8n OR "agent orchestration" OR "agent runtime" self-host after:{SEARCH_FROM}
GitHub Copilot Workspace OR Cline OR "AI IDE" coding assistant after:{SEARCH_FROM}
```

### AWS & Bedrock
```
AWS Bedrock new model OR pricing change OR feature after:{SEARCH_FROM}
Amazon SageMaker AI OR JumpStart OR HyperPod generative AI update after:{SEARCH_FROM}
Amazon Q Developer OR Kiro IDE OR Amazon QuickSight AI update after:{SEARCH_FROM}
AWS Trainium OR Inferentia OR Neuron SDK update after:{SEARCH_FROM}
site:aws.amazon.com/blogs/machine-learning after:{SEARCH_FROM}
AWS re:Invent OR AWS Summit AI announcement after:{SEARCH_FROM}
Bedrock guardrails OR knowledge bases OR agents feature after:{SEARCH_FROM}
```

### Source Discovery
```
AI analyst newsletter independent GenAI landscape weekly after:{SEARCH_FROM}
site:arxiv.org LLM benchmark evaluation model release 2026 after:{SEARCH_FROM}
AI researcher practitioner blog model comparison analysis after:{SEARCH_FROM}
```

### Analysts & Commentators (Specific)
```
site:importai.substack.com after:{SEARCH_FROM}
site:thezvi.substack.com AI after:{SEARCH_FROM}
site:oneusefulthing.org AI after:{SEARCH_FROM}
site:garymarcus.substack.com after:{SEARCH_FROM}
site:stratechery.com AI after:{SEARCH_FROM}
```

### Benchmarks & Evaluations
```
aider.chat leaderboard code benchmark update after:{SEARCH_FROM}
livebench.ai score update model after:{SEARCH_FROM}
scale AI leaderboard evaluation frontier model after:{SEARCH_FROM}
vellum LLM leaderboard model ranking after:{SEARCH_FROM}
```

### Chinese Ecosystem (via ChinAI & Tavily)
```
site:chinai.substack.com after:{SEARCH_FROM}
ChinAI Jeff Ding Chinese AI analysis translation after:{SEARCH_FROM}
```

### Enterprise AI Platforms & Dev Infra (Added 2026-04-26 Round 2)
```
site:databricks.com/blog AI OR LLM OR model after:{SEARCH_FROM}
site:cloud.google.com/blog AI machine-learning Vertex Gemini after:{SEARCH_FROM}
Snowflake Cortex OR Arctic OR "Snowflake AI" model after:{SEARCH_FROM}
site:blog.replit.com AI OR agent after:{SEARCH_FROM}
LlamaIndex LlamaParse OR LlamaAgents OR RAG release after:{SEARCH_FROM}
Sakana AI research paper OR model after:{SEARCH_FROM}
ElevenLabs voice model OR agent OR API after:{SEARCH_FROM}
site:weaviate.io/blog AI OR MCP OR vector after:{SEARCH_FROM}
site:pinecone.io/blog after:{SEARCH_FROM}
site:perplexity.ai/hub after:{SEARCH_FROM}
"Perplexity AI" (search OR agent OR pricing OR model OR Sonar) after:{SEARCH_FROM}
AMD Instinct OR ROCm LLM OR inference benchmark after:{SEARCH_FROM}
Runway ML Gen OR GWM model release after:{SEARCH_FROM}
site:every.to AI after:{SEARCH_FROM}
"Sequoia" AI essay OR report after:{SEARCH_FROM}
```

### VC Primary Research + Podcasts + Macro Analysis (Added 2026-04-26 Round 3)
```
site:menlovc.com State of AI OR perspective enterprise after:{SEARCH_FROM}
site:bvp.com/atlas AI OR "machine learning" after:{SEARCH_FROM}
"Bessemer Predicts" OR "Bessemer Atlas" AI after:{SEARCH_FROM}
site:exponentialview.co AI OR exponential after:{SEARCH_FROM}
"Cognitive Revolution" podcast AI episode after:{SEARCH_FROM}
```

---

## Chinese Ecosystem — Tracking Strategy

No reliable English RSS feeds exist for most Chinese labs. Use this multi-layer approach:

1. **Hugging Face as primary aggregator** — most Chinese lab releases (Qwen, DeepSeek, GLM, MiniMax) post model cards and blog posts on HuggingFace. Check the Open LLM Leaderboard and trending models.
2. **ChinAI Newsletter** (`chinai.substack.com`) — Jeff Ding's translations of Chinese AI research and policy. RSS ✅. Best single English-language source for China AI ecosystem analysis.
3. **Tavily queries** — weekly sweep with English-language queries (see query bank above).
4. **Simon Willison** — routinely covers Chinese model releases when they matter.
5. **arXiv** — technical papers for major releases often appear here first.
6. **The Verge / TechCrunch** — cover major Chinese releases when they have Western impact.

**Labs to track:** DeepSeek (deepseek.com), Qwen/Alibaba (qwen.ai), GLM/Zhipu AI, MiniMax, Yi/01.AI, Baichuan, SenseTime (SenseNova), Moonshot AI (Kimi), ByteDance (Doubao/ERNIE).

---

## Neo-Cloud Provider Tracking Strategy

These providers are signal-rich for: (a) which frontier models are available via API, (b) inference pricing wars, (c) speed/throughput benchmarks.

**Key data points to capture each run:**
- New models available via API (often before or simultaneously with lab announcements)
- Pricing changes ($/1M input + output tokens)
- Speed benchmarks (tokens/sec, TTFT)
- GPU availability (H100/H200/B200/Blackwell pool size announcements)

**Artificial Analysis** (`artificialanalysis.ai/leaderboards/models`) aggregates pricing and speed benchmarks across all major providers — check this instead of visiting each provider individually for comparative data.

---

## Notes

- All Tavily queries append `after:{SEARCH_FROM}` (7 days)
- **Promote to Tier 1** if: verified as consistently high-signal over 2+ editions
- **Demote from Tier 1** if: 4+ consecutive runs with no relevant results
- **Artificial Analysis** is the single best source for cross-provider pricing/speed — check weekly
- **OpenRouter rankings** is ground truth for adoption (actual usage beats announcement buzz)
- **No RSS** entries require direct Tavily query or manual check — note in run log if URL changed

---

## Discovered Sources Log

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| 2026-04-03 | OpenAI News (openai.com/news) | Tier 1 | RSS ✅ openai.com/news/rss.xml |
| 2026-04-03 | Anthropic News (anthropic.com/news) | Tier 1 | No RSS. Direct check + Tavily. |
| 2026-04-03 | Google DeepMind Blog (deepmind.google/blog) | Tier 1 | RSS ✅ deepmind.google/blog/rss.xml |
| 2026-04-03 | Google Blog / The Keyword (blog.google) | Tier 1 | RSS ✅ blog.google/rss/ — filter to AI/Gemini topics |
| 2026-04-03 | Hugging Face Blog (huggingface.co/blog) | Tier 1 | RSS ✅ huggingface.co/blog/feed.xml — very active, covers all major model releases |
| 2026-04-03 | OpenRouter Rankings (openrouter.ai/rankings) | Tier 1 | Live dashboard. Usage ground truth. No RSS. |
| 2026-04-03 | LMSYS / Chatbot Arena (arena.ai) | Tier 1 | Live ELO rankings. No RSS. Check directly. |
| 2026-04-03 | Artificial Analysis (artificialanalysis.ai) | Tier 1 | Independent benchmarks: quality, speed, $/1M tokens across 100+ models/providers. No RSS (✅ site verified). Best single cross-provider benchmark source. |
| 2026-04-03 | Hugging Face Open LLM Leaderboard | Tier 1 | Best source for open-weight model benchmarks. Live leaderboard. |
| 2026-04-03 | Simon Willison (simonwillison.net) | Tier 1 | RSS ✅ atom/everything/. Daily coverage of every significant LLM release, tool, and community development. Prolific and accurate. |
| 2026-04-03 | SemiAnalysis (semianalysis.com) | Tier 1 | RSS ✅ semianalysis.com/feed/. Dylan Patel's deep semiconductor + AI infrastructure analysis. Best source for GPU economics, training cost, datacenter strategy. |
| 2026-04-03 | Latent Space (latent.space) | Tier 1 | RSS ✅ latent.space/feed. AI engineering community podcast + newsletter. Strong signal on model releases, agentic AI, tooling. |
| 2026-04-03 | The Verge AI (theverge.com) | Tier 2 | RSS ✅ rss/ai-artificial-intelligence/index.xml. High quality AI journalism with impact on industry narrative. |
| 2026-04-03 | VentureBeat AI (venturebeat.com) | Tier 2 | RSS ✅ venturebeat.com/category/ai/feed/. Good for funding, infrastructure, enterprise moves. |
| 2026-04-03 | TechCrunch AI (techcrunch.com) | Tier 2 | RSS ✅ techcrunch.com/tag/artificial-intelligence/feed/. Funding rounds, startup moves. |
| 2026-04-03 | Wired AI (wired.com) | Tier 2 | RSS ✅ wired.com/feed/tag/ai/latest/rss. Deep-dive AI journalism, safety, policy. |
| 2026-04-03 | The Algorithmic Bridge | Tier 2 | RSS ✅ thealgorithmicbridge.substack.com/feed → redirects to thealgorithmicbridge.com/feed. AI analysis newsletter. |
| 2026-04-03 | The Information | Tier 2 | Paywalled. Tavily only. Best for industry scoops (OpenAI internals, lab drama, partnerships before official announcement). |
| 2026-04-03 | Together AI Blog (together.ai/blog) | Tier 2 | RSS ✅ together.ai/blog/rss.xml. Model availability, pricing, open-source model inference. |
| 2026-04-03 | Lambda / Lambda Labs (lambda.ai/blog) | Tier 2 | No RSS. URL is now lambda.ai (not lambdalabs.com). Blog confirmed ✅. MLPerf results, GPU benchmarks, inference pricing. |
| 2026-04-03 | Fireworks AI Blog (fireworks.ai/blog) | Tier 2 | No RSS. Site confirmed ✅. Inference performance, MoE model serving, frontier model availability. |
| 2026-04-03 | Groq Blog (groq.com/blog) | Tier 2 | No RSS. Site confirmed ✅. LPU speed benchmarks, model availability, pricing. |
| 2026-04-03 | Cerebras Blog (cerebras.ai/blog) | Tier 2 | No RSS. Site confirmed ✅. Inference speed records, wafer-scale chip. |
| 2026-04-03 | Nebius Blog (nebius.com/blog) | Tier 2 | No RSS. Site confirmed ✅. EU/EMEA-focused AI cloud, model hosting, pricing. |
| 2026-04-03 | CoreWeave | Tier 2 | No RSS. Tavily only. GPU cloud capacity, Blackwell availability, enterprise contracts. |
| 2026-04-03 | AI Explained (YouTube) | Tier 3 | youtube.com/@aiexplained-official. No RSS. Video-format model analysis. Check when major model drops. |
| 2026-04-03 | Qwen / Alibaba (qwen.ai/blog) | Tier 3 | No RSS. Track via HuggingFace + Tavily. Major OSS model family. |
| 2026-04-03 | DeepSeek (deepseek.com) | Tier 3 | No blog RSS. Releases via HuggingFace and arXiv primarily. Track via HuggingFace + Tavily. |
| 2026-04-03 | GLM / Zhipu AI | Tier 3 | No English RSS. Track via HuggingFace model releases + Tavily. |
| 2026-04-03 | MiniMax | Tier 3 | No English RSS. Track via HuggingFace + Tavily. |
| 2026-04-03 | Yi / 01.AI | Tier 3 | No English RSS. Track via HuggingFace + Tavily. |
| 2026-04-03 | Baichuan | Tier 3 | No English RSS. Track via HuggingFace + Tavily. |
| 2026-04-03 | SenseTime / SenseNova | Tier 3 | No English RSS. Tavily only. |
| 2026-04-03 | Mistral AI (mistral.ai/news) | Tier 3 | No RSS confirmed. Tavily: site:mistral.ai. Significant OSS contributor. |
| 2026-04-03 | Cohere (cohere.com/blog) | Tier 3 | No RSS confirmed. Tavily: site:cohere.com. Enterprise-focused. |

### Added 2026-04-03 (Research Expansion Pass)

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| 2026-04-03 | Import AI — Jack Clark (importai.substack.com) | Tier 1 | RSS ✅ importai.substack.com/feed. Former OpenAI policy. Deep weekly analysis of AI research, safety, geopolitics. High signal, long-running. |
| 2026-04-03 | One Useful Thing — Ethan Mollick (oneusefulthing.org) | Tier 1 | RSS ✅ oneusefulthing.org/feed. Wharton professor. Best source for AI adoption research and practical AI use in real orgs. |
| 2026-04-03 | Don't Worry About the Vase — Zvi Mowshowitz (thezvi.substack.com) | Tier 1 | RSS ✅ thezvi.substack.com/feed. Weekly AI roundups, deep safety + capability analysis. Highly read by AI insiders. |
| 2026-04-03 | Aider LLM Leaderboards (aider.chat/docs/leaderboards/) | Tier 1 | Live dashboard. Real-world coding benchmark (not synthetic). Tracks all frontier models. No RSS. |
| 2026-04-03 | LiveBench (livebench.ai) | Tier 1 | Live dashboard. Contamination-free benchmark refreshed monthly. No RSS. |
| 2026-04-03 | Stratechery — Ben Thompson (stratechery.com) | Tier 2 | RSS ✅ stratechery.com/feed/ (free posts only; mostly paywalled). Best AI strategy and business analysis. |
| 2026-04-03 | Marcus on AI — Gary Marcus (garymarcus.substack.com) | Tier 2 | RSS ✅ garymarcus.substack.com/feed. Prolific AI critic. Essential counterpoint on reliability, benchmark gaming, hype. |
| 2026-04-03 | State of AI Newsletter — Nathan Benaich (nathanbenaich.substack.com) | Tier 2 | RSS ✅ nathanbenaich.substack.com/feed. Annual State of AI Report + newsletter. Air Street Capital. |
| 2026-04-03 | Dwarkesh Podcast (dwarkesh.com) | Tier 2 | RSS ✅ dwarkesh.com/feed. Deep long-form AI interviews. Highly cited when posted. |
| 2026-04-03 | Epoch AI Newsletter — Gradient Updates (epochai.substack.com) | Tier 2 | RSS ✅ epochai.substack.com/feed. Research-backed compute trend analysis. |
| 2026-04-03 | TLDR AI (tldr.tech/ai) | Tier 2 | RSS ✅ tldr.tech/api/rss/ai. 920K readers. Daily digest. Good for catching missed items. |
| 2026-04-03 | Ben's Bites (bensbites.com) | Tier 2 | RSS ✅ bensbites.com/feed. AI startup moves, investments, product releases. |
| 2026-04-03 | The Neuron (theneuron.ai) | Tier 2 | No RSS (✅ site verified). 675K readers. AI tools and trend signals. |
| 2026-04-03 | AlphaSignal (alphasignal.ai) | Tier 2 | No RSS found (✅ site verified, 250K+ AI engineers). Daily frontier papers/repos digest. |
| 2026-04-03 | ChinAI Newsletter — Jeff Ding (chinai.substack.com) | Tier 2 | RSS ✅ chinai.substack.com/feed. 300+ issues. Best English coverage of Chinese AI policy and research. GWU professor. |
| 2026-04-03 | The Batch — DeepLearning.AI / Andrew Ng (deeplearning.ai/the-batch) | Tier 2 | No RSS found. Weekly AI news by Andrew Ng. Bridges research to business. |
| 2026-04-03 | BAIR Blog — Berkeley AI Research (bair.berkeley.edu/blog) | Tier 2 | RSS ✅ bair.berkeley.edu/blog/feed.xml. Technical depth on RL, LLMs, diffusion. UC Berkeley. |
| 2026-04-03 | Scale AI Leaderboards (labs.scale.com/leaderboard) | Tier 2 | Live dashboard. Independent frontier model evaluations. No RSS. |
| 2026-04-03 | Vellum AI LLM Leaderboard (vellum.ai/llm-leaderboard) | Tier 2 | Live dashboard. Quality scores across reasoning, math, code. No RSS. |
| 2026-04-03 | Epoch AI Research (epochai.org) | Tier 3 | No direct RSS found (blog RSS 404). Compute trends, model database, FrontierMath benchmark. Use Substack feed (epochai.substack.com). |
| 2026-04-03 | Weights & Biases Blog (wandb.ai/fully-connected) | Tier 3 | No RSS found. MLOps, training insights, tooling signals. |
| 2026-04-03 | Matthew Berman (YouTube @matthew_berman) | Tier 3 | No RSS. Model release demos and reviews. Check at major model drops. |
| 2026-04-03 | TheAIGRID (YouTube @TheAIGRID) | Tier 3 | No RSS. High-frequency AI news video summaries. |
| 2026-04-03 | Situational Awareness — Leopold Aschenbrenner (situational-awareness.ai) | Tier 3 | RSS ✅ situational-awareness.ai/feed/. Long-form AGI/national security essays. Infrequent but highly cited. |

### Added 2026-04-05 (Agentic Tools & Frameworks Section)

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| 2026-04-05 | Claude Code (Anthropic) | Tier 2 | Anthropic coding agent CLI. Track via Anthropic news + npm releases. |
| 2026-04-05 | Codex CLI (OpenAI) | Tier 2 | OpenAI terminal coding agent. GitHub releases. |
| 2026-04-05 | Cursor (cursor.com) | Tier 2 | AI-native IDE. No RSS. Tavily. |
| 2026-04-05 | Windsurf / Codeium (codeium.com) | Tier 2 | AI IDE. No RSS. Tavily. |
| 2026-04-05 | Aider (aider.chat) | Tier 2 | OSS coding agent + benchmark leaderboard. GitHub releases. |
| 2026-04-05 | Cline (github.com/cline/cline) | Tier 2 | OSS VS Code agent extension. GitHub releases. |
| 2026-04-05 | Devin / Cognition (cognition.ai) | Tier 2 | Commercial autonomous coding agent. Tavily. |
| 2026-04-05 | OpenHands (github.com/All-Hands-AI/OpenHands) | Tier 2 | OSS autonomous agent. SWE-bench contender. GitHub releases. |
| 2026-04-05 | SWE-agent / Princeton (github.com/SWE-agent/SWE-agent) | Tier 2 | Research agent. GitHub releases. |
| 2026-04-05 | Amazon Q Developer (aws.amazon.com/q/developer) | Tier 2 | AWS coding agent. Track via AWS blog. |
| 2026-04-05 | GitHub Copilot (github.blog) | Tier 2 | RSS ✅ github.blog/feed/. Agent mode + Workspace. |
| 2026-04-05 | OpenClaw (github.com/openclaw/openclaw) | Tier 2 | Personal AI agent orchestration. GitHub releases. |
| 2026-04-05 | n8n (n8n.io) | Tier 2 | OSS workflow automation + AI agents. Tavily. |
| 2026-04-05 | LangGraph / LangChain (blog.langchain.dev) | Tier 2 | RSS ✅ blog.langchain.dev/rss/. Agent orchestration. |
| 2026-04-05 | CrewAI (crewai.com) | Tier 2 | Multi-agent orchestration. Tavily. |
| 2026-04-05 | Strands SDK / AWS (github.com/strands-agents/sdk-python) | Tier 2 | AWS open-source agent SDK. GitHub releases. |
| 2026-04-05 | AutoGen / Microsoft (github.com/microsoft/autogen) | Tier 2 | Multi-agent framework. GitHub releases. |
| 2026-04-05 | MCP / Model Context Protocol (modelcontextprotocol.io) | Tier 2 | Anthropic tool/context protocol. Tavily. |

### Added 2026-04-26 (Round 1 Expansion — Safety, Infra, Practitioners, Regional)

Sources proposed by research subagent in `source-expansion-proposal-2026-04-26.md`, reviewed and approved (less EU regulatory per operator's direction). Bug fixes applied: duplicate W&B entry removed; Moonshot AI entry updated to `kimi.com/blog`.

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| 2026-04-26 | The Next Platform (nextplatform.com) | Tier 1 | RSS ✅ nextplatform.com/feed/. Deep HPC/infra analysis distinct from SemiAnalysis. Verified active 2026-04-24. Soft paywall. |
| 2026-04-26 | AI Alignment Forum (alignmentforum.org) | Tier 1 | RSS ✅ alignmentforum.org/feed.xml. Canonical primary venue for technical AI safety research. Community-run. Daily. |
| 2026-04-26 | MIT Technology Review AI (technologyreview.com) | Tier 2 | RSS ✅. Rigorous AI journalism on science/medicine/society. Soft paywall. |
| 2026-04-26 | The Pragmatic Engineer (newsletter.pragmaticengineer.com) | Tier 2 | RSS ✅. Gergely Orosz. 600K+ engineers. AI tools in Big Tech workflows. |
| 2026-04-26 | The Gradient (thegradient.pub) | Tier 2 | RSS ✅ thegradient.pub/rss/. Long-form ML journalism bridging research and practice. |
| 2026-04-26 | TechTalks — Ben Dickson (bdtechtalks.substack.com) | Tier 2 | RSS ✅. Skeptical analysis. Counterpoint alongside Gary Marcus. |
| 2026-04-26 | The AiEdge Newsletter (newsletter.theaiedge.io) | Tier 2 | RSS ✅. ML system design, MLOps, LLM architecture. Damien Benveniste. |
| 2026-04-26 | AI Supremacy (ai-supremacy.com) | Tier 2 | RSS ✅. AI business/startup intelligence. Michael Spencer. |
| 2026-04-26 | Andrej Karpathy Blog (karpathy.github.io) | Tier 2 | RSS ✅ karpathy.github.io/feed.xml. Monthly. microgpt (April 10, 2026). |
| 2026-04-26 | NLP News — Sebastian Ruder (newsletter.ruder.io) | Tier 2 | RSS ✅. DeepMind. Monthly NLP/multilingual research highlights. |
| 2026-04-26 | LessWrong AI content (lesswrong.com) | Tier 2 | Tavily-filtered only (full RSS too noisy). Community research. |
| 2026-04-26 | UK AI Security Institute Blog (aisi.gov.uk/blog) | Tier 2 | No RSS — direct + Tavily. UK government frontier AI evaluations. Only gov source publishing model evals pre-deployment. |
| 2026-04-26 | Stanford HAI (hai.stanford.edu/news) | Tier 2 | No RSS — Tavily. Annual AI Index Report, policy briefs. Academic primary. |
| 2026-04-26 | NVIDIA Technical Blog (developer.nvidia.com/blog) | Tier 2 | RSS ✅. Developer-facing GPU features, inference optimization, model partnerships. Daily. |
| 2026-04-26 | Microsoft Research Blog (microsoft.com/en-us/research/blog) | Tier 2 | RSS ✅. AutoAdapt, Phi models, reasoning, multi-agent. Weekly. |
| 2026-04-26 | EleutherAI Blog (blog.eleuther.ai) | Tier 2 | No RSS. OSS safety/alignment research. Common Pile, reward hacking, mech-interp. |
| 2026-04-26 | a16z AI (a16z.com/ai) | Tier 2 | No confirmed RSS — Tavily. Market theses, portfolio analyses. |
| 2026-04-26 | TechNode (technode.com) | Tier 2 | RSS ✅ technode.com/feed/. Premier English source for Chinese tech/AI ecosystem. |
| 2026-04-26 | Center for AI Safety Blog (safe.ai/blog) | Tier 3 | No RSS. Dan Hendrycks. Infrequent but high-impact. |
| 2026-04-26 | OECD AI Policy Observatory (oecd.ai) | Tier 3 | No RSS — Tavily. International AI policy across 46 countries. |
| 2026-04-26 | US AI.gov (ai.gov) | Tier 3 | No RSS — direct. Official US AI strategy. Low frequency. |
| 2026-04-26 | Hamel Husain Blog (hamel.dev) | Tier 3 | No RSS. LLM eval engineering in practice. |
| 2026-04-26 | Anthropic Transformer Circuits (transformer-circuits.pub) | Tier 3 | No RSS. Canonical mech-interp research. Infrequent. |
| 2026-04-26 | vLLM Blog (vllm.ai/blog) | Tier 3 | GitHub releases RSS ✅. Dominant OSS inference engine. |
| 2026-04-26 | PapersWithCode (paperswithcode.com) | Tier 3 | No RSS — Tavily. SOTA tracking across all ML tasks. |
| 2026-04-26 | ModelScope (modelscope.cn) | Tier 3 | No RSS — Tavily. Alibaba hub, Chinese-market HF equivalent. |
| 2026-04-26 | Analytics India Magazine (analyticsindiamag.com) | Tier 3 (experimental) | Tavily. India AI ecosystem. |
| 2026-04-26 | Geektime (geektime.com) | Tier 3 (experimental) | Tavily. Israeli tech press. |
| 2026-04-26 | e27 (e27.co) | Tier 3 (experimental) | Tavily. Southeast Asia/Singapore AI. |
| 2026-04-26 | Synced Global (syncedreview.com) | Tier 3 (experimental) | No RSS confirmed. Verify freshness before first use. |
| 2026-04-26 | Moonshot AI / Kimi (kimi.com/blog) | Tier 3 | Updated from prose-only to proper entry with verified URL (2026-04-20 active). |

**Skipped per direction:** EU AI Office (`digital-strategy.ec.europa.eu/en/policies/ai-office`) — Operator explicitly asked to avoid EU regulatory sources. Remaining non-EU policy/safety coverage: UK AISI, Stanford HAI, OECD.AI, US AI.gov, Center for AI Safety.

### Added 2026-04-26 (Round 2 Expansion — Tech Blogs & Market Leaders)

Sources proposed by research subagent in `source-expansion-proposal-round2-2026-04-26.md`, approved wholesale. Focus was gaps from Round 1: enterprise AI platforms, AI-native application leaders, developer infrastructure, non-NVIDIA hardware. Tight proposal — 16 sources proposed / 30 rejected with reasons.

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| 2026-04-26 | Databricks Blog (databricks.com/blog) | Tier 2 | RSS ✅ databricks.com/feed. Daily-active. Unity AI Gateway, Mosaic AI, DBRX, LLM agent optimization. |
| 2026-04-26 | Azure AI Blog (azure.microsoft.com/blog/tag/ai) | Tier 2 | RSS ✅. Azure AI Studio, Copilot enterprise, AI governance. Distinct from MS Research Blog. |
| 2026-04-26 | Google Cloud AI Blog (cloud.google.com/blog/topics/ai-machine-learning) | Tier 2 | No RSS — Tavily. Vertex AI, TPU availability, enterprise Gemini patterns. Distinct from DeepMind + Google Blog. |
| 2026-04-26 | Replit Blog (blog.replit.com) | Tier 2 | RSS ✅ blog.replit.com/feed.xml. Daily. Replit Agent 4, $400M raise at $9B. 50M+ users. Vibe-coding signal. |
| 2026-04-26 | LlamaIndex Blog (llamaindex.ai/blog) | Tier 2 | No RSS — Tavily. Pivoted to agentic document AI (ParseBench, LiteParse). LangChain competitor. |
| 2026-04-26 | Sakana AI (sakana.ai) | Tier 2 | No RSS — check directly. Ex-Google Brain lab. Fugu multi-agent FM, AI Scientist (Nature), Darwin Gödel Machine. |
| 2026-04-26 | ElevenLabs Blog (elevenlabs.io/blog) | Tier 2 | No RSS — Tavily. Eleven v3, ElevenAgents enterprise. $11B valuation. Voice AI layer. |
| 2026-04-26 | Weaviate Blog (weaviate.io/blog) | Tier 2 | RSS ✅ weaviate.io/blog/rss.xml. v1.37 built-in MCP server. Vector DB architecture. OSS. |
| 2026-04-26 | Pinecone Blog (pinecone.io/blog) | Tier 2 | No RSS — Tavily. Dedicated Read Nodes GA, load balancing AI services. Vector DB. |
| 2026-04-26 | Snowflake AI Blog (snowflake.com/en/blog) | Tier 3 | No RSS — Tavily. Cortex AI + Arctic. Weekly. Strategically important for enterprise data-cloud AI. |
| 2026-04-26 | Eugene Yan (eugeneyan.com) | Tier 3 | RSS ✅ eugeneyan.com/rss/. Anthropic MTS. LLM evals, LLM-as-judge. Bi-monthly but deep. |
| 2026-04-26 | Every — Dan Shipper et al. (every.to) | Tier 3 | No RSS — Tavily. AI strategy + building-with-LLMs essays. Soft paywall. |
| 2026-04-26 | Sequoia Capital AI (sequoiacap.com/stories) | Tier 3 | No RSS — Tavily. AI market theses, revenue benchmarks. Quarterly, high-impact. |
| 2026-04-26 | Perplexity AI Blog (perplexity.ai/hub/blog) | Tier 3 | Cloudflare-blocked — Tavily-only. Sonar API, answer-engine patterns, enterprise deployments. |
| 2026-04-26 | Runway Research (runwayml.com/news) | Tier 3 | No RSS — Tavily. Video AI / world model. Quarterly. Major releases only. |
| 2026-04-26 | AMD ROCm / Instinct (amd.com/en/developer/resources/amd-ai.html) | Tier 3 | No RSS — Tavily. MI300X + ROCm 6.x. #2 AI GPU after NVIDIA. |

**Notable rejections from round 2 (for reference, full list in proposal file):**
- Chip Huyen — RSS ✅ but no posts since Jan 2025 (15+ months). Revisit if active.
- Glean, Harvey, Sierra, Decagon, Writer — no verifiable public technical blogs. PR-only. Track via Tavily for major announcements only.
- Black Forest Labs (FLUX) — quarterly cadence, safety-policy-only content. Revisit if technical posts resume.
- Runway accepted at Tier 3 despite Dec 2025 last post (major releases only).
- Midjourney, Pika, Suno, Character.AI Research — low cadence or no formal blog.
- MongoDB AI, Supabase AI, Cloudflare Workers AI — tracked via Tavily when Dim D coverage is sufficient via Weaviate/Pinecone/LlamaIndex.
- Intel AI, Google TPU, Etched, Tenstorrent — insufficient cadence or covered elsewhere.

**Round 2 dedup catches:** AWS ML Blog (Tier 1), AlphaSignal (Tier 2), Groq (Tier 2), Cerebras (Tier 2), NVIDIA Technical Blog (Round 1), Microsoft Research (Round 1) — all confirmed already in registry.

### Added 2026-04-26 (Round 3 Expansion — VC Technical Content & Thought Leaders)

Sources proposed by research subagent in `source-expansion-proposal-round3-2026-04-26.md`, approved wholesale. Tight round — 4 proposed / 22 rejected. Included a re-audit of Round 2 rejections.

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| 2026-04-26 | Menlo Ventures Perspective (menlovc.com/perspective) | Tier 3 | RSS ✅ menlovc.com/feed/. Annual State of Gen AI in the Enterprise — 500+ decision-maker survey, $37B 2025 spend. Anthropic investor (Anthology Fund). |
| 2026-04-26 | Bessemer Atlas AI & ML (bvp.com/atlas) | Tier 3 | **Round 2 rejection reversed.** Round 2 checked wrong URL. Atlas publishes weekly AI content + Bessemer Predicts forecasts. Tavily-only. |
| 2026-04-26 | Exponential View — Azeem Azhar (exponentialview.co) | Tier 2 | RSS ✅ exponentialview.substack.com/feed. 152K subs. Weekly macro-exponential-tech. Distinct from Benedict Evans/Stratechery. |
| 2026-04-26 | Cognitive Revolution — Labenz + Torenberg (cognitiverevolution.substack.com) | Tier 2 | RSS ✅. Weekly podcast + newsletter. Builder interviews + capability deep-dives. VC-flavored (Torenberg @ Turpentine). |

**Round 2 rejections re-audited (2026-04-26):**
- **Bessemer — REVERSED.** Round 2 checked bvp.com, but AI content lives at bvp.com/atlas. Proposed as Tier 3 above.
- **Chip Huyen — CONFIRMED DEAD.** Last post Jan 16, 2025. No resumption.
- **Lilian Weng — CONFIRMED DORMANT.** Last post May 1, 2025. feed.xml 404. No new blog at Thinking Machines found.
- **First Round Review — CONFIRMED REJECT.** AI content is incidental to founder-playbook format.

**Notable round-3 rejections:**
- **No Priors (Sarah Guo + Elad Gil)** — nopriors.com fails DNS, no verifiable Substack. Popular podcast with no public feed we could find.
- **VC landscape thinner than assumed:** Khosla, Accel, Index, NEA, General Catalyst, Tiger Global, Founders Fund, Coatue — either no public blog or investment-announcement format. Lightspeed "Stories" = founder narratives. Greylock Greymatter = JS-rendered, mixed content, no RSS.
- **Yi Tay (yitay.net)** — only 2 posts in 14 months. Personal reflection, not technical.
- **fast.ai** — RSS ✅ active but content is AI-ethics/education, not architecture/engineering. Different angle, different report.
- **Machine Learning Street Talk** — mlst.fm DNS fails. Video-only.
- **Riley Goodside, Clementine Fourrier, Guillaume Lample** — X/Twitter-only, no blogs.
- **Masters of Scale, 20VC, Data Driven NYC** — not AI-specific enough, or low AI cadence.

**Ecosystem finding:** After 3 rounds of expansion, the VC technical content layer is effectively mapped. Core sources now = a16z AI + Sequoia + Menlo + Bessemer Atlas + Nathan Benaich (State of AI). Further rounds produce marginal noise, not signal.

### Added 2026-05-09 (operator handoff — Simon Willison link harvest)

Operator forwarded Simon Willison's Substack profile asking to add him + harvest his link blog for additional sources. Simon was already Tier 1 in this registry (RSS-confirmed). No tier change — entry enrichment only via SDLC alignment. Link harvest produced 1 new GenAI source.

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| 2026-05-09 | Andy Masley (blog.andymasley.com) | Tier 3 | RSS ✅ via Substack feed. 8K+ subs. Counter-FUD analyst on AI infrastructure: "The AI water issue is fake", "Data center land use issues are fake". Cited by Simon Willison in xAI/Anthropic Colossus deal coverage. Useful counterweight to mainstream "AI is destroying the planet" framing in data center economics coverage. Niche but high-signal when topic is environmental/political AI infra debate. |

**Other links surveyed but not added (preserved to prevent re-evaluation):**

- **Andon Labs (andonlabs.com/blog)** — AI-run cafe/store experiments. Anecdotal, not GenAI-frontier. Reject.
- **SpeechMap (speechmap.substack.com)** — Niche model-deprecation tracker / free-speech benchmark. Single-purpose, low cadence. Skip — Simon link-blogs the relevant moments.
- **xlr8harder (Twitter)** — X-only, no blog. Already surfaces via Simon's link blog.
- **Heavybit / High Leverage podcast** — Podcast format. Episodes get cited via Simon when relevant. Skip as direct source.
- **Politico (gas turbines / Memphis xAI)** — General-news outlet, not AI-specific. Already surfaces via Tier 1 commentators when material.
