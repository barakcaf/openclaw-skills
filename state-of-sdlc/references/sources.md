# AI SDLC Report — Source Registry

*Updated by the agent after each run. Add new sources with date discovered and tier.*

## Tier 1 — Must Check Every Run

| Source | Monitor | Tavily Query |
|--------|---------|-------------|
| Andrej Karpathy | `karpathy.github.io/feed.xml` ✅ + `karpathy.ai/blog/` (no RSS) | `"Andrej Karpathy" AI engineering site:karpathy.ai OR site:x.com` |
| Peter Steinberger (OpenClaw) | `steipete.me` (check for RSS) | `"Peter Steinberger" OR "steipete" AI agents openclaw site:steipete.me OR site:x.com` |
| Anthropic Engineering | `anthropic.com/engineering` (no RSS confirmed 2026-04-26) | `site:anthropic.com/engineering OR site:anthropic.com/research` |
| OpenAI Engineering | `openai.com/news/engineering/` + `openai.com/news/rss.xml` ✅ (RSS confirmed 2026-04-26) | `site:openai.com/news engineering OR research` |
| GitHub Blog | `github.blog/feed/` ✅ | `site:github.blog Copilot OR "developer productivity" OR AI` |
| GitLab | `about.gitlab.com/atom.xml` ✅ | `site:about.gitlab.com DevSecOps OR AI developer` |
| Shopify / Tobi Lütke | `shopify.engineering` (no RSS) | `site:shopify.engineering OR "Tobi Lutke" AI engineering` |
| Jellyfish | `jellyfish.co/blog/` (no RSS) — SEMR annually | `site:jellyfish.co AI OR "engineering management"` |
| DORA / Google | `dora.dev` — annual report + insights | `site:dora.dev` |
| Pragmatic Engineer | `newsletter.pragmaticengineer.com` ✅ RSS | `site:newsletter.pragmaticengineer.com` |
| METR | `metr.org` — RCTs on AI dev productivity | `site:metr.org AI developer productivity` |
| Martin Fowler / ThoughtWorks | `martinfowler.com/feed.atom` ✅ | `site:martinfowler.com AI OR "generative AI" OR "coding agent" OR "harness engineering"` |
| DX (getdx.com) | `getdx.com/research` (no RSS) — frequent peer-reviewed research | `site:getdx.com AI OR "developer productivity" OR SPACE research` |
| Will Larson / Irrational Exuberance | `lethain.com/feeds/` ✅ | `site:lethain.com AI OR "engineering productivity" OR "coding agents" OR "developer experience"` |
| Charity Majors (charity.wtf) | `charity.wtf/feed/` ✅ | `site:charity.wtf AI OR observability OR "agentic development" OR SRE` |
| Addy Osmani (Elevate) | `addyo.substack.com/feed` ✅ + `addyosmani.com/blog/` | `site:addyo.substack.com OR site:addyosmani.com agent OR agentic OR "AI engineering" after:{SEARCH_FROM}` — Google Cloud AI Director, 38K subs, weekly on agent harness engineering, parallel agents, factory model. Added 2026-04-26, promoted to Tier 1 by operator. |
| Simon Willison | `simonwillison.net` | `simonwillison.net/atom/everything/` ✅ | `site:simonwillison.net agentic OR "coding agent" OR harness OR "claude code" OR "vibe coding" after:{SEARCH_FROM}` — Django co-creator, Datasette author, 58K+ Substack subs. Daily long-form posts, live blogs from major AI events (Code w/ Claude 2026), original essays (vibe coding vs agentic engineering, normalization of deviance, harness engineering). Promoted from Tier 2 (2026-04-06) to Tier 1 on 2026-05-09 — 5+ weeks of consistent high-signal output, heavily cited by Böckeler/Pragmatic Engineer/Thoughtworks. Maintains Agentic Engineering Patterns guide. |

## Tier 2 — Check on Signal

| Source | Tavily Query |
|--------|-------------|
| Google DeepMind | `deepmind.google/blog/rss.xml` ✅ (RSS confirmed 2026-04-26) | `site:deepmind.google OR site:research.google AI software development` |
| Spotify Engineering | `site:engineering.atspotify.com AI OR "developer productivity"` (RSS: `/feed` ✅) |
| Vercel | `site:vercel.com/blog AI OR SDK OR v0` |
| DevEx / DX Data | `"developer experience" AI productivity research "DX" OR "DevEx" survey` |
| Thoughtworks Radar | `site:thoughtworks.com radar` (bi-annual Apr/Oct) |
| Stack Overflow | `site:stackoverflow.blog developer AI` (annual survey Q4/Q1) |
| Wix Engineering | `wix.engineering/blog` + `wix.engineering/ai-native-development` — `site:wix.engineering AI OR agent OR "AI-native"` — xEngineer org design, dedicated AI-Native Development section, AI Meetup. Discovered 2026-04-03, enriched 2026-04-26. |
| monday.com Engineering | `engineering.monday.com/feed` ✅ RSS — `site:engineering.monday.com AI OR agent` — Highly active (posted today Apr 26). Recent: "I Am Morphex: I'm an AI Agent Growing Up Inside a Real Codebase" (Apr 9), "The Death of model.fit(): What Data Scientists Do in the Age of AI Agents", scaling posts. Promoted from Tier 3 to Tier 2 on 2026-04-26. |
| JetBrains AI Blog | `blog.jetbrains.com/ai/feed/` ✅ RSS | `site:blog.jetbrains.com AI OR Junie OR Koog OR agent OR ACP after:{SEARCH_FROM}` — active Apr 22. Junie agent, Koog Java agent framework, JetBrains Central (agentic dev platform), ACP + Deep Agents posts. Distinct from Dev Ecosystem Survey (T3). Added 2026-04-26. |
| Block Engineering (formerly Square) | `engineering.block.xyz` (no RSS — all paths 404/500) | `site:engineering.block.xyz AI OR agent OR "AI-assisted" OR "developer productivity" after:{SEARCH_FROM}` — "AI-Assisted Development at Block", "How We Red-Teamed Our Own AI Agent", "3 Principles for Designing Agent Skills". **Promoted from Tier 3** and URL corrected 2026-04-26. |
| Apiiro | `apiiro.com/blog/feed/` ✅ RSS | `site:apiiro.com/blog AI code OR "AI-generated" OR SDLC OR "agentic" OR "AI threat modeling" after:{SEARCH_FROM}` — "The AI Control Plane Is the Only Way Out for AppSec", "Security Tools Were Built for Humans. We Built One for AI Agents — Introducing Apiiro CLI", "Introducing AI Threat Modeling", "Half of Google's Code Is Now AI-Generated: What That Means for Security Leaders". Israeli (Tel Aviv). Added 2026-04-26. |
| Snyk Blog | `snyk.io/blog/feed/` ✅ RSS | `site:snyk.io/blog AI agent OR "AI-generated code" OR "agent security" OR "supply chain" research after:{SEARCH_FROM}` — ToxicSkills empirical research (prompt injection in agent skills), "I Read Cursor's Security Agent Prompts", "The 89% Problem: How LLMs Are Resurrecting Dormant Open Source". Daily cadence — filter to research/engineering posts. Israeli-founded. Added 2026-04-26. |
| Zenity | `zenity.io/blog` (no RSS — feed 404) | `site:zenity.io/blog AI agent security OR "agentic AI" OR SDLC OR "coding agent" after:{SEARCH_FROM}` — "Context Engineering Is Security Engineering", "Why Soft Guardrails Get Us Hacked", "Securing Homegrown Agents in Runtime". Only Israeli startup writing directly about agentic AI in developer workflow. Israeli (Tel Aviv). Added 2026-04-26. |
| Wiz Blog | `wiz.io/blog` (no RSS — feed 404) | `site:wiz.io/blog "agentic" OR "AI-generated code" OR "AI SDLC" OR "coding agent" security after:{SEARCH_FROM}` — "Closing the Security Gap in the Age of Agentic Coding" (Apr 21), MCP server integration, Wiz Code for AI-IDEs. Heavy product framing — filter tight. Demote to T3 if signal-to-noise poor. Israeli (Tel Aviv). Added 2026-04-26. |
| Komodor | `komodor.com/blog` (no RSS — feed 301s to query param) | `site:komodor.com/blog "multi-agent" OR "AI SRE" OR "agentic" OR "autonomous" Kubernetes after:{SEARCH_FROM}` — "Multi-Agent AI SRE Has Landed" (11-min architecture deep-dive), autonomous K8s ops, agentic incident response. Observability-for-agentic-systems angle. Israeli (Tel Aviv). Added 2026-04-26. |
| OX Security | `"OX Security" AI code quality` — Discovered 2026-04-03 |
| Hacker News | `site:news.ycombinator.com AI SDLC OR "developer productivity"` |
| Google Testing Blog | `testing.googleblog.com/feeds/posts/default` ✅ (feedburner) | `site:testing.googleblog.com AI OR testing OR "developer productivity"` |
| Google Developers Blog | `developers.googleblog.com/feeds/posts/default/` ✅ | `site:developers.googleblog.com AI coding OR "developer productivity" OR "code assist" OR agent` |
| InfoQ (SE + AI track) | `feed.infoq.com` ✅ | `site:infoq.com AI "software engineering" OR "developer productivity" OR "coding agents"` |
| Platform Engineering Org | `platformengineering.org/blog` (no RSS confirmed) | `site:platformengineering.org AI OR "developer experience" OR IDP` |
| JetBrains Developer Ecosystem Survey | `jetbrains.com/lp/devecosystem-` (annual, large-N ~24K) | `"JetBrains" developer ecosystem survey AI productivity` |
| Sourcegraph Blog | `sourcegraph.com/blog` (no RSS confirmed) | `site:sourcegraph.com/blog AI OR agent OR benchmark OR SDLC after:{SEARCH_FROM}` — CodeScaleBench benchmark (Mar 2026), MCP at scale, code intelligence deployment. Added 2026-04-26. |
| Zed Blog | `zed.dev/blog` (check `zed.dev/blog/feed.xml`) | `site:zed.dev/blog agent OR agentic OR "parallel agents" OR metrics after:{SEARCH_FROM}` — Agent Metrics dashboard (weekly adoption data), Zeta2 model research, parallel agents. Added 2026-04-26. |
| Cognition Blog (Devin) | `cognition.ai/blog` (no RSS confirmed) | `site:cognition.ai/blog cloud agent OR multi-agent OR "SWE" after:{SEARCH_FROM}` — "Multi-Agents: What's Actually Working" (Apr 2026 follow-up to viral 2024 post), SWE-Check RL model, cloud agent learnings. Added 2026-04-26. |
| Meta Engineering Blog | `engineering.fb.com/feed/` ✅ | `site:engineering.fb.com AI OR agent OR "developer productivity" after:{SEARCH_FROM}` — KernelEvolve agent, unified AI agents at hyperscale, AI-mapped tribal knowledge. Added 2026-04-26. |
| The New Stack | `thenewstack.io/feed/` ✅ | `site:thenewstack.io AI coding OR "coding agent" OR "developer productivity" OR platform after:{SEARCH_FROM}` — Daily-active DevOps/cloud native/platform engineering with high AI-SDLC density. Added 2026-04-26. |
| LeadDev | `leaddev.com/rss.xml` ✅ | `site:leaddev.com AI OR "developer productivity" OR "engineering leadership" after:{SEARCH_FROM}` — Engineering leadership, AI Impact Report 2025. Formalized from candidate 2026-04-26. |
| GitClear Research | `gitclear.com` (no RSS; Tavily) | `site:gitclear.com AI OR "code quality" OR "code churn" OR "durable code" after:{SEARCH_FROM}` — 2026 research: 2,172 developer-weeks telemetry, Cursor/Copilot/Claude Code API integration, durable-code metrics. Added 2026-04-26. |
| CodeScene Blog | `codescene.com/blog` (no RSS; Tavily) | `site:codescene.com/blog AI OR agentic OR "code quality" OR refactoring after:{SEARCH_FROM}` — Agentic refactoring benchmarks, behavioral code analysis on AI-generated code. Added 2026-04-26. |
| Dagger.io Blog | `dagger.io/blog` (no RSS; Tavily) | `site:dagger.io/blog AI OR agent OR CI OR eval after:{SEARCH_FROM}` — "Evals as Code", LLM Primitive in Dagger, self-healing CI pipelines with agents. Added 2026-04-26. Demote to T3 if no 2026 posts found. |
| Geoffrey Huntley (ghuntley.com) | `ghuntley.com` (RSS likely at `/rss/`) | `site:ghuntley.com agent OR loop OR "ralph loop" OR codegen OR cursed after:{SEARCH_FROM}` — Practitioner who coined the "Ralph loop" pattern (cited by Anthropic's Cat Wu at Code w/ Claude 2026 keynote). Posts: "everything is a ralph loop", "anti-patterns and patterns for achieving secure generation of code via AI", "i ran Claude in a loop for three months" (genz language `cursed`), "AI as economic warfare", "cognitive security". Live, opinionated, hands-on. Discovered via Simon Willison citation 2026-05-09. |

## Tier 3 — Quarterly / Major Releases

| Source | Tavily Query |
|--------|-------------|
| McKinsey / BCG | `"McKinsey" OR "BCG" AI "software development" productivity report` |
| Linear | `site:linear.app/blog AI OR "product development"` |
| SPACE Framework Paper | `"SPACE framework" developer productivity Forsgren OR GitHub OR Microsoft` |
| Nicole Forsgren Research | `"Nicole Forsgren" developer productivity OR DORA OR SPACE OR AI` |
| arXiv cs.SE (AI-assisted dev) | `site:arxiv.org "AI-assisted" OR "LLM-assisted" software engineering 2025 OR 2026` |
| Microsoft Research (DevDiv) | `site:microsoft.com/en-us/research "developer productivity" OR "software engineering" AI` |
| DevOps Institute | `site:devopsinstitute.com AI OR "developer productivity" OR upskilling` |
| Camille Fournier | `medium.com/feed/@skamille` ✅ — `"Camille Fournier" AI OR "tech employment" OR "engineering org" after:{SEARCH_FROM}` — former MD Eng at Dropbox/Two Sigma. "Things I Currently Believe About AI and Tech Employment" (May 2025). Experienced CTO voice. Added 2026-04-26. |
| Team Topologies Blog | `site:teamtopologies.com AI OR "platform engineering" OR "org design" after:{SEARCH_FROM}` — canonical framework for org design in platform engineering. Stream-aligned/platform team structure under AI. Added 2026-04-26. |

## Notes

- All Tavily queries append `after:{SEARCH_FROM}`
- Fallback for any source: search HN or X for reactions
- **Promote to Tier 2** if: verifiable engineering leadership, original data/firsthand experience, covers our themes
- **Promote to Tier 1** if: consistent high-signal over 2+ editions or large-sample research
- **Discard** if: vendor marketing, AI-generated summaries, rehashes

## Discovered Sources Log

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| 2026-04-03 | METR (metr.org) | Tier 1 | Rigorous RCTs on AI developer productivity |
| 2026-04-03 | Wix Engineering (wix.engineering) | Tier 2 | xEngineer org design, AI-native transformation |
| 2026-04-03 | OX Security | Tier 2 | Empirical AI-generated code quality analysis |
| 2026-04-03 | Peter Steinberger (steipete.me) | Tier 1 | OpenClaw creator, joined OpenAI to build personal agents. Practitioner building agentic AI at the frontier. |
| 2026-04-03 | Martin Fowler / ThoughtWorks (martinfowler.com) | Tier 1 | RSS ✅ feed.atom. High-signal weekly. Actively covering harness engineering, coding agents, agentic SDLC, human-AI loops. Best single source for ThoughtWorks practitioner thinking on AI in SE |
| 2026-04-03 | DX (getdx.com/research) | Tier 1 | Peer-reviewed research archive on developer productivity, SPACE, DevEx. Curated by Abi Noda + Nicole Forsgren. Recent papers on AI impact on developers, Google LLM migrations at scale, SPACE of AI. No RSS but prolific — check monthly |
| 2026-04-03 | Will Larson / Irrational Exuberance (lethain.com) | Tier 1 | RSS ✅. VP Eng / CTO practitioner. Writing agents series (internal agents, evals, subagents, harness). Also engineering strategy, dev productivity benchmarks, LLM adoption. Recent post March 2026 on "agentic passive voice" — sharp framing |
| 2026-04-03 | Charity Majors (charity.wtf) | Tier 1 | RSS ✅ charity.wtf/feed/. Co-founder Honeycomb. High-signal on observability + agentic validation. March 2026 posts directly address AI agents needing production observability, DevOps 20-year failure, SRE in agentic era. Crossposted from Substack |
| 2026-04-03 | Google Testing Blog (testing.googleblog.com) | Tier 2 | RSS ✅ via FeedBurner. Google's internal testing culture blog — TotT (Tips on the Toilet) series. Slower cadence (~monthly) but authoritative on testing practices at scale. Relevant when AI changes test strategy |
| 2026-04-03 | Google Developers Blog (developers.googleblog.com) | Tier 2 | RSS ✅ feeds/posts/default. High-volume. Covers Gemini Code Assist, ADK, agent development, coding tools. Check for AI-SDLC-relevant posts; filter out product marketing |
| 2026-04-03 | InfoQ (infoq.com) | Tier 2 | RSS ✅ feed.infoq.com. Conference talk transcripts (QCon, InfoQ Dev Summit) + news. Strong AI engineering coverage: coding agents, developer experience, platform engineering. Adrian Cockcroft talk on agent swarms published Apr 2026 |
| 2026-04-03 | Platform Engineering Org (platformengineering.org) | Tier 2 | Active blog on IDP, DevEx, AI platform engineering. Platform engineering is the org design layer under AI-augmented dev. No RSS confirmed — check manually quarterly |
| 2026-04-03 | JetBrains Developer Ecosystem Survey | Tier 3 | Annual survey, 24K+ developers. 2025 edition available. Covers AI tool adoption, productivity perceptions, language trends. Good for large-N cross-validation. New edition ~Q3 annually |
| 2026-04-03 | SPACE Framework / Nicole Forsgren | Tier 3 | Framework paper at getdx.com/research/space-of-developer-productivity. Co-author at Microsoft Research + GitHub. Follow via DX research archive and Microsoft Research |
| 2026-04-03 | arXiv cs.SE | Tier 3 | Pre-print papers on AI-assisted SE. Search quarterly: `arxiv.org/search/?searchtype=cs&query=AI+software+engineering`. High signal when results cited by Tier 1 sources |
| 2026-04-03 | Microsoft Research (developer productivity) | Tier 3 | Team behind SPACE, Forsgren, Zimmermann. research.microsoft.com. No RSS. Search: `microsoft.com/research developer productivity AI` |
| 2026-04-03 (week 2) | Philipp Schmid (philschmid.de) | Tier 2 candidate | Hugging Face ML engineer. "Agent Harness 2026" post well-cited. Focused on practical agent infrastructure. No RSS confirmed. Query: `site:philschmid.de agent OR harness OR coding` |
| 2026-04-06 | Simon Willison (simonwillison.net) | **Tier 2** (promoted) | Promoted from candidate. Active Agentic Engineering Patterns guide with new chapters weekly (March 2026: subagents, linear walkthroughs, Red/Green TDD). High citation rate from Böckeler and Pragmatic Engineer. Independent practitioner voice distinct from Thoughtworks/DX. Query: `site:simonwillison.net agentic OR "coding agent" OR harness` |
| 2026-05-09 | Simon Willison (simonwillison.net) | **Tier 1** (promoted by operator) | Promoted from Tier 2 after 5 weeks of consistent high-signal output. Live blog of Code w/ Claude 2026 keynote, "Vibe coding and agentic engineering are getting closer than I'd like" essay, daily link-blog catching every signal. Auto-includes podcast appearances (Heavybit High Leverage), conference talks (Jenny Wen on design process), and primary research links. Best single aggregator of frontier AI-SDLC discourse. |
| 2026-04-06 | Cortex Engineering Benchmark (cortex.io) | Tier 3 | Annual benchmark report. 2026 edition: PRs +20%, incidents/PR +23.5%, change failure rates +30%. Good quantitative cross-validation. Query: `site:cortex.io AI engineering benchmark` |
| 2026-04-13 | Faros AI (faros.ai) | **Tier 2** | AI Engineering Report 2026 — 22K developers, 4K teams, 2yr telemetry. Best multi-org empirical dataset on AI quality impact. Not vendor-reported. Query: `site:faros.ai AI engineering report OR "developer productivity" OR metrics` |
| 2026-04-13 | LeadDev (leaddev.com) | Tier 2 candidate | Engineering leadership perspective, named CTO quotes, org-level AI impact coverage. Query: `site:leaddev.com AI OR "developer productivity" OR agents` |
| 2026-04-20 | Anthropic Agentic Coding Trends Report 2026 | Tier 1 (annual report) | Primary practitioner data on agent deployment: Rakuten (7hr autonomous run, 12.5M-line codebase, 99.9% accuracy), Zapier (89% org-wide AI adoption, 800+ agents). Annual Anthropic primary data. Query: `anthropic.com "agentic coding trends" report 2026` |
| 2026-04-22 | Sonar "Pragmatic" (sonarsource.com/pragmatic) | Tier 2 candidate | Added by operator. SonarSource's AI code quality content hub — likely covers LLM-generated code quality, trust, review at scale. Evaluate next edition: check article cadence, whether it's vendor marketing vs. engineering content. URL: https://www.sonarsource.com/pragmatic/ |
| 2026-05-10 | From the Trenches — Denis Stetskov (techtrenches.dev / @razoorka) | **Tier 2** (promoted 2026-05-11) | RSS ✅ `techtrenches.dev/feed`. Added by operator 2026-05-10. Engineering Manager, 2.1K+ subs. Weekly cadence (Mon/Tue). Skeptical-pragmatist voice — explicitly "signal vs. hype" framing. Strong empirical-data citations: CMU 807-repo Cursor study (complexity +41%, warnings +30%), AGENTIF instruction-compliance research, METR cross-references. Recent posts directly serve SDLC themes: "AI Is a Mirror of Our Engineering Culture" (May 5), "The Human Cost of 10x: How AI Is Physically Breaking Senior Engineers" (Apr 7), "Your CLAUDE.md Is a Wish List, Not a Contract" (Mar 30), "The Snake That Ate Itself: What Claude Code's Source Revealed About AI Engineering Culture" (Apr 1), "AI Agent Platforms: The Security Nightmare Nobody's Talking About" (Feb 23). Tier 2 candidate — promote to T2 confirmed after first edition cites him; Tier 1 candidate after 2+ editions of high-signal output (registry rule). Query: `site:techtrenches.dev after:{SEARCH_FROM}`. |

### Added 2026-04-26 (Round 1 Expansion — 6-Dimension Audit)

Sources proposed by research subagent in `source-expansion-proposal-2026-04-26.md`, approved with one tier adjustment (Addy Osmani → Tier 2 per registry promotion rule, not Tier 1 as proposed). Tight round: 12 proposed / 33 rejected across 6 dimensions (vendor eng blogs, big tech eng, DevProd research, leadership voices, platform eng, academic).

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| 2026-04-26 | Addy Osmani — Elevate (addyo.substack.com) | Tier 2 | RSS ✅ addyo.substack.com/feed. Google Cloud AI Director, 38K subs. Weekly on agent harness engineering, parallel agents, factory model. Headline find. Subagent proposed Tier 1 but registry rule requires 2 editions before T1 — downgraded to T2 with fast-promotion candidate flag. |
| 2026-04-26 | Sourcegraph Blog (sourcegraph.com/blog) | Tier 2 | No RSS — Tavily. CodeScaleBench benchmark (Mar 2026), MCP at scale, code intelligence deployment. |
| 2026-04-26 | Zed Blog (zed.dev/blog) | Tier 2 | RSS unconfirmed — check feed.xml. Weekly Agent Metrics dashboard (quantitative AI adoption data), Zeta2 model research, parallel agents. |
| 2026-04-26 | Cognition Blog (cognition.ai/blog) | Tier 2 | No RSS — Tavily. "Multi-Agents: What's Actually Working" (Apr 2026), SWE-Check RL model, cloud agent learnings. |
| 2026-04-26 | Meta Engineering Blog (engineering.fb.com) | Tier 2 | RSS ✅ engineering.fb.com/feed/. KernelEvolve agent, unified AI agents at hyperscale, AI-mapped tribal knowledge. |
| 2026-04-26 | The New Stack (thenewstack.io) | Tier 2 | RSS ✅ thenewstack.io/feed/. Daily-active DevOps/cloud native/platform engineering. High AI-SDLC density. |
| 2026-04-26 | LeadDev (leaddev.com) | Tier 2 | RSS ✅ leaddev.com/rss.xml. Formalized from candidate status (noted 2026-04-13). AI Impact Report 2025. |
| 2026-04-26 | GitClear Research (gitclear.com) | Tier 2 | No RSS — Tavily. 2,172 developer-weeks telemetry, Cursor/Copilot/Claude Code API. Empirical code quality research. |
| 2026-04-26 | CodeScene Blog (codescene.com/blog) | Tier 2 | No RSS — Tavily. Agentic refactoring benchmarks, behavioral code analysis on AI-generated code. |
| 2026-04-26 | Dagger.io Blog (dagger.io/blog) | Tier 2 | No RSS — Tavily. CI/CD × AI agents intersection. "Evals as Code", LLM Primitive. Demote to T3 if no 2026 posts. |
| 2026-04-26 | Camille Fournier (skamille.medium.com) | Tier 3 | RSS ✅ medium.com/feed/@skamille. Experienced CTO voice. Last post Nov 2025 — borderline flagged. |
| 2026-04-26 | Team Topologies Blog (teamtopologies.com) | Tier 3 | No RSS — Tavily. Canonical org-design framework for platform engineering under AI. |

**Notable rejections (reasoning preserved for future reference):**
- **Kent Beck (Medium) — CONFIRMED DEAD.** Last post October 2022 (3.5 years dormant). Community lore about "actively writing on AI-TDD" is stale. Do not re-propose unless evidence of new blog.
- **GeePaw Hill — dormant.** Last post Oct 2024, no AI content.
- **GitHub Next — not a blog.** Research team showcase only, no feed.
- **Cursor/Codeium/Windsurf blogs** — already covered via GenAI registry; product-announcement format.
- **Port.io blog** — gated behind lead-capture form, inaccessible.
- **Stripe/Airbnb/Uber/Pinterest/DoorDash/Etsy/Bloomberg/Canva Engineering** — none passed 90-day AI-SDLC content bar. Still infra-focused. Only Meta publicly discusses AI coding at hyperscale.
- **Slack Engineering** — last post Nov 2025.
- **LinkedIn Engineering** — RSS empty, no AI-SDLC posts in 90 days.
- **Julia Evans / Jessitron / Sarah Drasner / Dan Abramov** — active but content not AI-SDLC focused.
- **Gartner/Forrester/IDC** — paywalled.
- **Humanitec, Backstage, Dev Interrupted, SE Daily/Radio** — insufficient cadence or format mismatch.
- **Pete Hodgson, Gojko Adzic, James Stanier, Kelsey Hightower** — no verifiable active AI-SDLC content.

**Cross-registry finding:** After 3 rounds of GenAI expansion + SDLC round 1, most vendor tool blogs are already tracked on the GenAI side. The SDLC report reuses them primarily as *engineering content sources* (architecture posts, benchmark research, deployment learnings), not as release trackers. Dedup logic: same URL can appear in both registries if the content role is distinct.

### Added 2026-04-26 (Spot fixes — Lab engineering blogs + JetBrains AI)

Direct verification of operator's specific ask: Anthropic, OpenAI, Google, JetBrains coverage.

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| 2026-04-26 | JetBrains AI Blog (blog.jetbrains.com/ai) | Tier 2 | RSS ✅ blog.jetbrains.com/ai/feed/. Active Apr 22, 2026. Junie agent, Koog Java agent framework, JetBrains Central agentic dev platform, ACP + Deep Agents. Distinct from the Dev Ecosystem Survey (T3). |
| 2026-04-26 | monday.com Engineering (engineering.monday.com) | Tier 2 | **Promoted from Tier 3.** RSS ✅ engineering.monday.com/feed. Posted today (Apr 26). "I Am Morphex: I'm an AI Agent Growing Up Inside a Real Codebase", "The Death of model.fit()". Promotion earned on content quality. |

**Spot verifications (non-additions, but entries improved):**
- **OpenAI Engineering** — RSS confirmed at openai.com/news/rss.xml (previously listed as "no RSS"). Entry updated.
- **Google DeepMind** — RSS confirmed at deepmind.google/blog/rss.xml (previously Tavily-only). Entry updated.
- **Anthropic Engineering** — RSS still not available (all paths 404). Entry unchanged.
- **Wix Engineering** — Enriched: added AI-Native Development section (wix.engineering/ai-native-development) to monitor paths.
- **Addy Osmani** — Promoted to Tier 1 by operator, overriding the 2-edition rule.

### Added 2026-04-26 (Spot check — Block / xAI / Tesla / SpaceX / Gong)

Direct verification of operator's specific ask.

| Date | Source | Outcome | Notes |
|------|--------|---------|-------|
| 2026-04-26 | Block Engineering (engineering.block.xyz) | **Added Tier 2** | Entry existed in Tier 3 as "Square/Block" with stale URL (developer.squareup.com). Real blog is engineering.block.xyz. Promoted + URL fixed. Strong AI-SDLC content: "AI-Assisted Development at Block", "How We Red-Teamed Our Own AI Agent", "3 Principles for Designing Agent Skills". No RSS (6 paths tried, all 404/500). |

**Rejected — logged to prevent re-evaluation:**

| Source | Status | Reason |
|--------|--------|--------|
| xAI (x.ai/blog) | Cloudflare-gated; accessible via Chrome UA | **Content mismatch:** xAI blog is product announcements only (Grok 1/2/3, Series B/C, Image Gen, Grok Voice, API Beta). Zero engineering/architecture/research posts. Belongs in GenAI registry as a lab news tracker, not SDLC. Skip for SDLC. |
| Tesla (tesla.com/blog) | 403 across all UAs | **Blocked AND wrong domain.** Even if accessible, Tesla's AI content is vehicle autonomy, not SDLC (coding agents, dev productivity). Not this report's domain. |
| SpaceX (spacex.com/updates, spacex.com/feed) | Updates page 200, but feed returns HTML-wrapped metadata only (no real RSS) | **Wrong domain.** Content is launch updates + rocketry. AI work (if any) is not publicly discussed. Not SDLC. |
| Gong (gong.io) | Main blog 200, but no engineering subsite found | **No engineering blog exists.** Tried engineering.gong.io (DNS fail), labs.gong.io (DNS fail), medium.com/gong-engineering (404), gong.io/labs (404). Marketing blog only — not an engineering content source. |

**Methodology note:** For famous companies with "we must publish engineering content, they're huge" intuition — always verify with RSS probes + recent-titles scan. xAI/Tesla/SpaceX/Gong collectively demonstrate the pattern: big company ≠ engineering blog. Block is the counter-example where the eng blog existed but was mis-tracked in the registry.

### Added 2026-04-26 (Israeli Startup Audit)

Focused 5-category scan of Israeli tech startups. 6 proposed / 27 rejected. All 6 accepted (with two RSS corrections from original proposal).

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| 2026-04-26 | Apiiro (apiiro.com/blog) | Tier 2 | RSS ✅ apiiro.com/blog/feed/. Israeli (TLV). Best SDLC-native AppSec blog in scan. Posts directly address AI-generated code volume, SDLC System of Record, AI threat modeling, Apiiro CLI for AI coding agents. Engineering quality high, not threat intel. |
| 2026-04-26 | Snyk Blog (snyk.io/blog) | Tier 2 | RSS ✅ snyk.io/blog/feed/. Israeli-founded (UK/US HQ). Daily cadence — Tavily filter to research/engineering posts. ToxicSkills original empirical research on prompt injection in agent skills. |
| 2026-04-26 | Zenity (zenity.io/blog) | Tier 2 | No RSS (feed returns 404, subagent's 308 note was misread). Tavily-only. Israeli (TLV, Gartner Cool Vendor 2025). Only Israeli startup writing directly about agentic AI in developer workflow. |
| 2026-04-26 | Wiz Blog (wiz.io/blog) | Tier 2 | No RSS (feed returns 404). Tavily-only. Israeli (TLV, $12B Google acq pending). Heavy product framing — filter tight, demote to T3 if signal-to-noise poor after 2 editions. |
| 2026-04-26 | Komodor (komodor.com/blog) | Tier 2 | No RSS — Tavily. Israeli (TLV). Multi-agent AI SRE architecture (March 2026, 11min deep-dive). Observability-for-agentic-systems angle. |
| 2026-04-26 | Logz.io (logz.io/blog) | **REMOVED** | Added then removed same day by operator. Entry preserved here for audit trail only — do not re-propose. |

**Notable rejections (with rationale preserved):**

- **Cyera** — DSPM/AI data security. Blog mostly empty on fetch. DSPM focus is data governance, not SDLC. **Direct operator callout but rejected on content fit.**
- **Checkmarx / Aqua / Legit Security / Orca / Sweet Security** — all Israeli security companies but publish **threat advisories and product PR**, not engineering research. The defining split in this scan: Israeli security startups are 70% threat intel, 30% engineering content.
- **Cycode, Armis, Oligo, Noname, Backslash** — either marketing-only, wrong domain (API security vs SDLC), or blog inaccessible.
- **Lightricks, D-ID, Explorium, Weka** — wrong domain (AI media, data enrichment, storage infra — not SDLC).
- **Tabnine** — already rejected in round 1 as marketing-only, no signal to overturn.
- **Run:ai, Iguazio, Granulate, Spot.io, Rookout** — all acquired, blogs inactive or absorbed into acquirer.
- **BigPanda** — product marketing, no engineering research.
- **Coralogix** — borderline Israeli observability. Tutorials rather than architecture. Flag for next audit cycle.
- **Uri Levine, Gil Dibner, Geektime** — voices on X not blogs, or already covered via VC/regional sources.

**Meta-findings from this scan:**
1. **Israeli security startup landscape is 70% threat intel, 30% engineering.** Only 4 of ~13 security startups cleared the engineering-content bar (Apiiro, Snyk, Zenity, Wiz — with Wiz caveat).
2. **No Israeli equivalent to Cursor/Cognition/Zed exists.** The Israeli AI-native developer tooling space outside security is effectively empty in 2026. Tabnine is the only major name and it doesn't publish engineering content.
3. **Israeli data/AI infra is acquired out.** Run:ai (NVIDIA), Iguazio (McKinsey), Granulate (Intel), Spot (NetApp), Rookout (Dynatrace) — all gone as standalone blog sources.
4. **Individual Israeli eng voices are on X, not long-form.** Blog-based practitioner voices are rare in the Israeli engineering ecosystem.

### Added 2026-05-09 (operator handoff — Simon Willison link harvest)

Operator forwarded Simon Willison's Substack profile asking to add him + harvest his link blog for additional sources. Outcome: Simon promoted from Tier 2 → Tier 1 (5 weeks of consistent high-signal output exceeded promotion bar). Link harvest added 1 new SDLC source.

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| 2026-05-09 | Simon Willison (simonwillison.net) | **Tier 1** (promoted from T2) | Originally promoted from candidate to T2 on 2026-04-06. After 5+ weeks of daily AI-SDLC-relevant posts (Code w/ Claude 2026 live blog, "Vibe coding and agentic engineering are getting closer than I'd like", normalization-of-deviance pieces), promoted to Tier 1 on operator's request. Already T1 in GenAI registry — now aligned across both reports. |
| 2026-05-09 | Geoffrey Huntley (ghuntley.com) | Tier 2 | Coined the "Ralph loop" pattern, cited by Anthropic's Cat Wu at Code w/ Claude 2026 keynote ("Outcomes... sounds like a Ralph loop"). Active practitioner blog: secure code generation, agent loops, AI economic warfare, cognitive security, vibe-coded language experiments. Discovered via Simon Willison citation. |

**Other links surveyed but rejected (preserved to prevent re-evaluation):**

- **Andon Labs (andonlabs.com/blog)** — Cute experiments (AI-run retail store, AI-run cafe). Anecdotal narrative content, not SDLC-relevant. Reject.
- **Luke Curley (moq.dev/blog)** — One-off WebRTC critique post. Niche transport/streaming engineering, not AI-SDLC. Reject.
- **Thariq Shihipar (thariqs.github.io)** — Anthropic Claude Code team member. Posts via Twitter, not a sustained blog. Reject as direct source — content surfaces via Simon and Anthropic anyway.
- **Heavybit / High Leverage podcast** — Podcast format, episode discovery requires audio transcription. Skip as primary source — Simon link-blogs the relevant takes.
- **Andy Masley (blog.andymasley.com)** — Counter-FUD on AI water/land/energy claims. **Belongs in GenAI registry as Tier 3** (data center economics angle). Not SDLC-relevant. Logged here for traceability — added to GenAI registry separately.
