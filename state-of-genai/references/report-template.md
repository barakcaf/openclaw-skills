# Report Template — State of GenAI

The Telegram-ready markdown template. Used by the drafter (step 6) and re-applied by the auditor (step 3 revise). Sections may be omitted per the omission rules in [METHODOLOGY.md](METHODOLOGY.md) section 5.

```
🧠 STATE OF GENAI — [Date] Edition
━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔑 LEAD SIGNAL
[1 paragraph, 80-120 words. The single most important development this week — a capability leap, a pricing shock, an open source release that changes the calculus. Name the model, cite the number, link the source. One sentence "so what" for builders.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

🤖 MODEL RELEASES & BENCHMARKS
**[Model Name vX.X]** — [2-3 sentences: what changed, key benchmark numbers with links, context vs. prior SOTA. 1 sentence "so what".]
[2-4 entries]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

💰 PRICING & INFRASTRUCTURE
[2-4 bullets. Cross-cloud pricing (AWS Bedrock as primary lens, delta vs Azure/GCP/neo-clouds), inference stack, hardware. Every price claim: exact $/1M tokens (input+output) + provider + [source link]. Use [Artificial Analysis](https://artificialanalysis.ai) as cross-provider benchmark.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

🏭 INDUSTRY MOVES
[3-4 bullets. Funding rounds (size + valuation), acquisitions, major strategy pivots. No press release tone — state the implication directly.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

📊 USAGE & ADOPTION
[2 bullets max. OpenRouter ranking shifts ([link](https://openrouter.ai/rankings)), Arena ELO movement. Brief — just the data points that moved, not commentary.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

🌐 OPEN SOURCE & COMMUNITY
[2-3 bullets. Notable OSS model releases. Framework updates worth caring about. Community sentiment shift. Include Chinese OSS releases when they clear the bar. Skip minor patch releases — only if it changes what's buildable.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

🤖 AGENTIC TOOLS & FRAMEWORKS
[2-3 bullets. New tool releases, major version bumps, benchmark results (SWE-bench leaderboard shifts), adoption data, pricing changes, open-source agent tools closing gap on commercial.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎯 MULTI-MODALITY & DOMAINS
[Include only when there's genuine news. 1-3 bullets max. Vision model benchmarks, audio/video generation releases, medical/code/legal model launches with eval data.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

☁️ AWS & BEDROCK
[2-3 bullets. AWS-specific GenAI developments with direct builder impact. Bedrock pricing changes > new model availability > feature launches > blog posts. Every pricing claim needs exact $/1M tokens + [link].]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚖️ REGULATION & SAFETY
[1-2 bullets only if something actually moved this week. Skip if nothing significant. A bill being discussed is not signal. A bill passing, a major fine, or a new enforcement action is signal.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

⚡ TENSION / COUNTER-SIGNAL
[1 paragraph, 60-80 words. Where credible sources or data points genuinely disagree. E.g., "Arena rankings show X but OpenRouter usage shows Y." Real intellectual friction, named sources on both sides.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔭 FORWARD SIGNAL
[3-4 bullets. What to watch next week. Dated events when possible: upcoming releases, conference talks, benchmark announcements, earnings calls with AI implications.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━

📚 SOURCES
[Linked list: [Source Name](url) · [Source Name](url) · ...]

📊 GENERATION STATS
• Sources consulted: X | Sources cited: Y | Scored: X included / Y excluded
• Research time: Xm Xs (Step 1: Xm, Step 2: Xm, Step 3: Xm, Step 4: Xm, Step 5: Xm)
• Tokens used: ~X input / ~Y output (~Z total)
• Estimated cost: $X.XX (based on model pricing at time of generation)
• Model: [model name + version used for this run, e.g. Claude Opus 4.7 via Amazon Bedrock]
```

## Delivery format

The same template is rendered into:

1. **Telegram inline message** — split at section boundaries if it exceeds 4096 chars.
2. **Markdown attachment** — `YYYY-MM-DD-slug.md`, the canonical version.
3. **S3 final artifact** — `s3://genai-reports-{ACCOUNT_ID}/{REPORT_DATE}/final.md`.
4. **Local mirror** — `{WORKSPACE}/projects/genai-report/reports/{REPORT_DATE}-{slug}.md` for grep/reference.
