# Domain 5 — Context Management & Reliability (15%)

## Table of Contents
- [5.1 Context Window Management](#51-context-window-management)
- [5.2 Escalation Patterns](#52-escalation-patterns)
- [5.3 Error Propagation in Multi-Agent Systems](#53-error-propagation-in-multi-agent-systems)
- [5.4 Information Provenance](#54-information-provenance)

---

## 5.1 Context Window Management

### Key Problems
- **"Lost in the middle"** — models reliably process beginning and end of long inputs, but miss content in middle sections
- **Progressive summarization risk** — condensing "$47.50 refund for order ORD-12345 placed March 3" into "customer wants a refund" loses critical facts
- **Tool result bloat** — an order lookup returns 40+ fields when only 5 are relevant

### Solutions

| Problem | Solution |
|---|---|
| Lost-in-the-middle | Place key findings at **beginning**; use explicit section headers |
| Summarization loss | Extract transactional facts into a persistent **"case facts" block** |
| Tool result bloat | Trim verbose outputs to **relevant fields only** (via hooks or prompt) |
| Cross-agent context limits | Subagents return **structured data** (key facts, citations, scores), not verbose reasoning chains |

### Practice Questions

**Q5.1:** During a long customer support session handling 3 issues, the agent correctly recalls the first and third but confuses details of the second (wrong order number, wrong amount). What's happening and how do you fix it?

A) Model is hallucinating — add "be accurate" to system prompt
B) Lost-in-the-middle effect — extract key facts (order IDs, amounts, statuses) into a persistent case facts block included in each prompt
C) Context window overflow — truncate older messages
D) Model needs few-shot examples of multi-issue handling

<details><summary>Answer</summary>
**B.** Classic lost-in-the-middle: the second issue sits in the middle. Extract structured facts into a persistent block at a consistent position.
</details>

---

## 5.2 Escalation Patterns

### When to Escalate
- ✅ Customer **explicitly asks for a human** → immediately honor it
- ✅ **Policy gap** — issue falls outside defined policies
- ✅ No **meaningful progress** after reasonable attempts
- ✅ Business rules require **human judgment** (exceptions, edge cases)

### When NOT to Escalate
- ❌ Sentiment analysis says customer is angry (sentiment ≠ complexity)
- ❌ Self-reported confidence is low (LLM confidence is poorly calibrated)
- ❌ Case seems "complex" but is actually standard (e.g., damage replacement with photo evidence)

### Structured Handoffs
When escalating, compile: customer ID, root cause analysis, what was attempted, recommended action. The human agent doesn't have the conversation transcript.

### Practice Questions

**Q5.2:** Agent achieves only 55% first-contact resolution (target 80%). Logs show it escalates simple cases (standard returns with evidence) while attempting complex policy exceptions autonomously. Fix?

A) Explicit escalation criteria with few-shot examples showing when to escalate vs resolve
B) Self-reported confidence to route low-confidence cases to humans
C) Separate classifier trained on historical tickets
D) Sentiment analysis to detect frustrated customers

<details><summary>Answer</summary>
**A.** Unclear decision boundaries. Explicit criteria + few-shot contrasts (simple → resolve, exception → escalate) address calibration directly. B fails because LLM confidence is poorly calibrated. C requires ML infra when prompt tuning hasn't been tried. D measures frustration, not complexity.
</details>

---

## 5.3 Error Propagation in Multi-Agent Systems

### Key Principle
Subagents should:
1. Handle transient errors locally (retry timeouts, etc.)
2. Only propagate errors they **can't resolve**
3. Include: failure type, what was attempted, partial results, alternatives

### Anti-Patterns
- ❌ Generic error messages ("search unavailable")
- ❌ Silently returning empty results as success
- ❌ Terminating entire workflow on single failure
- ❌ Not distinguishing access failures from valid empty results

### Practice Questions

**Q5.3:** Document analysis subagent searches a database and gets zero results. How should it report to the coordinator?

A) Error with `isRetryable: true`
B) Success response with empty results, clearly marked "no matching documents found"
C) Generic "search failed" error
D) Retry 3 times, then return timeout error

<details><summary>Answer</summary>
**B.** Zero results from a successful query is NOT an error — it's a valid empty result. Coordinator needs to distinguish "search worked, found nothing" from "search was unavailable."
</details>

---

## 5.4 Information Provenance

### Core Concept
When synthesizing from multiple sources, **preserve claim-source mappings**. Never compress findings to the point where you can't trace which source said what.

### Key Patterns
- Subagents output: `{claim, evidence, source_url, publication_date}`
- Conflicting statistics → **annotate both with their sources**, don't pick one
- Include publication/collection dates to prevent temporal confusion
- Distinguish well-established findings from contested ones in reports

### Practice Questions

**Q5.4:** Research system produces report stating "AI market grew 45% in 2025." Two credible sources were used — one says 38%, the other 52%. What went wrong and how do you fix it?

A) Synthesis averaged — add "don't average" to prompt
B) Synthesis lost source attribution during summarization — require structured claim-source mappings preserved through synthesis
C) One source is wrong — add fact-checking subagent
D) Synthesis should use the more recent source

<details><summary>Answer</summary>
**B.** Root cause is provenance loss during synthesis. Structured claim-source mappings force synthesis to preserve both values with sources ("Source A: 38%, Source B: 52%") so readers evaluate the conflict themselves.
</details>

---

## Domain 5 Recap

- Lost-in-the-middle: key facts at beginning, use headers.
- Extract transactional facts into persistent "case facts" block.
- Trim tool outputs to relevant fields (via hooks or prompt).
- Escalate: explicit customer ask, policy gap, no progress, human judgment required.
- NEVER escalate on: sentiment, self-reported confidence, "seems complex."
- Valid empty results ≠ errors. Distinguish them.
- Structured handoffs: customer ID + root cause + attempts + recommendation.
- Preserve claim-source mappings through synthesis. Annotate conflicts with sources.
