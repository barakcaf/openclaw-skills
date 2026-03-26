# Skill Audit Checklist

Based on the [AgentSkills spec](https://agentskills.io/specification) and the skill-creator best practices. Use this when auditing SKILL.md files.

Last updated: 2026-03-26

## Structure

- [ ] SKILL.md exists at skill directory root
- [ ] Directory name matches `name` field in frontmatter
- [ ] No extraneous files (README.md, CHANGELOG.md, INSTALLATION_GUIDE.md, QUICK_REFERENCE.md)
- [ ] Scripts in `scripts/`, references in `references/`, assets in `assets/`
- [ ] Only resource directories that are actually used exist (no empty dirs)

## Frontmatter

- [ ] `name` field: lowercase, letters/digits/hyphens, ≤64 chars, no leading/trailing hyphens
- [ ] `description` field: non-empty, ≤1024 chars
- [ ] Description says what the skill does
- [ ] Description says when to use it (trigger phrases, contexts)
- [ ] Description includes NOT-for exclusions (what it's not for)
- [ ] No extra fields beyond spec (`name`, `description`, `license`, `compatibility`, `metadata`, `allowed-tools`)

## Body — Size & Structure

- [ ] Under 500 lines (warning at 300+, ideal under 200)
- [ ] Markdown headers (H2/H3) for organization
- [ ] Bullet points over dense paragraphs
- [ ] One concern per section

## Body — Voice & Conciseness

- [ ] Imperative voice throughout ("Read the file", "Check for X", not "The agent should read")
- [ ] No information the agent already knows (don't explain how git/gh/curl works)
- [ ] Concise examples over verbose explanations
- [ ] Each paragraph justifies its token cost — challenge: "Does the agent really need this?"

## Body — Instructions

- [ ] Clear step numbering for sequential workflows
- [ ] Appropriate degrees of freedom:
  - Low freedom (exact scripts) for fragile/error-prone operations
  - High freedom (text guidance) for context-dependent decisions
- [ ] Concrete "NEVER" rules for dangerous patterns
- [ ] No "When to Use This Skill" section in body (belongs in frontmatter description)

## Progressive Disclosure

- [ ] SKILL.md contains only the essentials — workflow + navigation
- [ ] Heavy content (templates, detailed guides, API docs) in `references/`
- [ ] References clearly linked from SKILL.md with "when to read" context
- [ ] Reference index or table if 3+ reference files
- [ ] Reference files have table of contents if over 100 lines
- [ ] References one level deep (no references referencing other references)
- [ ] No duplication between SKILL.md body and reference files

## Scripts

- [ ] Scripts are for tasks that need deterministic reliability or get rewritten repeatedly
- [ ] Scripts are tested and functional
- [ ] Config via environment variables (not hardcoded values)

## Bootstrap / Config (if applicable)

- [ ] First-run config questions are grouped: required first, then optional with defaults
- [ ] Not too many questions at once (≤5 required, rest optional)
- [ ] Config stored in memory for persistence across sessions
- [ ] Users can update config values at any time

## Common Anti-Patterns

Flag these if found:

- ❌ Duplicating SKILL.md content in a reference file (or vice versa)
- ❌ "When to Use" section in the body (should be in description)
- ❌ README.md or CHANGELOG.md in the skill directory
- ❌ Hardcoded repo/project names in a reusable skill
- ❌ Over-explaining tools the agent already knows (git, curl, gh CLI syntax)
- ❌ Multiple overlapping sections saying the same thing different ways
- ❌ Deeply nested reference chains (reference A links to reference B links to reference C)
- ❌ Empty resource directories
