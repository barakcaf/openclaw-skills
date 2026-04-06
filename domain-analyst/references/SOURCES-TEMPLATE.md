# Source Registry Template

Use this structure for every domain report's `sources.md`.

## Tier 1 — Must Check Every Run

| Source | Monitor | Tavily Query |
|--------|---------|-------------|
| [Name] | [RSS URL or "no RSS"] | `"query terms" after:{SEARCH_FROM}` |

## Tier 2 — Check on Signal

| Source | Tavily Query |
|--------|-------------|
| [Name] | `"query terms" after:{SEARCH_FROM}` |

## Tier 3 — Quarterly / Major Releases

| Source | Tavily Query |
|--------|-------------|
| [Name] | `"query terms"` |

## Source Evolution Rules

- **Promote to Tier 1:** Cited in 2+ consecutive editions with high signal
- **Promote to Tier 2:** Verifiable domain leadership, original data, covers tracked themes
- **Demote one tier:** No relevant results for 4+ consecutive runs
- **Remove:** Consistently low signal, broken feeds, defunct
- **Query refinement:** If a query stops returning good results, update it and note the change

## Notes

- All Tavily queries append `after:{SEARCH_FROM}` for the search window
- Fallback: search HN or X for reactions to any source
- Every run should include source discovery queries to find new voices
- The source list must grow — a static list is a failing list

## Discovered Sources Log

| Date | Source | Initial Tier | Notes |
|------|--------|-------------|-------|
| | | | |
