# Steering File Recommendations

Compiled from research across 7+ sources. Apply these on every steering file update.

Last researched: 2026-03-26

## Sources
- Anthropic official docs (code.claude.com/docs/en/memory, /best-practices)
- GitHub blog: "How to write a great agents.md" (2,500+ repos analyzed)
- Builder.io: "How to Write a Good CLAUDE.md File"
- eesel.ai: "7 Claude Code best practices for 2026"
- kylestratis.com: "A Better Practices Guide to Using Claude Code"
- Medium/Rick Hightower: "Claude Code Rules: Stop Stuffing Everything into One CLAUDE.md"
- Internal lessons: MEMORY.md, daily notes

## Recommendations

### 1. Size — Keep It Short
- **MUST**: Under 200 lines per file (Anthropic official recommendation)
- Longer files consume more context and reduce adherence
- ❌ 400-line CLAUDE.md with everything in one file

### 2. Commands First
- **MUST**: Put build/test/lint commands at the top, before philosophy or conventions
- GitHub blog: commands first is the #1 differentiator in effective agent files
- ❌ Burying `npm run test` on line 80 after three sections of architecture docs

### 3. Specificity Over Vagueness
- **MUST**: Write instructions concrete enough to verify
- ❌ "Format code properly" — use "Use 2-space indentation, no tabs"
- ❌ "Run the tests" — use `cd backend && pytest -v`

### 4. Explicit Boundaries
- **MUST**: Include "never" rules — files to never touch, patterns to never use, actions to never take
- Boundaries are the most impactful section (GitHub blog, 2,500 repos)
- ❌ Leaving dangerous patterns unmentioned and hoping the agent guesses

### 5. Structure With Headers and Bullets
- **MUST**: Use markdown headers (H2/H3) and bullet points — agents scan structure like humans
- ❌ Dense paragraphs of prose explaining coding conventions

### 6. Real Examples
- **SHOULD**: Include code examples for conventions — examples beat abstract descriptions
- Show the pitfall, not just the rule
- ❌ "Follow good naming conventions" without showing what that looks like

### 7. One Concern Per Section
- **SHOULD**: Don't mix build commands with deployment rules with code style
- Group related rules, separate unrelated ones
- ❌ A "Rules" section that covers testing, security, git workflow, and CSS in one list

### 8. Verification Instructions
- **MUST**: Include how to verify work — test commands, expected outputs, success criteria
- Anthropic calls this the "single highest-leverage" practice
- ❌ No test commands anywhere in the steering file

### 9. Modular When Growing
- **SHOULD**: Split into `.claude/rules/` or `@imports` when approaching 200 lines
- Subdirectory CLAUDE.md files load on-demand (only when agent works in that directory)
- ❌ One 400-line file covering frontend, backend, infra, and CI rules

### 10. Evolve Through Iteration
- **SHOULD**: Add rules when the agent makes mistakes, not upfront
- "Start simple. Test it. Add detail when your agent makes mistakes."
- ❌ Trying to write the perfect steering file before any code is written

### 11. No Duplicates Across Files
- **MUST**: A rule lives in exactly one file — cross-reference, don't repeat
- See File Ownership table in SKILL.md for which file owns what
- ❌ Same merge rule in both CLAUDE.md and WORKFLOW.md with slightly different wording

### 12. Plan Before Code
- **SHOULD**: Explore → plan → implement → verify (Anthropic best practices)
- Complex changes benefit from planning phase; trivial changes can skip it
- ❌ Jumping straight to implementation on a multi-file feature

### 13. Rule Format
- **MUST**: Every rule uses a severity level (**MUST** = hard blocker, **SHOULD** = strong default) and includes a ❌ pitfall example
- Template: `- **MUST/SHOULD**: [one-line rule] — [why]` followed by `  - ❌ common pitfall`
- ❌ Rules with no severity level and no concrete example of what to avoid

### 14. Bridge AGENTS.md Into CLAUDE.md
- **MUST**: If repo has both CLAUDE.md and AGENTS.md, add `@AGENTS.md` import to CLAUDE.md
- Claude Code reads CLAUDE.md but ignores AGENTS.md — without the import, AGENTS.md content is invisible
- The `@import` bridges them without duplicating content
- ❌ Having an AGENTS.md with architecture/conventions that Claude Code never sees
