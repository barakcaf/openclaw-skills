# Skill Best Practices — Recommendations

Compiled from the AgentSkills spec, Anthropic skill-creator guidelines, and community patterns. Apply these when auditing skills.

Last researched: 2026-03-26

## Sources
- AgentSkills spec (agentskills.io/specification)
- Anthropic skill-creator (bundled with Claude Code / OpenClaw)
- Claude Code skills docs (code.claude.com/docs/en/skills)
- OpenClaw skills docs (docs.openclaw.ai/skills)
- Community patterns from ClawHub, Cursor, Gemini CLI, OpenHands skill ecosystems

## Recommendations

### 1. Frontmatter Is Your Trigger
- **MUST**: `description` includes both what the skill does AND specific trigger phrases/contexts
- **MUST**: Include NOT-for exclusions so the agent knows when NOT to load the skill
- The body is only loaded after triggering — "When to Use" sections in the body are wasted tokens
- ❌ Description that only says what it does: "Manages PDFs"
- ✅ Description with triggers: "Extract text, fill forms, merge PDFs. Use when user mentions PDFs, document extraction, or form filling. NOT for image processing."

### 2. Context Window Is a Public Good
- **MUST**: Only add information the agent doesn't already know
- Challenge each paragraph: "Does this justify its token cost?"
- Default assumption: the agent is already very smart
- Prefer concise examples over verbose explanations
- ❌ Explaining how `git commit` or `curl` works
- ✅ Explaining project-specific conventions or non-obvious tool interactions

### 3. Progressive Disclosure
- **MUST**: Keep SKILL.md under 500 lines (under 200 preferred)
- **MUST**: Split heavy content into `references/` — loaded only when needed
- **MUST**: Reference files clearly linked from SKILL.md with "when to read" guidance
- **SHOULD**: Add a reference index table if 3+ reference files
- **SHOULD**: Table of contents in reference files over 100 lines
- ❌ 400-line SKILL.md with embedded templates and full API docs
- ✅ Lean SKILL.md with "See [references/template.md] when generating the config"

### 4. Degrees of Freedom
- **MUST**: Match specificity to task fragility
- Low freedom (exact scripts, few parameters) for fragile/error-prone operations
- High freedom (text guidance) for context-dependent decisions
- Think of the agent on a path: narrow bridge = guardrails, open field = many routes
- ❌ Exact script for "decide which framework to use"
- ✅ Exact script for "rotate this PDF 90 degrees"

### 5. Imperative Voice
- **MUST**: Use imperative/infinitive form throughout
- "Read the file", "Check for X", "Generate the config"
- ❌ "The agent should read the file" / "This file is read by the agent"
- ✅ "Read the file. Check for X. If missing, generate it."

### 6. No Extraneous Files
- **MUST**: Only files that directly support the skill's function
- No README.md, CHANGELOG.md, INSTALLATION_GUIDE.md, QUICK_REFERENCE.md
- The skill is for an AI agent, not a human developer browsing a repo
- ❌ README.md explaining how to install the skill
- ✅ Just SKILL.md + references/ + scripts/ as needed

### 7. Scripts for Determinism
- **SHOULD**: Include scripts when the same code gets rewritten repeatedly
- **SHOULD**: Config via environment variables, not hardcoded values
- Scripts can be executed without loading into context (token-efficient)
- Test scripts before including them
- ❌ Hardcoded API URLs and credentials in scripts
- ✅ Scripts driven by env vars with sensible defaults

### 8. Bootstrap Pattern
- **SHOULD**: Group config questions — required first (≤5), then optional with defaults
- **SHOULD**: Store config in memory for cross-session persistence
- **SHOULD**: Allow users to update any setting at any time
- Don't overwhelm with 15 questions at once
- ❌ "Answer these 15 questions before we can start"
- ✅ "3 required questions, then: here are defaults for the rest — customize any?"

### 9. One Concern Per Section
- **SHOULD**: Don't mix unrelated topics in a single section
- Each section has a clear purpose and audience
- ❌ A "Rules" section covering testing, security, git workflow, and CSS
- ✅ Separate sections: "Testing", "Security", "Git Workflow"

### 10. References Over Duplication
- **MUST**: Information lives in either SKILL.md or a reference file, not both
- **MUST**: References one level deep (no chains: ref A → ref B → ref C)
- Cross-reference between files, don't repeat
- ❌ Same template in SKILL.md body AND references/template.md
- ✅ Template in references/template.md, SKILL.md says "Use the template in [references/template.md]"

### 11. Clear Step Numbering
- **SHOULD**: Sequential workflows use numbered steps
- Each step has a clear entry condition and exit condition
- Branch points explicitly state what to do in each case
- ❌ Vague flow: "First do this, then maybe do that, or sometimes this other thing"
- ✅ "Step 3: If approved → Step 4. If changes requested → revise and re-present. If rejected → stop."

### 12. Hard Boundaries
- **MUST**: Include "NEVER" rules for dangerous patterns specific to the skill's domain
- Boundaries are the most impactful content (per community research)
- ❌ Leaving dangerous patterns unmentioned
- ✅ "NEVER hardcode credentials", "NEVER cancel in-progress fix workflows"

### 13. Naming Conventions
- **MUST**: Skill name matches directory name
- **MUST**: Lowercase, letters/digits/hyphens, ≤64 chars
- **SHOULD**: Short, verb-led phrases describing the action
- **SHOULD**: Namespace by tool when it improves clarity (e.g., `gh-address-comments`)
- ❌ `PDF-Processing`, `-pdf`, `pdf--processing`
- ✅ `pdf-processing`, `bedrock-pr-review`, `feature-workflow`
