---
name: trackmax-rate
description: Score a hackathon idea against the evidence model in a Trackmax cheat sheet. Use only when an existing Trackmax analysis report and an idea are supplied; not for generic idea reviews.
---

# Trackmax Rate

Score an idea only against the `TRACKMAX_RATE_MODEL` in a completed Trackmax analysis report. This is a historical winner-pattern similarity score, not a prediction, product review, or general hackathon score.

## Inputs

You need both:

1. A valid `[name]-analysis.md` Trackmax cheat sheet with a complete `TRACKMAX_RATE_MODEL`, a nonzero population, and project/source support for every predicate.
2. An idea document, or a clear description of the proposed project.

If either input is missing, malformed, or lacks a usable model, ask only for the missing or corrected input. Never invent a fallback rubric.

## Score the idea

1. Read the model's population, predicates, required evidence, weights, formula, missing-evidence rule, and project/source support.
2. Check the model before scoring. It is invalid if a non-gate predicate lacks supporting project IDs and source links, has fewer than three supporting projects, or its count and prevalence do not reconcile to the population. Ask only for a corrected report.
3. Mark a predicate met only when the idea explicitly states the exact evidence the model requires. Vague claims, future aspirations, and generic technology mentions are not enough.
4. Do not infer implementation details, users, chain use, track integration, or outcomes that are not stated in the idea.
5. Apply the report's formula exactly. An unmet hard gate contributes zero; do not add another penalty.
6. Round only at the final step using the report's rounding rule.

## Output

When the inputs are valid, respond with exactly one line and nothing else:

```text
<one-decimal-number>/10
```

Examples: `0.0/10`, `6.4/10`, `10.0/10`.
