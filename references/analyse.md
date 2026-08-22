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
research_date: YYYY-MM-DD
```

Use at least one year ending on the research date. Enumerate all in-window events in the datasource, including online and in-person events where the datasource covers them. Record a retrieval log with query/filter, source endpoint or page, retrieval date, raw result count, and disposition. Then publish a coverage-reconciliation table: events expected, events searched, award pages found, project records found, qualifying award records, unique eligible projects, and exclusions by reason. Missing, inaccessible, or unsearchable data is a coverage gap, never a zero.

## 2. Use a reproducible inclusion ladder

Assign every project one tier. Do not merge tiers before reporting counts.

| Tier | Include when | Purpose |
|---|---|---|
| A | The project won the exact target sponsor/track prize | Primary winner set |
| B | The project won a prize whose official title/qualification directly names the target domain, on the requested chain | Domain-adjacent winner set |
| C | The project was an overall winner/finalist and has explicit target-domain and requested-chain evidence in its project record | Context set only; never mix with Tier A in primary percentages |
| D | The project merely mentions the target | Mention set only; do not call it a winner or use it in winner percentages |

For an exact sponsor track, query sponsor winners first. For a domain, use keyword retrieval plus prize retrieval, then manually validate the project description and award. Capture every inclusion and material exclusion with a one-sentence decision, evidence URL, and retrieval date. Normalize each project to a stable `project_id` (canonical project URL, then repository URL, then normalized name + event); retain all award records, but calculate project-level prevalence on unique `project_id`s. Flag multi-award projects explicitly so repeated awards cannot inflate a trend.

## 3. Chain filtering

Classify chain relationship from explicit project evidence:

| Label | Meaning |
|---|---|
| primary | The target chain is the project's execution/deployment chain or central user workflow |
| material | The target chain is a meaningful integrated component but not central |
| incidental | The target chain is mentioned but not functionally important |
| unknown | The source does not establish chain usage |

Primary analysis includes `primary` and `material` records. List `incidental` and `unknown` records separately. If fewer than five primary/material winner records exist, do not manufacture percentages; state the scarcity.

Attach an evidence grade to every chain and track-integration classification: `E1` = official award/project page states it; `E2` = first-party repository/demo corroborates it; `E3` = reputable secondary record only; `E0` = unknown. Only E1/E2 may establish `primary` or `material`; report E3 as unverified and exclude it from the primary population.

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
- stable project_id and multi-award/duplicate flag;
- source URL(s), retrieval date, and evidence grade;
- explicit inclusion/exclusion decision and reason.

Then add a per-project section for every Tier A/B record. Each section must state, using source-backed language:

1. The user/problem context.
2. The core mechanism.
3. The track/domain integration.
4. The functional outcome.
5. The primary project archetype.
6. Secondary tags.

Do not blur a project’s technical implementation into an inferred motive.

## 5. Tagging and trend extraction

Create and publish a project-level codebook before counting. Every code must have an operational definition, minimum source evidence, and examples/non-examples from the ledger. Use concise, observable tags such as `wallet-security`, `fraud-detection`, `agentic-trading`, `liquidity-optimization`, `consumer-social`, `developer-tooling`, `knowledge-graph`, `physical-world-settlement`, or a target-specific equivalent. Apply the locked codebook to all eligible unique projects. If a code changes, log the change and recode the entire population; never add a category only after seeing its frequency.

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
8. **Concentration test:** For every candidate trend, report distinct projects/N, percentage, and status. `signature` requires at least 3 distinct projects and at least 30% of the primary population. If N < 10, label all statuses exploratory even when this rule is met. All other observations are `signals`, not signatures.

For every reported pattern, distinguish:

- `Observed`: direct count from the ledger.
- `Interpretation`: a bounded description of the repeated project shape.
- `Not established`: claims the data cannot support.

If the datasource exposes a complete, comparable entrant or non-winner population under the same coverage rule, add a separate contrast table with winner prevalence, comparator prevalence, percentage-point difference, and prevalence ratio. Name the comparator population and its denominator. Never construct a comparator from search results; absent such data, do not use the terms lift, odds, driver, or probability.

## 6. Build a data-derived rating model

End the report with this exact block. It is the only permitted rubric for `trackmax/rate`.

```markdown
## TRACKMAX_RATE_MODEL

model_version: 2
population: Tier A/B primary+material records only (N=<integer>)
score_definition: historical winner-set similarity, not probability of winning
missing_evidence_rule: unmet

| id | observable predicate | evidence required in idea document | winners with predicate | denominator | prevalence_pct | weight |
|---|---|---|---:|---:|---:|---:|
| P1 | ... | ... | ... | ... | ... | ... |

formula: score = 10 * sum(weight for satisfied predicates) / sum(all weights)
rounding: one decimal, conventional half-up
predicate_support: distinct project IDs and source links supporting each predicate
```

Rules for predicates:

- Use 4–8 predicates only. If fewer than four defensible signatures exist, use fewer and label the model `exploratory`; never pad it with generic criteria.
- Each predicate must be observable from an idea document without guessing.
- Derive predicates only from signatures that pass the concentration test (or documented hard eligibility gates), not generic quality criteria or weak signals.
- A predicate can describe an intersection, such as `wallet-security + risk action`, only if it recurs in the ledger.
- Set `weight` equal to `prevalence_pct` by default. Alter it only to avoid double-counting logically nested predicates, and explicitly show the calculation.
- Do not include prize qualifications unless the final winner records demonstrate the attribute or the target’s official rules make it a hard eligibility gate. Hard gates get a binary predicate with documented source.
- Do not use subjective fields such as "great", "innovative", "polished", or "strong team".

## Required report shape

1. Research frame, retrieval log, and coverage reconciliation
2. Datasource method and limitations
3. Prize/track definitions, quoted fully where the datasource requires it
4. Full award/project ledger and exclusion log
5. Per-project analysis
6. Codebook and non-overlapping distributions
7. Overlapping trend matrix and concentration results
8. Intersection, recency, prize-rank, and valid comparator analysis
9. Signatures / signals / bounded interpretations / not established
10. `TRACKMAX_RATE_MODEL`
