# Concept Primer — Zero Assumptions

_Read this FIRST if your baseline assessment score is 5/15 or lower, or if you've never worked with Claude's Agent SDK, MCP, or Claude Code beyond basic chat. The domain reference files assume you already know these fundamentals._

## Table of Contents
1. [What is Claude Code?](#1-what-is-claude-code)
2. [What is the Agent SDK?](#2-what-is-the-agent-sdk)
3. [What is a "Tool" in Claude terms?](#3-what-is-a-tool-in-claude-terms)
4. [What is MCP?](#4-what-is-mcp)
5. [What is an "Agentic Loop"?](#5-what-is-an-agentic-loop)
6. [What is a Subagent?](#6-what-is-a-subagent)
7. [What is CLAUDE.md?](#7-what-is-claudemd)
8. [What is a Skill in Claude Code?](#8-what-is-a-skill-in-claude-code)
9. [What is a "Hook"?](#9-what-is-a-hook)
10. [What is "Structured Output"?](#10-what-is-structured-output)
11. [What does "tool_choice" mean?](#11-what-does-tool_choice-mean)
12. [Glossary](#12-glossary)

---

## 1. What is Claude Code?

**Claude Code** is a command-line tool (CLI) that lets Claude read, write, and edit files on your computer, run shell commands, and do work on a codebase — not just chat. It's distributed by Anthropic.

You run it in a terminal:
```bash
claude            # starts an interactive session
claude -p "..."   # runs one prompt non-interactively (for scripts/CI)
```

Think of it as "ChatGPT in a terminal that can actually touch your files."

The exam tests configuration details (where its config files live, how to scope rules, how to run it in CI) — not the act of using it.

---

## 2. What is the Agent SDK?

The **Anthropic Agent SDK** (Python and TypeScript) is a library for building your OWN agent applications — not Claude Code itself. You call it from your code:

```python
from anthropic import Anthropic
client = Anthropic()
response = client.messages.create(
    model="claude-sonnet-4-6",
    tools=[...],
    messages=[{"role": "user", "content": "..."}]
)
```

When you hear "agent," "coordinator," "subagent," "tool," or "Task tool" in the exam, it's usually in the context of the SDK, not Claude Code.

**Key distinction:**
- **Claude Code** = the CLI tool you use as a user/developer
- **Agent SDK** = the library you use to build agents into your own products

Both show up on the exam. Keep them separate in your head.

---

## 3. What is a "Tool" in Claude terms?

A **tool** is a function Claude can call. You define it with a name, description, and input schema:

```json
{
  "name": "get_weather",
  "description": "Get current weather for a city",
  "input_schema": {
    "type": "object",
    "properties": {"city": {"type": "string"}},
    "required": ["city"]
  }
}
```

When Claude decides to call it, your code runs the function and sends the result back. Then Claude decides what to do next.

**Why this matters:** Claude picks which tool to call based almost entirely on the **description** field. Bad descriptions → wrong tool calls. This is the #1 tested concept in Domain 2.

---

## 4. What is MCP?

**MCP = Model Context Protocol.** It's an open standard Anthropic created for connecting external systems (databases, APIs, dev tools) to AI assistants in a standardized way.

An **MCP server** is a program that exposes tools and data. Claude Code or your agent connects to it.

Common examples:
- Jira MCP server → lets Claude read/write Jira tickets
- GitHub MCP server → lets Claude interact with repos
- AWS docs MCP server → lets Claude search AWS documentation

**Two ways to configure MCP servers in Claude Code:**
| Config file | Scope | Shared with team? |
|---|---|---|
| `.mcp.json` in your repo | Project | ✅ Yes (via git) |
| `~/.claude.json` (home dir) | User | ❌ No (personal) |

**Exam tip:** if a scenario describes "new team member doesn't have access to Jira MCP," the answer is almost always "it's in user-scope, move it to project-scope." Classic question.

---

## 5. What is an "Agentic Loop"?

When Claude uses tools, it happens in a loop:

```
You: "Submit my expense report"
  ↓
Claude → decides to call `validate_receipt(receipt.pdf)`
  ↓
Your code → runs the function, gets back "valid"
  ↓
You send that result back to Claude
  ↓
Claude → decides to call `submit_expense(...)` next
  ↓
Your code → runs it, gets "success"
  ↓
You send that back
  ↓
Claude → responds to you: "Your expense was submitted"
  ↓
STOP
```

That entire cycle is the **agentic loop**.

The signal to continue or stop comes from Claude's `stop_reason`:
- `"tool_use"` → Claude wants to call a tool, execute it, continue
- `"end_turn"` → Claude is done, stop

**Exam rule:** always use `stop_reason` to drive the loop. Never parse Claude's text for "I'm done" or "task complete" — that's fragile and always the wrong answer.

---

## 6. What is a Subagent?

For complex tasks, one agent often delegates to others. This is the **coordinator + subagents** pattern (a.k.a. "hub-and-spoke"):

```
Coordinator ("research renewable energy")
  ├─ Search subagent ("find recent papers on solar")
  ├─ Search subagent ("find recent papers on wind")
  └─ Synthesis subagent ("combine findings into a report")
```

**Critical rules:**
1. Subagents are spawned via the **Task tool**. The coordinator's `allowedTools` must include `"Task"`.
2. Subagents have **isolated context** — they don't see the coordinator's conversation history. You must pass everything they need in their prompt.
3. Subagents **never talk to each other directly**. All communication flows through the coordinator.
4. To run multiple subagents in parallel, the coordinator emits **multiple Task calls in a single response** — not across multiple turns.

**Why isolated context?** Subagents get a fresh context window. This keeps them focused but means you lose all the coordinator's work if you forget to pass it along. This is tested heavily.

---

## 7. What is CLAUDE.md?

**CLAUDE.md** is a markdown file Claude Code reads automatically when you start a session. It's where you put instructions like "always use tabs" or "our API uses async/await with try/catch."

**Where does it live?** Multiple places:

```
~/.claude/CLAUDE.md              ← YOUR personal settings (not shared)
./CLAUDE.md   OR   .claude/CLAUDE.md  ← TEAM settings (shared via git)
subdirectory/CLAUDE.md           ← Folder-specific settings
.claude/rules/*.md               ← Focused rule files (can use glob scoping)
```

All of these **concatenate** into Claude Code's context — they don't override each other.

**Exam traps:**
- `~/.claude/CLAUDE.md` is **user-only**. Teammates don't see it. If the exam says "new hire doesn't have standards," the fix is moving to project-scope.
- `.claude/rules/` files can include YAML frontmatter like `paths: ["**/*.test.*"]` — this makes the rule load ONLY when editing matching files (saves tokens). Great for cross-cutting conventions like test style.

---

## 8. What is a Skill in Claude Code?

A **skill** is a reusable workflow you bundle as a directory:

```
.claude/skills/my-skill/
  SKILL.md       ← instructions + metadata (YAML frontmatter)
  scripts/       ← optional: reusable scripts
  references/    ← optional: docs the skill might load
```

You invoke a skill by typing `/my-skill` in Claude Code, or Claude can auto-invoke it when the description matches.

**Key frontmatter options (exam-tested):**

| Option | What it does | When to use |
|---|---|---|
| `context: fork` | Runs the skill in an isolated subagent context | Verbose output that would fill main context |
| `allowed-tools: Read Grep` | Restrict which tools the skill can use | Skills that should never write files |
| `disable-model-invocation: true` | Only YOU can trigger it — Claude can't auto-invoke | Destructive operations, expensive operations |
| `argument-hint: "file path"` | Prompts the user for required args | Skills that need input to work |

**Skills vs CLAUDE.md:**
- CLAUDE.md = always loaded, universal standards
- Skills = on-demand, task-specific workflows

**Skills vs slash commands:**
- Commands (`.claude/commands/foo.md`) = simple slash commands
- Skills (`.claude/skills/foo/SKILL.md`) = same idea but with a directory for assets/scripts
- If both named `foo` exist, **skill wins**

---

## 9. What is a "Hook"?

A **hook** is deterministic code that runs at a specific point in the agent loop. Two main kinds:

**PostToolUse hooks** — run AFTER a tool returns, BEFORE Claude sees the result.
- Example: normalize dates from Unix timestamps to ISO 8601 across different tools
- Example: trim a 40-field API response down to the 5 fields you care about

**Tool-call interception hooks** — run BEFORE a tool is executed, can block it.
- Example: block any `process_refund` over $500, force escalation

**Why hooks matter:** prompts are probabilistic ("always verify the customer first" can fail 12% of the time). Hooks are deterministic (code that refuses to run the second tool until the first returned the right thing).

**Exam rule:** when compliance matters (financial operations, identity verification, regulated workflows), the correct answer is almost always "hook," not "better prompt."

---

## 10. What is "Structured Output"?

Structured output means Claude returns **JSON matching a defined schema**, not free-form text.

The most reliable way to get it: define a tool with a JSON schema, and make Claude call that tool. The schema enforces structure automatically.

```python
tools = [{
    "name": "extract_invoice",
    "description": "Extract invoice fields from a document",
    "input_schema": {
        "type": "object",
        "properties": {
            "invoice_number": {"type": "string"},
            "total_amount": {"type": "number"},
            "phone_number": {"type": ["string", "null"]}   # ← nullable!
        }
    }
}]
```

**Key trick the exam tests:** when a field might not exist in the source data, make it **optional or nullable**. Otherwise, Claude will fabricate a plausible-looking value to satisfy the schema. This is one of the most-tested prompt-engineering concepts.

---

## 11. What does "tool_choice" mean?

`tool_choice` controls whether Claude MUST call a tool:

| Setting | Behavior |
|---|---|
| `"auto"` (default) | Claude may call a tool OR return text |
| `"any"` | Claude MUST call some tool (its choice of which) — guarantees structured output |
| `{"type": "tool", "name": "extract_invoice"}` | Claude MUST call this specific tool |

**NOT a valid option:** `"required"` — this doesn't exist in the Claude API, but the exam plants it as a distractor.

**When to use `"any"`:** when you have multiple tools (e.g., extractors for invoices, receipts, contracts) and you need Claude to use ONE of them, but you don't know in advance which is correct. Forces structured output without forcing a specific tool.

---

## 12. Glossary

| Term | Meaning |
|---|---|
| **Agent** | A system built on an LLM that uses tools to act in a loop |
| **Agentic loop** | The cycle of prompt → tool use → observe result → decide next action |
| **allowedTools** | The list of tools an agent (or subagent) is permitted to call |
| **Coordinator** | An agent that delegates work to subagents |
| **CLAUDE.md** | Markdown file Claude Code reads automatically for instructions |
| **Fork session** | Branching a conversation into independent paths from a shared baseline |
| **Glob pattern** | File-matching pattern like `**/*.test.tsx` |
| **Hook** | Deterministic code that runs at a specific lifecycle point |
| **JSON schema** | Formal definition of a JSON structure (fields, types, required/optional) |
| **MCP** | Model Context Protocol — standard for connecting external systems to LLMs |
| **MCP server** | Program exposing tools/data via MCP (Jira, GitHub, databases, etc.) |
| **Plan mode** | Claude Code mode for investigating before making changes |
| **PostToolUse hook** | Hook that runs after a tool returns, before Claude sees the result |
| **Prompt chaining** | Fixed sequential workflow (output of step N becomes input of step N+1) |
| **Skill** | Bundled workflow directory with SKILL.md + optional scripts/references |
| **stop_reason** | The reason Claude ended its turn (`"tool_use"`, `"end_turn"`, etc.) |
| **Structured output** | Model output conforming to a defined schema (usually via tool_use) |
| **Subagent** | An agent spawned by a coordinator via the Task tool |
| **System prompt** | Top-level instructions that shape the model's behavior |
| **Task tool** | Built-in tool used to spawn subagents |
| **Tool** | A function definition (name + description + schema) Claude can call |
| **tool_choice** | Setting that controls whether Claude must call a tool |
| **tool_use** | Content block type when Claude wants to call a tool |
| **Validation-retry** | Pattern of extract → validate → re-extract with error context if invalid |

---

## What to Read Next

Once you've read this primer and can answer the [baseline-assessment.md](baseline-assessment.md) questions with ≥9/15 confident correct:

1. Start with **Domain 1 — Agentic Architecture** ([domain-1-agentic-architecture.md](domain-1-agentic-architecture.md)) since it's the highest-weighted (27%) and foundational to all others.
2. Then in weight order: Domain 3 → 4 → 2 → 5.
3. Revisit this primer's glossary any time a term feels unfamiliar.
