# `trackmax/rate` protocol

## Goal

Return a strict historical-similarity score for an idea, using only the model embedded in a TrackMax analysis report. The score is not a product review, a generic hackathon score, or a probability claim.

## Inputs

1. A valid `[name]-analysis.md` containing a complete `TRACKMAX_RATE_MODEL` block.
2. An idea document that describes the proposed project.

If either input is missing, malformed, or lacks a model, ask only for the missing input. Do not create a fallback rubric.

## Scoring procedure

1. Parse the model's population, predicates, evidence requirements, weights, formula, and missing-evidence rule.
2. For each predicate, mark it satisfied only when the idea document explicitly establishes the required evidence.
3. A vague claim, a future aspiration, or a generic technology mention is not evidence. It is unmet.
4. Do not infer implementation details, user segment, chain usage, track integration, or functional outcomes that the idea document does not state.
5. Apply the formula exactly. A hard eligibility predicate that is unmet remains zero-weight contribution; do not invent an additional penalty.
6. Round only at the final step according to the report.

## Output contract

When valid inputs are supplied, answer with exactly one line and nothing else:

```text
<one-decimal-number>/10
```

Examples: `0.0/10`, `6.4/10`, `10.0/10`.

Do not explain the score, offer advice, soften the verdict, list failed predicates, or add Markdown.
