# `trackmax/analyse` protocol

## Goal

Produce a detailed, plain-English developer cheat sheet of what has won the requested track/domain on the requested chain. It should name the actual products, explain what each made, and show the winner patterns, micro-trends, mutations, and timing states behind those patterns. The result measures the historical winner set; it does not produce generic advice or pretend that winner-only data proves why projects win.

Write like you are briefing a developer who wants to understand the track quickly. Prefer familiar words, short sentences, bullets, and concrete examples. Define unavoidable research terms in one plain sentence. Do not use business-school language, inflated prose, or unexplained jargon.

## 1. Lock the research frame

Record these fields at the top of the report:

```yaml
datasource: exact datasource/skill/API used
target: exact sponsor, prize, or domain term
chain: requested chain
window_start: YYYY-MM-DD
window_end: YYYY-MM-DD
coverage_rule: target the prior 4 years; use every available eligible record in that period, and use at least the prior 2 years when the datasource has that coverage. State any unavailable years or coverage gaps.
research_date: YYYY-MM-DD
```

Research the last four years ending on the research date. If the datasource only covers part of that period, use every available year but do not shrink a covered window below two years. Only use a shorter window when fewer than two years exist, and say why in plain English. Enumerate all in-window events in the datasource, including online and in-person events where the datasource covers them. Record a retrieval log with query/filter, source endpoint or page, retrieval date, raw result count, and disposition. Then publish a coverage-reconciliation table: events expected, events searched, award pages found, project records found, qualifying award records, unique eligible projects, and exclusions by reason. Missing, inaccessible, or unsearchable data is a coverage gap, never a zero.

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

Then add a per-project section for every Tier A/B record. Start with `What they made` in simple English, then state, using source-backed language:

1. The user/problem context.
2. The core mechanism.
3. The track/domain integration.
4. The functional outcome.
5. The primary project archetype.
6. Secondary tags.

Do not blur a project’s technical implementation into an inferred motive.

## 5. Tagging and trend extraction

Create and publish a project-level tag guide before counting. A tag guide is a short list of labels used the same way for every project. Every tag needs a plain definition, minimum source evidence, and examples/non-examples from the ledger. Use concise, observable tags such as `wallet-security`, `fraud-detection`, `agentic-trading`, `liquidity-optimization`, `consumer-social`, `developer-tooling`, `knowledge-graph`, `physical-world-settlement`, or a target-specific equivalent. Apply the locked tag guide to all eligible unique projects. If a tag changes, log the change and retag the full population; never add a category only after seeing its frequency.

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
6. **How it changed:** Compare each available year, then compare the newest two years with the older years. Name the projects behind each change, show raw counts, and do not claim a trend when either comparison group has fewer than three records.
7. **Prize-rank trend:** Compare first-place records with all other ranked winners separately.
8. **Concentration test:** For every candidate trend, report distinct projects/N, percentage, and status. `signature` requires at least 3 distinct projects and at least 30% of the primary population. If N < 10, label all statuses exploratory even when this rule is met. All other observations are `signals`, not signatures.

For every reported pattern, distinguish:

- `Observed`: direct count from the ledger.
- `Interpretation`: a bounded description of the repeated project shape.
- `Not established`: claims the data cannot support.

If the datasource exposes a complete, comparable entrant or non-winner population under the same coverage rule, add a separate contrast table with winner prevalence, comparator prevalence, percentage-point difference, and prevalence ratio. Name the comparator population and its denominator. Never construct a comparator from search results; absent such data, do not use the terms lift, odds, driver, or probability.

### 5A. Micro-trend decomposition

Broad tags such as `DeFi`, `AI`, or `consumer` are not sufficient. Decompose every eligible project into small, observable fields before looking for patterns. Use the same fields for every project:

- problem pressure: the concrete pain, risk, or unmet need;
- user segment: who uses it and in what context;
- workflow: the action or sequence being changed;
- mechanism: how the product produces its result;
- technical primitive: protocol, API, model, wallet, data source, or chain feature;
- track integration: exactly where the named chain, sponsor, or domain is functional;
- outcome: what the product causes, decides, secures, optimizes, settles, or enables;
- automation level: manual, assisted, rule-based, agentic, or autonomous;
- trust model: custodial, non-custodial, social, cryptographic, reputation-based, or hybrid;
- distribution surface: wallet, protocol, browser, mobile app, API, developer tool, physical-world interface, or social platform;
- integration dependency: partner, standard, infrastructure launch, grant, or ecosystem program;
- narrative language: recurring words used by the project, sponsor, or award description;
- monetization or incentive mechanism, when explicitly documented.

Publish a micro-trend ledger with one row per project-field observation. Each row must include the project ID, field, normalized label, verbatim evidence, source URL, evidence grade, and first observed event/year. Do not infer a field from a product name alone. Use `unknown` when the source does not establish it.

### 5B. Trend lifecycle and state

For each recurring label and important intersection, calculate its state by cohort and event date. Use these states:

| State | Operational meaning |
|---|---|
| `emerging` | First meaningful appearances; not yet persistent across cohorts |
| `accelerating` | Occurrence, rank, or technical depth is increasing across comparable cohorts |
| `mature` | Repeated across multiple cohorts with stable meaning |
| `crowded` | Frequent, but increasingly common and less distinctive among winners |
| `mutating` | An older pattern is being combined with a new primitive, user, workflow, or outcome |
| `fading` | Less frequent or less central in newer cohorts |
| `re-emerging` | A previous pattern returns in a materially changed form after a quiet period |
| `insufficient-data` | Too few comparable records to assign a lifecycle state |

Do not assign a state from frequency alone. Record the evidence for the state: earliest appearance, latest appearance, cohort counts, rank distribution, named projects, and the rule used. A high-frequency old category is not automatically an opportunity; test it for crowding and mutation.

### 5C. Cohorts, transitions, and trend velocity

Use all available years individually, then compare three cohorts when the data permits: `older`, `transition`, and `recent`. The default split is older years, the middle year(s), and the newest two years. State exact dates and record counts. If either comparison group has fewer than three primary/material projects, describe the result as a case study rather than a trend.

For every label or intersection with enough data, report:

- first appearance and first repeated appearance;
- latest appearance;
- count and share in every available year;
- change in count and percentage points between cohorts;
- time since first appearance;
- number of consecutive cohorts present;
- award-rank distribution over time;
- number of distinct projects and duplicate/multi-award handling;
- whether the label is broadening, narrowing, or changing meaning;
- named projects behind every material increase or decrease.

Use `trend velocity` only as a descriptive index, never as a probability:

```text
velocity = (recent cohort share - older cohort share) / number of cohort intervals
```

Publish the raw inputs alongside the index. If denominators differ, show both counts and shares. Do not rank trends by velocity when the underlying cohort is too small.

### 5D. Trend mutations and intersections

Search for transitions from an earlier project shape to a newer one. For every material mutation, publish:

| Earlier pattern | Enabling or pressure change | New pattern | First evidence | Current state | Projects |
|---|---|---|---|---|---|

Treat a mutation as supported only when the older and newer forms are separately evidenced in project records. Useful mutation dimensions include manual to automated, dashboard to action, protocol-only to consumer-facing, single-chain to cross-chain, human-operated to agent-assisted, and generic infrastructure to domain-specific workflow. These are examples, not assumed categories.

Count intersections such as `problem + mechanism`, `user + workflow`, `primitive + outcome`, and `track integration + product surface`. Report the full combination, its component counts, distinct project IDs, first/latest appearance, and whether it is a new combination or an established combination with a new primitive.

### 5E. Right-place/right-time assessment

Timing is an evidence-backed interpretation layered on top of the winner ledger. It is not a claim that timing caused a win. For each important project and trend, record the following dated signals when the datasource or approved supplementary sources support them:

- **problem pressure:** evidence that the need became more visible or urgent;
- **technical enablement:** a newly available primitive, API, model, standard, or chain capability;
- **ecosystem readiness:** liquidity, users, integrations, grants, distribution, or infrastructure;
- **sponsor alignment:** the prize wording or sponsor activity explicitly matching the pattern;
- **novelty:** whether the project was early, following an established pattern, or entering a crowded category;
- **competition density:** number of comparable included winners, and comparator data only when a complete comparator population exists;
- **narrative timing:** whether the project’s language matches a newly recurring ecosystem narrative;
- **award timing:** whether the pattern appears first in lower-ranked awards, then higher-ranked awards, or only in a particular event format.

For every timing assessment, publish `Observed`, `Interpretation`, and `Not established`. Use one of these timing labels:

| Timing label | Meaning |
|---|---|
| `early-mover` | Appears before the pattern becomes common in the observed winner set |
| `rapidly-adopted` | Appears soon after an enabling or pressure signal, then repeats |
| `well-timed-mature` | Uses a mature pattern with evidence of strong contemporary alignment, without claiming causality |
| `sponsor-created` | The pattern appears mainly after explicit sponsor/prize framing |
| `crowded-entry` | The pattern is already frequent when the project appears |
| `declining` | The pattern is less frequent or less central in recent cohorts |
| `re-emergent` | An older pattern returns with a materially new form |
| `unknown` | Evidence is insufficient to classify timing |

Include a `timing confidence` field (`high`, `medium`, `exploratory`) based on source quality and completeness. Winner-only evidence cannot establish market causation, user demand, or that a project won because it was early.

### 5F. Temporal trend map

Add a final trend map that separates historical repetition from current momentum:

| Trend/mutation | Historical frequency | Recent momentum | Enablement | Sponsor alignment | Crowding | Lifecycle state | Timing confidence |
|---|---:|---:|---|---|---|---|---|

Use raw counts and denominators beside every percentage. This map must name the projects supporting each row. It should make clear whether a pattern is common-but-crowded, rare-but-accelerating, mature-and-stable, or newly mutating.

## 6. Build a data-derived historical-fit model

End the report with this exact block. It supplies the reproducible historical-fit component used by the standalone `trackmax-rate` assessment; it is not, by itself, an overall rating or a forecast. Trackmax Rate separately validates eligibility, current track direction, official sponsor alignment, demo memorability, and feasibility.

```markdown
## TRACKMAX_RATE_MODEL

model_version: 2
population: Tier A/B primary+material records only (N=<integer>)
score_definition: historical winner-set similarity only; not an overall rating or probability of winning
missing_evidence_rule: unmet

| id | observable predicate | evidence required in idea document | winners with predicate | denominator | prevalence_pct | weight |
|---|---|---|---:|---:|---:|---:|
| P1 | ... | ... | ... | ... | ... | ... |

formula: historical_fit = 10 * sum(weight for satisfied predicates) / sum(all weights)
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

## 7. Optional supplementary timing sources

Winner records are the primary evidence. To study “right time” responsibly, supplementary sources may be used for dated context: official protocol releases, sponsor announcements, hackathon rule pages, ecosystem grants, first-party product launches, developer documentation, incident reports, and comparable dated ecosystem records. Keep these sources in a separate context ledger. They may explain enablement, pressure, or sponsor alignment, but they must not silently expand the winner population or create unsupported winner percentages.

For each supplementary signal record: date, event, source URL, source type, exact claim supported, related project/trend IDs, and whether the signal is direct or interpretive. If supplementary coverage is incomplete, say so.

## Required report shape

1. What this cheat sheet covers (research frame, time window, retrieval log, and coverage reconciliation)
2. Data gaps and limits (plain English)
3. Track/prize definition
4. Every winning product (full award/project ledger and exclusion log)
5. Product breakdowns: what each team made, how it worked, and why it fit the track
6. Micro-trend ledger and tag guide
7. Repeated patterns: problems, mechanisms, user outcomes, and technical track use
8. Cohort evolution, trend velocity, and lifecycle states
9. Trend mutations and recurring intersections
10. Right-place/right-time assessments and supplementary context ledger
11. Temporal trend map: historical frequency versus recent momentum
12. Strong repeated patterns, weaker signals, and what the data cannot prove
13. `TRACKMAX_RATE_MODEL`
