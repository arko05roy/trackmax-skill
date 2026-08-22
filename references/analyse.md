# `trackmax/analyse` protocol

## Goal

Produce a forensic, project-level map of what has won the requested track/domain on the requested chain. The result measures the historical winner set; it does not produce generic advice or pretend that a winner-only sample estimates causal win probability.

## 1. Lock the research frame

Record these fields at the top of the report:

```yaml
datasource: exact datasource/skill/API used
target: exact sponsor, prize, or domain term
chain: requested chain
window_start: YYYY-MM-DD
window_end: YYYY-MM-DD
coverage_rule: at least the prior 365 days; extend backward only when the target has fewer than 10 eligible winner records, and state the extension
```

Use at least one year ending on the research date. Enumerate all in-window events in the datasource, including online and in-person events where the datasource covers them.

## 2. Use a reproducible inclusion ladder

Assign every project one tier. Do not merge tiers before reporting counts.

| Tier | Include when | Purpose |
|---|---|---|
| A | The project won the exact target sponsor/track prize | Primary winner set |
| B | The project won a prize whose official title/qualification directly names the target domain, on the requested chain | Domain-adjacent winner set |
| C | The project was an overall winner/finalist and has explicit target-domain and requested-chain evidence in its project record | Context set only; never mix with Tier A in primary percentages |
| D | The project merely mentions the target | Mention set only; do not call it a winner or use it in winner percentages |

For an exact sponsor track, query sponsor winners first. For a domain, use keyword retrieval plus prize retrieval, then manually validate the project description and award. Capture both positive and negative filtering decisions when they affect a count.

## 3. Chain filtering

Classify chain relationship from explicit project evidence:

| Label | Meaning |
|---|---|
| primary | The target chain is the project's execution/deployment chain or central user workflow |
| material | The target chain is a meaningful integrated component but not central |
| incidental | The target chain is mentioned but not functionally important |
| unknown | The source does not establish chain usage |

Primary analysis includes `primary` and `material` records. List `incidental` and `unknown` records separately. If fewer than five primary/material winner records exist, do not manufacture percentages; state the scarcity.

## 4. Build the raw project ledger

Include every eligible project in a table with:

- project name and source hyperlink;
- event and event type (online/IRL if available);
- date/year;
- award text verbatim;
- tier;
- chain relationship label and direct evidence;
- project tagline;
- concise factual description;
- source reliability/coverage note.

Then add a per-project section for every Tier A/B record. Each section must state, using source-backed language:

1. The user/problem context.
2. The core mechanism.
3. The track/domain integration.
4. The functional outcome.
5. The primary project archetype.
6. Secondary tags.

Do not blur a project’s technical implementation into an inferred motive.

## 5. Tagging and trend extraction

Create a project-level codebook before counting. Use concise, observable tags such as `wallet-security`, `fraud-detection`, `agentic-trading`, `liquidity-optimization`, `consumer-social`, `developer-tooling`, `knowledge-graph`, `physical-world-settlement`, or a target-specific equivalent.

For every tag, provide:

- operational definition;
- included project names;
- count;
- denominator;
- percentage;
- whether tags overlap.

Also produce these non-overlapping classifications so percentages can sum to 100%:

- primary problem family;
- primary mechanism;
- primary user/outcome;
- award rank (first/second/third/unranked).

Use an `Other/unclear` bucket instead of forcing a classification.

Required analysis slices:

1. **Problem trend:** What recurring problem is solved?
2. **Mechanism trend:** What recurring mechanism produces the result?
3. **Outcome trend:** What does the project cause, decide, secure, optimize, or enable?
4. **Track-usage trend:** How is the named track/domain technically used?
5. **Intersection trend:** Which problem + mechanism pairs recur?
6. **Recency trend:** Compare the most recent half of the window with the earlier half. Show raw counts and do not claim a trend when either cohort has fewer than three records.
7. **Prize-rank trend:** Compare first-place records with all other ranked winners separately.

For every reported pattern, distinguish:

- `Observed`: direct count from the ledger.
- `Interpretation`: a bounded description of the repeated project shape.
- `Not established`: claims the data cannot support.

## 6. Build a data-derived rating model

End the report with this exact block. It is the only permitted rubric for `trackmax/rate`.

```markdown
## TRACKMAX_RATE_MODEL

model_version: 1
population: Tier A/B primary+material records only (N=<integer>)
score_definition: historical winner-set similarity, not probability of winning
missing_evidence_rule: unmet

| id | observable predicate | evidence required in idea document | winners with predicate | denominator | prevalence_pct | weight |
|---|---|---|---:|---:|---:|---:|
| P1 | ... | ... | ... | ... | ... | ... |

formula: score = 10 * sum(weight for satisfied predicates) / sum(all weights)
rounding: one decimal, conventional half-up
```

Rules for predicates:

- Use 4–8 predicates only.
- Each predicate must be observable from an idea document without guessing.
- Derive predicates from the most recurrent, discriminating project-level patterns, not generic quality criteria.
- A predicate can describe an intersection, such as `wallet-security + risk action`, only if it recurs in the ledger.
- Set `weight` equal to `prevalence_pct` by default. Alter it only to avoid double-counting logically nested predicates, and explicitly show the calculation.
- Do not include prize qualifications unless the final winner records demonstrate the attribute or the target’s official rules make it a hard eligibility gate. Hard gates get a binary predicate with documented source.
- Do not use subjective fields such as "great", "innovative", "polished", or "strong team".

## Required report shape

1. Research frame and coverage
2. Datasource method and limitations
3. Prize/track definitions, quoted fully where the datasource requires it
4. Full project ledger
5. Per-project analysis
6. Non-overlapping distributions
7. Overlapping trend matrix
8. Intersection and recency analysis
9. Observed patterns / bounded interpretations / not established
10. `TRACKMAX_RATE_MODEL`
