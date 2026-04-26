# Domain 1 — Agentic Architecture & Orchestration (27%)

_Highest weighted domain. Master this first. Exam loves the anti-patterns._

## Table of Contents
- [1.1 The Agentic Loop](#11-the-agentic-loop)
- [1.2 Multi-Agent Orchestration (Hub-and-Spoke)](#12-multi-agent-orchestration-hub-and-spoke)
- [1.3 Context Passing Between Agents](#13-context-passing-between-agents)
- [1.4 Hooks and Programmatic Enforcement](#14-hooks-and-programmatic-enforcement)
- [1.5 Task Decomposition and Session Management](#15-task-decomposition-and-session-management)

---

## 1.1 The Agentic Loop

### Core Concept
An agentic loop is the fundamental pattern for autonomous Claude agents:

```
Send request to Claude → Check stop_reason →
  If "tool_use" → Execute tool → Append result to conversation → Loop
  If "end_turn" → Present final response → Stop
```

**Claude decides** what to do next. You don't hardcode a sequence of tools.

### What Gets Tested
- `stop_reason = "tool_use"` → execute the tool, append result, continue
- `stop_reason = "end_turn"` → Claude is done
- Tool results are **appended to conversation history** so Claude can reason about them

### Anti-Patterns (exam favorites)
- ❌ Parsing natural language to detect completion ("if response contains 'I'm done'")
- ❌ Using iteration caps as the **primary** stopping mechanism
- ❌ Checking assistant text content as a completion indicator
- ❌ Pre-configuring tool sequences instead of letting Claude decide

### Practice Questions

**Q1.1:** Your agent processes customer requests but occasionally enters infinite loops, calling the same tool repeatedly with identical parameters. Most appropriate fix?

A) Parse the agent's text for "I've completed the task" to detect completion
B) Set a maximum of 5 tool calls and terminate the loop when reached
C) Continue using stop_reason, but add duplicate-call detection that surfaces the repetition to the model in the next turn
D) Replace the loop with a fixed tool sequence

<details><summary>Answer</summary>
**C.** The loop should always use stop_reason as primary control. Surfacing duplicate calls to the model lets it break out naturally. A is fragile NLP. B uses arbitrary caps as primary mechanism. D removes Claude's decision-making.
</details>

**Q1.2:** Agent has tools: `get_policy`, `validate_receipt`, `submit_expense`, `get_approval_status`. User says "Submit my lunch receipt for $45." The loop should:

A) Call all four tools in sequence automatically
B) Send request to Claude, which decides to call `validate_receipt` first, then based on the result decides the next step
C) Check the user's message for keywords and route to the right tool
D) Call `submit_expense` directly since the user asked to submit

<details><summary>Answer</summary>
**B.** The loop lets Claude reason about which tool based on context. Not keyword matching (C), fixed sequences (A), or skipping validation (D).
</details>

---

## 1.2 Multi-Agent Orchestration (Hub-and-Spoke)

### Core Concept
When a task is too complex for one agent, use a **coordinator** that delegates to **subagents**:

```
Coordinator
  ├── Search Subagent
  ├── Analysis Subagent
  └── Synthesis Subagent
```

**Critical rule:** All communication flows through the coordinator. Subagents never talk to each other directly.

### What Gets Tested
- Coordinator handles: task decomposition, delegation, result aggregation, error handling
- Subagents have **isolated context** — they do NOT inherit coordinator's history
- Context must be **explicitly passed** in subagent prompts
- Coordinator decides *which* subagents to invoke — not always all of them

### The Task Tool
- Subagents are spawned via the **Task tool**
- Coordinator's `allowedTools` must include `"Task"`
- **Parallel execution:** multiple Task calls in a **single response** (not across turns)

### Anti-Patterns
- ❌ Subagents talking directly to each other (bypassing coordinator)
- ❌ Assuming subagents inherit parent context automatically
- ❌ Always invoking all subagents regardless of complexity
- ❌ Overly narrow task decomposition (e.g., "AI in creative industries" → only visual arts)

### Practice Questions

**Q1.3:** Multi-agent research system with coordinator, web search, document analysis, and synthesis agents researches "renewable energy trends." The synthesis produces a report covering only solar and wind — missing hydro, geothermal, nuclear. Each subagent completed successfully within its scope. Root cause?

A) Synthesis needs better coverage-gap instructions
B) Web search queries were too narrow
C) Coordinator's task decomposition didn't cover all relevant subtopics
D) Document analysis filtered out non-solar/wind sources

<details><summary>Answer</summary>
**C.** When subagents complete successfully but coverage is narrow, look at what they were *assigned*, not how they executed. Always look at the coordinator's decomposition first.
</details>

**Q1.4:** How to run web search and document analysis subagents in parallel?

A) Two separate coordinator agents, one per subagent
B) Coordinator emits multiple Task tool calls in a single response
C) Spawn subagents in separate threads via programming language threading
D) First subagent triggers the second

<details><summary>Answer</summary>
**B.** Parallel subagent execution = multiple Task calls in one coordinator response turn. SDK-native approach. D violates hub-and-spoke.
</details>

---

## 1.3 Context Passing Between Agents

### Core Concept
Subagents start with a **blank slate**. Everything they need must be in their prompt:
- Complete findings from prior agents
- Structured data with metadata (URLs, document names, dates)
- Research goals and quality criteria

### What Gets Tested
- Pass findings **directly in the prompt** — not by reference, not via shared memory
- Use **structured data** to preserve attribution
- Coordinator prompts specify **goals and quality criteria**, not step-by-step procedures
- `fork_session` for divergent exploration from a shared baseline

### Practice Questions

**Q1.5:** Synthesis subagent needs results from web search + document analysis subagents. Coordinator should pass this info by:

A) Storing results in a shared database that all subagents access
B) Including complete findings from both agents directly in the synthesis subagent's prompt
C) Having synthesis call the other subagents to get their results
D) Passing only a summary to reduce token usage

<details><summary>Answer</summary>
**B.** Context goes in the prompt. Shared DBs break isolation (A). Direct subagent-to-subagent calls violate hub-and-spoke (C). Summaries lose critical details like source URLs (D).
</details>

---

## 1.4 Hooks and Programmatic Enforcement

### Core Concept
Two ways to enforce rules:
1. **Prompt instructions** — probabilistic, can fail ("Always verify customer")
2. **Programmatic hooks** — deterministic, guaranteed (code that blocks until verification returns)

**Exam rule:** When compliance MUST be guaranteed (financial ops, identity verification, policy enforcement), use hooks. Prompts have non-zero failure rate.

### Hook Types
- **PostToolUse hooks** — intercept tool results *after* execution, *before* the model sees them
  - Normalize timestamps (Unix → ISO 8601)
  - Trim verbose API responses to relevant fields
- **Tool call interception hooks** — block outgoing tool calls that violate rules
  - Block refunds > $500, redirect to escalation

### Practice Questions

**Q1.6:** Production shows your agent skips customer verification in 12% of cases, causing occasional incorrect refunds. Most effective fix?

A) Programmatic prerequisite blocking `lookup_order` and `process_refund` until `get_customer` returns a verified ID
B) Enhance system prompt to emphasize verification is mandatory
C) Few-shot examples showing verification first
D) Routing classifier to enable only appropriate tools per request type

<details><summary>Answer</summary>
**A.** Financial consequences require programmatic enforcement. B and C rely on the LLM following instructions (already fails 12%). D addresses tool availability, not ordering.
</details>

**Q1.7:** PostToolUse hook normalizes date formats from different backend tools. Where does it execute?

A) Before the tool is called, modifying input parameters
B) After the tool returns, transforming the result before the model sees it
C) In the model's context as a system instruction
D) As a separate API call after each agent turn

<details><summary>Answer</summary>
**B.** PostToolUse hooks intercept results after execution, before the model processes them.
</details>

---

## 1.5 Task Decomposition and Session Management

### Key Patterns
- **Prompt chaining** (fixed sequential): predictable multi-step workflows (per-file review → cross-file pass)
- **Dynamic adaptive decomposition**: open-ended tasks where next steps depend on discoveries

### Session Management
- `--resume <session-name>` — continue a prior conversation
- `fork_session` — independent branches from a shared analysis baseline
- On resume: inform the agent about file changes since last session
- Stale context: start fresh with injected summaries instead of resuming

### Practice Questions

**Q1.8:** You've spent an hour exploring a codebase and identified 3 potential root causes for a bug. You need to test two different fix approaches. Best approach?

A) Continue in the same session and test both sequentially
B) Use `fork_session` to create two branches from the current analysis point, test one in each
C) Two completely new sessions, re-explain the bug in each
D) Resume the session twice with different names

<details><summary>Answer</summary>
**B.** `fork_session` creates independent branches from a shared baseline — perfect for divergent approaches without losing investigation context.
</details>

---

## Domain 1 Recap

- `stop_reason: "tool_use"` → continue. `"end_turn"` → stop. Never use text parsing.
- Hub-and-spoke only. Subagents never talk to each other.
- Parallel subagents = multiple Task calls in ONE coordinator response.
- Subagents have isolated context. Pass everything in the prompt.
- Hooks = deterministic. Prompts = probabilistic. Use hooks for compliance.
- PostToolUse hooks transform results AFTER execution, BEFORE the model sees them.
- `fork_session` for divergent exploration from shared baseline.
