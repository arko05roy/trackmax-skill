# Trackmax strategy protocol

## Objective and boundaries

Optimize for at least one eligible prize, including a small cash award such as $10, a lower-ranked placement, or a non-cash prize. Treat a multi-prize result as additional upside, not as a reason to sacrifice the best credible route to one prize. “Winning” is the outcome; historical similarity, sponsor fit, and a striking demo are signals, not a calibrated forecast.

The analyst does not pretend to produce unprecedented ideas from nothing. It should identify evidence-backed opportunity spaces, pressure-test directions, and give the builder several precise keywords and combinations to explore. Leave room for the builder's own taste, product intuition, and creative leaps.

## Intake and research frame

Extract what the user has already provided. Ask only for information that can change eligibility, feasibility, or the recommendation:

- event name, dates, location/format, and official event page;
- target ecosystem/chain and the ecosystems or technologies the team can realistically build with;
- every track/prize and the exact award/rank/value, including sponsor, qualification, and submission rules;
- team skills and size, remaining time, existing prototype, preferred technologies, and hard constraints;
- whether the goal is any award, a particular prize floor, or a broader multi-prize portfolio.

If details are unavailable, proceed with explicit assumptions. Do not invent a prize value, event rule, team capability, available build time, judge preference, or entrant count. Record research date, sources, years covered, and gaps. Prefer official event and sponsor sources, then first-party project pages/repositories/demos, then reputable reporting. Every agent must use the actual web-search tool with the query volume and source checks in [the research-source adapters](research-sources.md). For every crypto/Web3 task, activate and query **both** `ethglobal-skills` and `colosseum-copilot` as independent evidence lanes, regardless of the target event or chain; follow each skill's endpoint, naming, and authentication instructions. These source corpora provide different analogues and must not gate or replace one another. They also do not replace independent web search or primary-source verification. A target-chain mismatch is not a reason to skip either source; skip only when the task is clearly non-Web3 or the adapter is unavailable, and record the reason.

For historical track or domain evidence, follow [the analysis protocol](analyse.md) when its full ledger and temporal analysis are useful. Keep the event-specific comparison narrower when that is enough to make a decision; never pad a strategy answer with irrelevant analysis.

## Delegated research

Delegate genuinely independent research, not the final decision. Follow the subagent protocol loaded with this strategy and the research-source adapters. Give every analyst the same canonical event packet, evidence rules, and return schema so the main analyst can compare like with like. Research tasks must not edit the user's repository or claim evidence they did not retrieve.

### Event with tracks or prizes

Create one independent research task per independently judged track or prize family. Keep placements with the same rubric together; split sponsor prizes that have materially different criteria. Each analyst investigates only its assigned scope, including:

1. Official eligibility, judging rubric, sponsor wording, award/rank/value, and submission requirements.
2. Past winners of this exact prize and closely related prizes in the same ecosystem; identify what each product actually did, not just its tagline.
3. Repeated winner patterns, meaningful ecosystem gaps, current sponsor/ecosystem priorities, and dated technical or distribution changes.
4. Any available entrant/non-winner data and what it can validly establish.
5. Several project-direction hypotheses and specific keywords or technical primitives, each tied to evidence.
6. A memorable, showable demo moment that is genuinely relevant to the prize; include implementation/eligibility risks and source gaps.

Return source-linked findings, not a winner prediction. Start independent tasks in parallel only up to both platform task capacity and shared source-adapter limits. If either limit is reached, run deterministic waves without merging or dropping scopes. Quality-check every result against the return schema, then issue targeted follow-ups only for missing or contradictory evidence. Do not ask an analyst to rewrite an otherwise usable report. If subagent tools are unavailable, perform distinct track-by-track passes and say they were not independently delegated. Never claim agents ran unless they did.

### Event without tracks

Use two independent research tasks with the prompt templates in the subagent protocol:

- **Ecosystem analyst:** Deeply examine what won, relevant runner-up or finalist products, official ecosystem priorities, grants/roadmaps/releases, judging criteria, and credible evidence about what struggled. Separate observed non-winner/failure evidence from mere absence in winner lists.
- **Keyword analyst:** Find the most useful current keywords and phrases by triangulating winner descriptions, sponsor/ecosystem language, official technical documentation, launches, grants, and ecosystem discussions. Return several keyword clusters (problem/workflow, user, technical primitive, outcome/narrative), representative source links, counts/time coverage when measurable, and warnings against overreading buzzword frequency.

Start both tasks together. Then the main analyst checks important claims against primary sources, develops evidence-backed directions, and selects the strongest feasible route. If the research tasks disagree, show the disagreement and resolve it with source quality, coverage, and relevance rather than averaging opinions. For a decision-changing evidence gap, create one narrow verification task with the disputed claim and required proof; do not rerun both broad investigations.

## Main analyst: compare, select, and challenge

For every plausible path, assess the same dimensions. Write “unknown” rather than filling a gap with intuition:

- **Eligibility and fit:** Does the idea satisfy the official rules and rubric in substance?
- **Historical evidence:** How many distinct, comparable winning projects support this direction, across what years and awards? Is a historical entrant denominator available?
- **Cross-ecosystem transfer:** What comparable workflow, feature, technical primitive, demo pattern, or vocabulary exists in the other ecosystem? Is it portable to the target ecosystem, and what target-specific evidence validates that it is missing or useful there?
- **Current alignment and gap:** What dated evidence supports sponsor interest or an ecosystem opening? Is the gap directly observed, or only an inference from products not found?
- **Competition/crowding:** How common is the pattern among comparable winners or entrants? A crowded category can still win; rarity is not automatically an advantage.
- **Feasibility:** Can this team build and prove the required behavior in the time available, given its stated skills and dependencies?
- **Memorability/showoff value:** What exact 30–90 second demo makes the ecosystem's capability feel unexpectedly powerful? What is the real user benefit, and what would make the moment credible rather than a gimmick?
- **Prize portfolio:** Could one coherent product materially qualify for another prize? Verify each qualification independently and count only prizes whose actual criteria it meets.

Prefer a path that gives a credible chance at at least one prize and a clear demo over a technically ambitious path that depends on unproven scope. Do not automatically pick the highest cash award or the most fashionable trend. Explain the tradeoff.

## Statistics and confidence

- Count **distinct projects**, not award rows. Flag one project winning multiple prizes and avoid duplicate inflation.
- When complete, comparable entrant data exists, show winning projects/eligible entrants and the precise denominator for each track/cohort. Give counts with percentages; if sample sizes are small, label the rate unstable and avoid false precision.
- Without a valid entrant denominator, report winner-set prevalence and counts only (for example, “4 of 18 distinct recorded winners”), never a win rate or probability.
- For every percentage, show numerator, denominator, time window, inclusion rule, and whether categories overlap. Treat missing records as a coverage gap, not zero.
- If a comparative score or weighting helps choose, publish its dimensions, scale, weights, evidence, and uncertainty. Label it a **decision score**, not a chance of winning. Prefer a transparent ranked comparison when weights would be arbitrary.
- Do not calculate a numeric probability of “at least one prize” by combining track scores or assuming independent outcomes. Tracks and judges are correlated, data is sparse, and scores are not calibrated probabilities.
- Keep observation and interpretation separate: `Observed` (source/count), `Interpretation` (bounded reading), `Unknown/Not established` (what the evidence cannot tell us).

## Required strategy output

Give the decision first, then the evidence. For a hackathon with tracks, include every supplied track in the comparison, including rejected ones. For an event without tracks, compare several distinct evidence-backed directions. Use this order:

1. **Recommendation:** Primary path and the objective it optimizes (at least one prize, desired prize floor, or multiple credible prize paths).
2. **Why this leads:** Eligibility, historical evidence, current alignment/gap, feasibility, and strongest counterargument, with source links and coverage caveats.
3. **The demo hook:** Exact user action, surprising result, ecosystem capability shown, and why it is useful. State what still needs validation.
4. **Keyword bank:** Several specific keywords/phrases grouped by problem, workflow/user, technical primitive, and sponsor/ecosystem language. Tie them to sources and explain that they are search/design leads, not magic judge terms.
5. **Cross-ecosystem signals:** Show what the ETHGlobal and Colosseum corpora independently surfaced, what is transferable from the other ecosystem, and what requires adaptation. Keep source populations separate. Call a capability absent locally only after checking target-specific products and sources—not because another corpus lacks it.
6. **Alternatives not selected:** List every track/path considered, its evidence counts and denominators where available, prize/rule facts, strengths, weaknesses, and the statistical or practical reason it ranked below the recommendation. Give opportunity cost and what new evidence would change the ranking. Do not imply an alternative was rejected “statistically” when only qualitative evidence exists.
7. **Buildable direction(s):** Offer a small set of concrete product hypotheses derived from the evidence, not a single generic brainstorm. For each, explain the evidence-to-feature chain, likely prize fit, demo, key risk, and which assumptions require the builder's judgment.
8. **Sources and limits:** List the most decision-relevant primary sources, research window, missing evidence, adapter calls/blocks, and any analyst disagreements.

Keep the answer proportional to the decision. If the user wants a durable artifact and a workspace is available, save one consolidated `[event]-trackmax-strategy.md` using a lowercase kebab-case event name; include the track comparison and evidence links in that file rather than creating one file per analyst. If no workspace is available, deliver the complete strategy in the response.
