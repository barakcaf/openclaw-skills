# Domain 4 — Prompt Engineering & Structured Output (20%)

## Table of Contents
- [4.1 Explicit Criteria Over Vague Instructions](#41-explicit-criteria-over-vague-instructions)
- [4.2 Few-Shot Prompting](#42-few-shot-prompting)
- [4.3 Structured Output via tool_use](#43-structured-output-via-tool_use)
- [4.4 Validation-Retry Loops](#44-validation-retry-loops)
- [4.5 Message Batches API](#45-message-batches-api)
- [4.6 Multi-Pass Review](#46-multi-pass-review)

---

## 4.1 Explicit Criteria Over Vague Instructions

### Core Concept
"Be conservative" and "only report high-confidence findings" are **useless**. Define **exactly** what to report:

❌ Vague: "Check that comments are accurate"
✅ Explicit: "Flag comments only when claimed behavior contradicts actual code behavior"

❌ Vague: "Only report important issues"
✅ Explicit: "Report: bugs, security vulnerabilities, data loss risks. Skip: style preferences, minor naming, local patterns"

### False Positive Management
High FP rates destroy developer trust — even in accurate categories.
- Temporarily disable high-FP categories while improving prompts
- Define severity criteria with concrete code examples
- Add `detected_pattern` field to findings for systematic FP analysis

### Practice Questions

**Q4.1:** Automated code review produces useful security findings but also 60% false positives for "inconsistent naming." Developers ignore ALL findings including valid security ones. Most effective immediate action?

A) Add "be more conservative" to the review prompt
B) Temporarily disable the naming category to restore trust, while improving its prompts separately
C) Require developers to confirm each finding before it's posted
D) Switch to a more capable model

<details><summary>Answer</summary>
**B.** High FP in one category erodes trust across ALL categories. Temporarily disable to restore confidence in valid findings. A is vague. C adds manual overhead. D doesn't address prompt quality.
</details>

---

## 4.2 Few-Shot Prompting

### When to Use
- Natural language descriptions produce **inconsistent** results
- Ambiguous cases need judgment (routing decisions, edge cases)
- Format consistency matters
- Reducing hallucination in extraction tasks

### How Many
**2–4 targeted examples.** Show reasoning for ambiguous cases — why one option over plausible alternatives.

### What Makes Good Examples
- Cover **ambiguous** cases, not obvious ones
- Show desired output **format** exactly
- Demonstrate varied document structures
- Distinguish acceptable patterns from genuine issues

### Practice Questions

**Q4.2:** Extraction system handles invoices reliably but fails on receipts with informal formatting (e.g., "about $50" instead of "$50.00"). Most effective improvement?

A) Add rule: "always extract exact amounts"
B) 2–3 few-shot examples showing correct extraction from informally formatted documents
C) Pre-process documents to normalize formatting before extraction
D) Confidence threshold to reject low-confidence extractions

<details><summary>Answer</summary>
**B.** Few-shot examples are most effective for varied structures. Show the model how to interpret informal formats. A restates the problem. C is over-engineered. D rejects rather than solves.
</details>

---

## 4.3 Structured Output via tool_use

### The Hierarchy of Reliability
1. **tool_use with JSON schemas** — guaranteed schema compliance, no JSON syntax errors. Best.
2. **`tool_choice: "any"`** — guarantees a tool is called. Model chooses which.
3. **`tool_choice: "auto"`** — model might return text. No guarantee.

### Schema Design Tips
- **Optional/nullable fields** — for data that may not exist. Prevents hallucination (model returns null).
- **Enum + "other" + detail field** — for extensible categories: `category: "return" | "exchange" | "other"` + `categoryDetail: string`
- **Strict schemas** eliminate syntax errors, NOT semantic errors (e.g., line items not summing to total)

### Practice Questions

**Q4.3:** Extraction tool requires `phone_number` field, but some documents don't contain phone numbers. Production shows Claude fabricates plausible phone numbers. Fix?

A) Add "don't make up phone numbers" to prompt
B) Make `phone_number` optional/nullable in the schema
C) Add validation that checks phone numbers against a database
D) Use `tool_choice: "auto"` so Claude can skip extraction

<details><summary>Answer</summary>
**B.** When a field is required, models fabricate values to satisfy the schema. Optional/nullable lets the model return null. A conflicts with a required field. C is downstream validation. D skips the entire extraction.
</details>

---

## 4.4 Validation-Retry Loops

### Pattern
```
Extract → Validate → If invalid: Send (original doc + failed extraction + specific error) → Re-extract
```

### When Retries Work vs Don't

| Retries WILL work | Retries WON'T work |
|---|---|
| Format mismatches (date formats) | Information absent from source document |
| Structural output errors | Data requiring external lookup |
| Missed fields that exist in the doc | Contradictory source data |

### Practice Questions

**Q4.4:** Extraction fails validation because `total_amount` doesn't match sum of `line_items`. Happens in 5% of documents. Best approach?

A) Retry with validation error appended, ask Claude to recalculate
B) Automatically recalculate the total from line items
C) Add `calculated_total` and `stated_total` fields to detect discrepancies, with a `conflict_detected` boolean
D) Increase temperature

<details><summary>Answer</summary>
**C.** Semantic validation issue (not a schema error). Smart approach captures both values and flags discrepancies, letting downstream logic reconcile. A treats symptom. B assumes line items are right. D is random.
</details>

---

## 4.5 Message Batches API

### Key Facts

| Feature | Detail |
|---|---|
| Cost savings | **50%** |
| Processing time | Up to **24 hours** |
| Latency guarantee | **None** |
| Multi-turn tool calling | **NOT supported** |
| Request correlation | `custom_id` field |

### When to Use
- ✅ Overnight reports, weekly audits, nightly test generation
- ❌ Blocking pre-merge checks, real-time user-facing requests

### Practice Questions

**Q4.5:** Your manager wants to move both pre-merge code review (blocking, developers wait) and overnight technical debt analysis to Batch API for 50% savings. Recommendation?

A) Move both — Batch API is usually faster than 24 hours
B) Move only overnight analysis; keep real-time for pre-merge
C) Keep both real-time — batch result ordering is unreliable
D) Move both with timeout fallback to real-time

<details><summary>Answer</summary>
**B.** Batch API has no latency SLA — unsuitable for blocking workflows. Overnight is a perfect fit. A relies on "usually." C is wrong about ordering (custom_id solves that). D is unnecessary complexity.
</details>

---

## 4.6 Multi-Pass Review

### Core Concept
Single-pass review of many files → attention dilution, missed bugs, contradictory findings.

**Better pattern:**
1. **Per-file local analysis** — review each file individually for local issues
2. **Cross-file integration pass** — examine data flow, API contracts, consistency

### Self-Review Limitation
The same model instance that generated code retains its reasoning context, making it less likely to question its own decisions. **Use a separate, independent Claude instance** for review.

### Practice Questions

**Q4.6:** A PR modifies 14 files. Single-pass review gives detailed feedback on some files, misses obvious bugs in others, flags a pattern as "problematic" in one file while approving identical code elsewhere. Fix?

A) Split into per-file analysis passes plus a separate cross-file integration pass
B) Require smaller PRs
C) Use a larger context model
D) Run three independent passes, flag only issues in 2+ runs

<details><summary>Answer</summary>
**A.** Focused passes address attention dilution directly. B shifts burden to developers. C doesn't solve attention quality. D suppresses intermittently-detected valid bugs.
</details>

---

## Domain 4 Recap

- Few-shot examples > vague prose for ambiguous cases. 2–4 examples.
- tool_use with JSON schemas = guaranteed schema compliance (not semantic).
- Optional/nullable fields prevent hallucination of missing data.
- Enum + "other" + detail string for extensible categories.
- Validation retries work for format errors, NOT for absent information.
- Batch API: 50% savings, up to 24hr, NO multi-turn tools, NOT for blocking workflows.
- Per-file + cross-file passes > single-pass for large reviews.
- Independent instance reviews > self-review.
