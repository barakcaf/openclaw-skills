# Design Doc Template

Use this template when producing a design doc in Step 3.

```markdown
# Design: <Feature Title>

## Summary
One paragraph: what we're building and why.

## Current State
How things work today. What's missing or broken.

## Proposed Design

### Approach
High-level approach and key decisions. Why this approach over alternatives.

### Components Changed
| Component | Change | Risk |
|-----------|--------|------|
| file/module | what changes | low/med/high |

### API / Interface Changes
New endpoints, modified schemas, changed props — if applicable.

### Data Model Changes
New tables, fields, indexes — if applicable.

## Alternatives Considered
What else was evaluated and why it was rejected. At least one alternative for non-trivial decisions.

## Testing Strategy
- Unit tests needed
- E2E tests needed
- Manual verification steps

## Rollout / Migration
Any deployment order dependencies, feature flags, data migrations.

## Open Questions
Anything that needs user input before implementation.
```
