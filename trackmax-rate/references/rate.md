# Trackmax Rate: assessment protocol

This protocol is the standalone source of truth for the `trackmax-rate` skill. Read it in full before rating. It assumes a fresh context and must not rely on prior chat history, hidden project memory, or an earlier Trackmax recommendation.

## 1. Purpose and input contract

Assess whether a specific idea fits a specified hackathon track or prize **and whether the available evidence supports pursuing it now**. The deliverable is a transparent strategic-fit rating, qualitative outcome outlook, track-direction assessment, conditional best/base/worst scenarios, and a recommendation. It is not product ideation, a product review, or a promise of a prize.

Freeze one evidence packet before delegating:

- the idea exactly as supplied, with its behaviors and unresolved questions kept distinct;
- the hackathon, edition/date, candidate prize track(s), official rules and sponsor rubric;
- stated team skills, build time, existing code, and hard constraints; mark unstated facts `unknown`;
- the supplied winner corpus or completed analysis report, including its `TRACKMAX_RATE_MODEL` if present;
- research date and the corpus/source coverage already established.

If the user provides a completed Trackmax report, use it as input, not as a prior recommendation to inherit. If they provide a raw winner ledger instead, validate it and derive a historical-fit model only from its evidence. If no usable winner corpus is supplied, research it independently from the installed source skills and current web sources. Do not silently substitute a generic rubric. If the event/edition or idea is too ambiguous to identify the proper rules or corpus, ask only for the decision-critical missing detail.

Keep three populations separate: exact target-track/prize winners, other relevant winner analogues, and broader/entrant records. Deduplicate projects with multiple awards. Never treat missing from a winner corpus as a losing submission. Do not calculate an award rate without a complete, comparable entrant denominator.

## 2. Independent research

For each candidate prize track, create two read-only research tasks with the exact same frozen packet:

1. **Historical-fit analyst:** validates the model and winner records, tests the idea against observable recurring winner patterns, and reports `H` with predicate-by-predicate evidence.
2. **Trajectory and counterevidence analyst:** independently studies temporal movement, current sponsor/ecosystem signals, and evidence against momentum; reports `T`, trend state, official-rubric fit `S`, and uncertainty.

Do not show either analyst the other's output. Do not ask either to select an overall rating or see the other track's conclusion. The main analyst independently scores eligibility, demo memorability, feasibility, reconciles evidence, and writes the final assessment. If there are several candidate tracks, preserve the two roles for every track and run research in waves that respect connector concurrency/rate limits; do not collapse unrelated tracks into a consensus prompt.

For **every crypto/Web3 idea**, both analysts must independently load and query `$ethglobal-skills` and `$colosseum-copilot`, regardless of target chain. Keep their records as separate source lanes and use them for exact-event evidence only where appropriate; otherwise label projects as same- or cross-ecosystem analogues. Follow each installed skill's exact API/auth/rate rules. Do not reveal credentials, ask for tokens in chat, or initiate paid requests. If an adapter is inaccessible, record the actual block and continue with evidence that was retrieved.

Both analysts must also make at least **eight separate, focused calls to the actual `web_search` tool**, and fetch at least three independent first-party/official pages with `web_fetch` where they exist. Connector calls do not count as web searches. Include queries on official eligibility/rubric, recent and older winners, what relevant winners actually built, dated current priorities or technical changes, cross-ecosystem analogues, and a deliberate falsification/crowding check. Log exact queries, source URLs, dates, selected/excluded results, API calls and counts, and coverage gaps. Never say the agents or searches ran unless their tool results confirm it.

Use official event/sponsor pages for eligibility and prize wording, and first-party project sites, repositories, demos, and documentation for product behavior. Search result snippets only discover sources; they do not verify claims. Label material statements `Observed`, `Interpretation`, or `Unknown`; distinguish source date from retrieval date. Keep stable project/evidence IDs so the final rating can be audited.

### Historical-fit model validation

If a `TRACKMAX_RATE_MODEL` is present, validate its declared population has `N > 0`, and validate denominators, distinct project/source support, predicates, weights, formula, and missing-evidence rule before using it. The applicable denominator must reconcile to the declared population. Preserve the model's meaning as **historical winner-pattern similarity, not probability**. A non-gate predicate is not usable when its project support is missing, fewer than three distinct projects support it, or count/prevalence cannot be reconciled. A documented hard eligibility gate may be supported by official rules rather than three winners.

When a valid model exists, score only evidence the supplied idea explicitly establishes; vague claims and future aspirations are unmet. Do not infer implementation, user, integration, or outcome. Apply the exact model formula and rounding rule, and call the result `H` (historical fit).

When only a raw corpus is supplied, derive observable predicates from recurring evidence in that corpus using the Trackmax model rules: usually 4–8 predicates, no padding with generic quality criteria, prevalence-based weights by default, and explicit support by distinct projects and links. If the corpus is too small or weak to establish recurring predicates, mark `H` insufficient; do not manufacture a similarity score. A model marked `exploratory` remains exploratory in the final confidence.

## 3. Track trajectory: is it rising, fading, or changing?

Use dated project/winner cohorts—preferably the latest four years—and current primary-source evidence. Report corpus coverage by year and distinct projects before describing motion. Analyze:

- how often relevant mechanisms/problems appear among the target's winners in each comparable cohort (counts and winner-set denominators);
- whether patterns are new, accelerating, mature/crowded, mutating, fading, or re-emerging;
- whether recent official sponsor priorities, product releases, ecosystem actions, or new technical capabilities directly enable or reward the direction;
- counterexamples, crowding, abandoned/withdrawn priorities, rubric changes, and evidence that an apparent rise is only broader industry buzz.

Use one lifecycle label: `emerging`, `accelerating`, `mature`, `crowded`, `mutating`, `fading`, `re-emerging`, `stable/mixed`, or `insufficient-data`. Also state the shorter directional read: `rising`, `stable`, `declining`, `mixed`, or `unknown`. These labels describe observed evidence, not the likelihood of a prize. Winner-set frequency does not establish entrants' behavior, demand, judge motives, or causation.

Rate `T` (0–10) using these anchors. Do not use the low end to punish missing research; use `N/A` when the evidence is inadequate.

| T | Evidence anchor |
|---:|---|
| 0–2 | Relevant pattern is materially declining or directly contradicted by current primary evidence; no credible current enabling signal. |
| 3–4 | Weak/old support or fading pattern; current evidence is mostly absent or points away. |
| 5–6 | Stable or mixed pattern, or a real but incomplete shift with counterevidence. |
| 7–8 | Clear emerging/accelerating/mutating signal across distinct winners/cohorts plus dated current support. |
| 9–10 | Strong, repeated rise across multiple comparable cohorts, corroborated by current primary priorities/enablers, with the crowding and contrary evidence tested and disclosed. |

Do not give 9–10 from a single announcement or a burst of keyword mentions. If year coverage is sparse, no comparable cohort denominator exists, or findings are just general market narratives, choose `insufficient-data` for the lifecycle and `T = N/A`.

## 4. Rating rubric

Report these dimensions separately, each from 0 to 10, and show evidence for each. The fixed weights make ratings comparable as a transparent decision aid; the composite is **not statistical and not a win probability**.

| Dimension | Weight | What is scored |
|---|---:|---|
| `H` Historical fit | 30% | The validated Trackmax model's idea-to-winner-pattern score. |
| `T` Track trajectory | 20% | The dated cohort movement and current enablement, using the anchors above. |
| `S` Sponsor/criteria fit | 15% | Direct match to verified official judging criteria and required integrations, not keyword overlap. |
| `M` Memorable demo | 20% | Whether the stated idea produces a concrete, useful, surprising, repeatable live demonstration tied to the track. Judge reaction is not knowable. |
| `F` Feasibility | 15% | Whether the stated scope can be built and demonstrated within the supplied time/team/technical constraints. Unknown team/time is `N/A`, not an assumed failure or success. |

For each component, cite observed evidence and state what the idea description does not establish. Score the idea **as specified**, not the imagined best version. For `M`, a generic dashboard or unshown capability is weak evidence; do not infer a compelling demo. For `F`, score scope and stated constraints, not an invented team skill set. A documented official disqualifier makes the track ineligible even if the idea otherwise scores well.

Use these anchors for dimensions without an embedded formula; apply `N/A` rather than a low score when evidence is missing:

| Dimension | Score anchors |
|---|---|
| `S` Sponsor/criteria fit | `0–2`: no demonstrated relationship to the verified criteria, or material contradiction; `3–4`: tangential fit with no direct track-specific behavior; `5–6`: partial criterion match but material required elements are unclear; `7–8`: explicit track-specific behavior and required integration map to the rubric; `9–10`: verified hard requirements are met and the core product behavior directly delivers multiple rubric priorities. Do not double-count formal ineligibility; the eligibility gate controls that case. |
| `M` Memorable demo | `0–2`: no concrete user-visible result or only generic claims; `3–4`: demonstrable but familiar behavior with little immediate payoff; `5–6`: clear useful result with a relevant, understandable live demonstration; `7–8`: immediate, repeatable, surprising track-specific capability tied to a real user outcome; `9–10`: unusually memorable end-to-end moment that lands in seconds, makes the ecosystem capability tangible, and is both useful and reproducible rather than a gimmick. These scores assess the stated demo plan, not predicted judge reaction. |
| `F` Feasibility | `0–2`: core delivery is impossible under the stated constraints or a blocking dependency is unavailable; `3–4`: scope is oversized or depends on several unproven critical integrations; `5–6`: a reduced demo seems achievable but important integration/time risks remain; `7–8`: bounded scope, known tools, and a credible working demo fit the supplied team/time; `9–10`: the critical path is already proven or readily available and a complete, coherent demo is straightforward within the budget. If team/time is unknown, use `N/A`. |

For a track that is ineligible under verified rules, report `0.0/10 — ineligible` as its actionable rating and name the disqualifier. If eligibility is unresolved, label the rating conditional and state the exact fact to verify. Do not present it as unconditional.

Otherwise compute:

```text
Trackmax rating = sum(weight × dimension score for each scored dimension)
                  / sum(weights for scored dimensions)
```

Use the weights in the table as decimals and round once at the end to one decimal using conventional half-up rounding. `H`, `S`, and at least one of `T`, `M`, or `F` must be scoreable for a numerical composite; otherwise report `N/R — insufficient evidence` and request only the material missing corpus/input. Mark a composite `provisional` if any dimension is `N/A`, eligibility is unresolved, the historical model is exploratory, or the source coverage is materially incomplete. Show the unscored dimensions; never replace them with zero. Ratings with different missing dimensions are not directly comparable.

Interpret the composite as an assessment band, not an award forecast: `0–2` poor documented fit; `3–4` weak; `5–6` mixed/conditional; `7–8` promising; `9–10` unusually strong evidence alignment. A strong score still does not imply a likely win.

## 5. Outcome outlook and scenarios

Answer “will it win?” with the strongest supportable qualitative statement: `favorable but uncertain`, `plausible but competitive`, `weakly supported`, `unresolved`, or `not eligible`. Use `favorable` only when eligibility is verified, `H` and `S` are each at least 7, and at least one of `T`, `M`, or `F` is at least 7. Use `plausible` when a real historical or official-criteria fit remains but material risks or mixed timing persist; use `weakly supported` when both `H` and `S` are below 5 or decisive counterevidence dominates; use `unresolved` when key facts or comparable evidence are missing. These thresholds guide an evidence summary; they do not mechanically convert the composite score into an award forecast. Explain the decisive evidence and confidence. Do not convert the composite rating, winner prevalence, agent agreement, or number of searches into a probability.

Only report an event/track award base rate when the participant denominator is complete, comparable, and sourced. Even then, label it as the cohort's historical base rate—not this idea's individual chance. Winner-only records cannot identify what failed or calibrate an idea-level forecast. If those data do not exist, say so directly and still give the qualitative outlook when the evidence supports one.

Give these three conditional scenarios for each serious candidate track:

- **Best plausible case:** the concrete demo, verified rubric fit, timing signal, and delivery assumptions required for a strong award outcome; do not call it guaranteed.
- **Base case:** the outcome best supported by this evidence set. If it is an analyst judgment rather than a statistically estimated modal outcome, label it that way.
- **Worst plausible case:** the most decision-relevant failure path, such as rule mismatch, crowded pattern, weak demo, failed dependency, or no award; connect it to evidence and a mitigation if possible.

Separate scenario conditions from facts. Don't force a best/base/worst award rank when the corpus or competition evidence cannot support it; state the limits. Finish with a direct recommendation: pursue as written, sharpen the demo/track fit, switch track, resolve a gate first, or stop. Name the one evidence or implementation change most likely to alter that recommendation.

## 6. Final response format

For one candidate track, return a concise analyst brief with these sections and end with the rating line:

```text
Idea / event / track
Verdict — pursue, revise, switch, resolve gate, or stop; one-sentence reason.
Eligibility — eligible, ineligible, or unresolved; official-source evidence.
Track direction — lifecycle label; rising/stable/declining/mixed/unknown; timeframe and confidence.
Evidence scores — H, T, S, M, F, each with weight, score or N/A, and its decisive reason.
Outcome outlook — qualitative category and confidence; historical base rate only if a complete denominator exists.
Best plausible case — conditions and outcome.
Base case — conditions and evidence limits.
Worst plausible case — failure path and mitigation.
Why it could win — strongest evidence-backed pattern and demo.
Why it could lose — strongest counterevidence/risk.
Next move — one highest-value change or verification.
Coverage — corpus/event-years, distinct winner counts, searched sources/adapters, blocked lanes, important gaps.
Trackmax rating — X.X/10 [provisional/conditional if applicable], strategic-fit score, not probability.
```

For multiple tracks, give one compact comparison table with separate eligibility, `H/T/S/M/F`, direction, rating, and outlook per track, then give scenarios and a recommendation for the leading one(s). Do not combine track scores or pretend that scores are independent probabilities. Link claims to primary sources and distinguish target winners from analogues.

## 7. Independent task prompts

Copy one prompt per assigned role and track. Replace bracketed text and paste the same frozen packet into both roles. Each task must use the runtime's actual research tools and return a query/source log. Do not combine their findings before both finish.

### Historical-fit analyst prompt

```text
You are the independent historical-fit analyst for one hackathon prize track. Work only from the frozen packet below and evidence you actually retrieve. Do not ideate replacements, make an overall recommendation, estimate win odds, or use another analyst's output. This is read-only.

FROZEN PACKET
[exact idea, event/edition, target track and rules, team/time constraints, supplied corpus/report, research date]

Follow the Trackmax Rate protocol's source, model-validation, and logging requirements. If this is a crypto/Web3 idea, independently load/query both $ethglobal-skills and $colosseum-copilot in separate lanes, regardless of target chain; follow their auth/rate rules, label exact targets versus analogues, and never expose credentials or initiate payment. Use the actual web_search tool for at least eight separate focused searches and web_fetch primary/first-party sources. Record exact queries, source/API calls and counts, URLs/dates, corpus coverage, and blocks. Do not claim research that tool results do not confirm.

Validate supplied winner records, deduplicate multi-award projects, and test any TRACKMAX_RATE_MODEL population/counts/support before use. Evaluate each predicate only from explicit idea evidence and apply its formula exactly to return H. If only raw records exist, derive predicates only when repeated evidence supports them under the rate protocol; otherwise H=N/A. Keep target winners separate from same- and cross-ecosystem analogues. Winner-set prevalence is not a win rate.

The model score is historical similarity only: `H = 10 × sum(weights of satisfied predicates) / sum(all predicate weights)`, rounded as the model specifies. A non-gate predicate is unusable if fewer than three distinct projects support it or its count/prevalence cannot be reconciled; documented official hard gates are an exception.

Return: (1) model validation and population/coverage; (2) predicate table with ID, idea evidence satisfied/unmet, distinct supporting project IDs/links, denominator, weight, and score calculation; (3) H score or N/A and confidence; (4) strongest historical match and strongest mismatch; (5) direct evidence gaps; (6) source/query log. Label Observed/Interpretation/Unknown. Do not score current momentum, feasibility, showoff, or overall rating.
```

### Trajectory and counterevidence analyst prompt

```text
You are the independent track-trajectory and counterevidence analyst for one hackathon prize track. Work only from the frozen packet and evidence you actually retrieve. Do not ideate replacements, make an overall recommendation, estimate win odds, or use another analyst's output. This is read-only.

FROZEN PACKET
[exact idea, event/edition, target track and rules, team/time constraints, supplied corpus/report, research date]

Follow the Trackmax Rate protocol's source, trend, and logging requirements. If this is a crypto/Web3 idea, independently load/query both $ethglobal-skills and $colosseum-copilot in separate lanes, regardless of target chain; follow their auth/rate rules, label exact targets versus analogues, and never expose credentials or initiate payment. Use the actual web_search tool for at least eight separate focused searches and web_fetch primary/first-party sources. Include official criteria, recent and older winner cohorts, dated sponsor/ecosystem priorities or enablers, cross-ecosystem analogues, and deliberate falsification/crowding searches. Record exact queries, source/API calls and counts, URLs/dates, coverage, and blocks. Do not claim research that tool results do not confirm.

Use preferably four years of dated cohorts, reporting distinct project counts and denominators by year. Decide whether the exact direction is emerging, accelerating, mature, crowded, mutating, fading, re-emerging, stable/mixed, or insufficient-data; separately report rising/stable/declining/mixed/unknown. Do not call broad buzz winner momentum. Search for weakening, contradictory, stale, and crowding signals. Score T only when evidence is adequate, using the protocol anchors; otherwise T=N/A. Score S only against verified official prize wording and required integrations. Track sponsor signals separately from winner-set prevalence. Do not infer judge motives or a win probability.

Use these `T` anchors: `0–2` material decline or direct contradiction; `3–4` weak/old/fading support; `5–6` stable or mixed evidence; `7–8` clear emerging/accelerating/mutating evidence across distinct winners/cohorts plus dated current support; `9–10` repeated rise across multiple comparable cohorts, corroborated by primary current signals with crowding and counterevidence tested. Missing or sparse comparable cohorts means `T=N/A`, not a low score.

Return: (1) event/track rule evidence and S score; (2) year-by-year winner/trend table with counts, denominators, and source IDs; (3) lifecycle and directional labels with timeframe/confidence; (4) T score or N/A with anchor rationale; (5) dated current enablers and strongest counterevidence; (6) what would change the trend conclusion; (7) source/query log and coverage gaps. Label Observed/Interpretation/Unknown. Do not score historical fit, feasibility, showoff, or overall rating.
```
