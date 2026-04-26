# Domain 2 — Tool Design & MCP Integration (18%)

## Table of Contents
- [2.1 Tool Descriptions](#21-tool-descriptions)
- [2.2 Structured Error Responses](#22-structured-error-responses)
- [2.3 Tool Distribution & tool_choice](#23-tool-distribution--tool_choice)
- [2.4 MCP Server Configuration](#24-mcp-server-configuration)
- [2.5 Built-in Tools](#25-built-in-tools)

---

## 2.1 Tool Descriptions

### Core Concept
**Tool descriptions are the #1 mechanism LLMs use to select tools.** Vague, similar, or overlapping descriptions → Claude misroutes.

### Good vs Bad

❌ Bad (too similar):
```
get_customer: "Retrieves customer information"
lookup_order: "Retrieves order details"
```

✅ Good (differentiated):
```
get_customer: "Retrieves customer profile by email or phone. Returns name,
account status, loyalty tier, contact preferences. Use when you need to
identify or verify a customer. Do NOT use for order-specific data."

lookup_order: "Retrieves order details by order number (format: ORD-XXXXX).
Returns items, quantities, prices, shipping status, delivery date.
Use when the customer asks about a specific order."
```

### Anti-Patterns
- ❌ Minimal descriptions ("Retrieves data")
- ❌ Overlapping tool names (`analyze_content` vs `analyze_document` with similar descriptions)
- ❌ System prompt keywords overriding well-written tool descriptions
- ❌ Giving an agent 18 tools when it needs 4–5 (decision complexity degrades selection)

### Practice Questions

**Q2.1:** Your agent has `analyze_content` and `analyze_document`. Logs show 30% misroute. Both have similar descriptions. Most effective first step?

A) Few-shot examples showing correct routing
B) Rename tools and rewrite descriptions to clearly differentiate purpose, inputs, outputs, boundaries
C) Build a pre-routing classifier
D) Merge both into one generic `analyze` tool

<details><summary>Answer</summary>
**B.** Tool descriptions are the primary selection mechanism. Highest leverage, lowest effort. A adds tokens without fixing root cause. C is over-engineered. D may be valid long-term but isn't the right "first step."
</details>

---

## 2.2 Structured Error Responses

### Core Concept
When an MCP tool fails, return **structured error metadata**, not generic messages.

```json
{
  "isError": true,
  "errorCategory": "transient",
  "isRetryable": true,
  "description": "Database timeout after 30s. Query: get_order(ORD-12345)",
  "partialResults": null,
  "alternativeApproach": "Try lookup by customer email instead"
}
```

### Error Categories

| Category | Retryable? | Agent Action |
|---|---|---|
| **Transient** (timeout, service down) | Yes | Retry with backoff |
| **Validation** (bad input format) | No (same input) | Fix input and retry |
| **Business** (policy violation) | No | Explain to user / escalate |
| **Permission** (access denied) | No | Escalate or use alternative |

### Anti-Patterns
- ❌ Generic "Operation failed" messages (model can't decide recovery)
- ❌ Silently returning empty results as success (hides failures)
- ❌ Terminating entire workflows on a single tool failure

### Practice Questions

**Q2.2:** Web search subagent times out during research. How should this flow back to the coordinator?

A) Return structured error context: failure type, attempted query, partial results, alternatives
B) Retry with backoff, return "search unavailable" on final failure
C) Catch timeout, return empty result set marked as successful
D) Propagate the exception to terminate the workflow

<details><summary>Answer</summary>
**A.** Structured context enables intelligent coordinator recovery. B hides context. C silently suppresses failure. D is disproportionate.
</details>

---

## 2.3 Tool Distribution & tool_choice

### tool_choice Options

| Setting | Behavior | Use When |
|---|---|---|
| `"auto"` | Model may return text OR call a tool | Default — model decides |
| `"any"` | Model MUST call a tool (can choose which) | Guarantee structured output when multiple tools exist |
| `{"type": "tool", "name": "X"}` | Model MUST call this specific tool | Force a specific first step |

**Not a valid option:** `"required"` — does not exist in the Claude API.

### Tool Distribution Rules
- **4–5 tools per agent** is the sweet spot. 18 tools = unreliable selection.
- Each subagent gets only tools relevant to its role
- Exception: limited cross-role tools for high-frequency needs (e.g., `verify_fact` for synthesis agent)

### Practice Questions

**Q2.3:** Document extraction system where upload is unknown type (invoice/receipt/contract). Three extraction tools, one per type. What `tool_choice` ensures Claude always returns structured output?

A) `"auto"` — let Claude decide
B) `"any"` — force Claude to call one of the extraction tools
C) `{"type": "tool", "name": "extract_invoice"}` — force invoice extractor
D) Don't set tool_choice; add "always call a tool" to the prompt

<details><summary>Answer</summary>
**B.** `"any"` guarantees a tool call while letting Claude choose which based on type. `"auto"` might return text. C assumes type is known. D is probabilistic.
</details>

---

## 2.4 MCP Server Configuration

### Scoping

| Scope | Location | Shared? | Use For |
|---|---|---|---|
| **Project** | `.mcp.json` in repo root | ✅ Version controlled | Team tooling (Jira, GitHub, DB) |
| **User** | `~/.claude.json` | ❌ Personal only | Experimental / personal servers |

### Key Points
- Env vars expand in `.mcp.json`: `${GITHUB_TOKEN}`
- All configured servers are discovered at connection time and available simultaneously
- **MCP resources** expose content catalogs (issue summaries, docs, DB schemas) — reduces exploratory tool calls
- Prefer existing community MCP servers over building custom ones for standard integrations

### Practice Questions

**Q2.4:** New team member reports Claude Code doesn't have Jira access, but it works for everyone else. The MCP server is in `~/.claude.json`. Problem?

A) Their API token expired
B) `~/.claude.json` is user-scoped and not shared — config should be in `.mcp.json`
C) MCP server needs restart for new users
D) Claude Code only supports one MCP server at a time

<details><summary>Answer</summary>
**B.** User configs aren't shared. Team tooling belongs in `.mcp.json` (project-scoped, version controlled). Classic scoping trap.
</details>

---

## 2.5 Built-in Tools

| Tool | Purpose | Use When |
|---|---|---|
| **Grep** | Search file **contents** for patterns | Finding function callers, errors, imports |
| **Glob** | Find files by **name/path** pattern | Finding `**/*.test.tsx`, `**/config.*` |
| **Read** | Load full file contents | Examining a specific file |
| **Write** | Create/overwrite entire file | Creating new files |
| **Edit** | Targeted text replacement | Modifying specific sections (needs unique anchor) |
| **Bash** | Run shell commands | Tests, builds, scripts |

### Edit Fallback
When Edit fails due to non-unique text → use **Read + Write** as fallback.

### Codebase Exploration Pattern
1. **Grep** to find entry points (function names, error messages)
2. **Read** to follow imports and trace flows
3. Don't Read all files upfront — build understanding incrementally

### Practice Questions

**Q2.5:** You need to find all files containing the string "deprecated_api_call". Which tool?

A) Glob — find files matching pattern
B) Grep — search file contents
C) Bash — `find . -name "*deprecated*"`
D) Read — check each file manually

<details><summary>Answer</summary>
**B.** Grep = contents. Glob = filenames. The string is inside contents, not the filename.
</details>

---

## Domain 2 Recap

- Tool descriptions are the PRIMARY mechanism for tool selection.
- 4–5 tools per agent; more degrades selection.
- `tool_choice: "any"` = must call a tool. `"auto"` = might return text. `"required"` ≠ valid.
- `.mcp.json` = project (shared). `~/.claude.json` = user (personal).
- Structured errors with `errorCategory`, `isRetryable`, `partialResults` — not generic messages.
- Grep = content search. Glob = filename matching.
- Edit fails on non-unique anchors → fallback to Read + Write.
