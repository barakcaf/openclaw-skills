# Scoring Rubric Examples

Scoring makes the include/exclude decision transparent and auditable. Every result gets scored individually.

## SDLC Report (Weekly Research)

**Dimensions:**
- **Novelty** (0-3): Is this new information vs. last edition?
- **Evidence quality** (0-3): Primary data > anecdote > opinion > speculation
- **Theme relevance** (0-3): Direct hit on tracked themes > tangential

**Thresholds:** Include ≥5, discard ≤2

## GenAI Report (Weekly Research)

**Dimensions:**
- **Novelty** (0-3): New vs. previously covered?
- **Evidence quality** (0-3): Independent benchmark > lab claim > anecdote > speculation
- **Relevance** (0-3): Direct hit on tracked themes > tangential

**Thresholds:** Include ≥5, discard ≤2

## Stock Report (Daily Data-Driven)

**Dimensions:**
- **Relevance** (0-3): Does it explain a price move or sector trend?
- **Evidence** (0-3): Named source with data > analyst opinion > speculation
- **Timeliness** (0-3): Today's news > this week > older

**Thresholds:** Include ≥5

## Checkpoint Format

Every scoring step saves this structure:

```json
{
  "step": N,
  "name": "scoring",
  "startTime": "ISO-8601",
  "endTime": "ISO-8601",
  "totalResults": 85,
  "included": 24,
  "excluded": 61,
  "results": [
    {
      "url": "https://...",
      "title": "Article title",
      "source_step": "step-1-models",
      "dimension_1": 3,
      "dimension_2": 2,
      "dimension_3": 2,
      "total": 7,
      "decision": "include",
      "reasoning": "First independent RCT on topic, high-quality data"
    }
  ],
  "synthesis": {
    "convergence": ["3+ sources agree: ..."],
    "divergence": ["Source A says X, Source B says Y"],
    "lead_signal": "The single strongest new signal",
    "themes": {
      "theme_name": ["url1", "url2"]
    }
  }
}
```

## Designing Custom Rubrics

When the standard Novelty/Evidence/Relevance doesn't fit, ask:

1. What makes a source WORTH including? (that's your positive dimensions)
2. What makes a source NOISE? (that's your discard criteria)
3. What's the time sensitivity? (add Timeliness if daily, probably not if monthly)
4. Is there a domain-specific quality signal? (Severity for security, Impact for business, Reproducibility for science)

Keep to 3 dimensions. More than that and the scoring becomes noisy.
