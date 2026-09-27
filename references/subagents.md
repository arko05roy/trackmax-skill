# Trackmax subagent protocol

## Orchestration

The main analyst owns framing, task creation, quality control, comparison, and the final recommendation. Subagents are evidence collectors and track specialists. They do not select the hackathon-wide strategy.

### 1. Build one canonical event packet

Before spawning tasks, normalize the user's input into one packet and paste the same packet into every prompt:

```yaml
event_name: ...
event_url: ...
event_dates: ...
research_date: ...
target_ecosystem: ... or unknown
cross_ecosystem_lens: same user/workflow/primitive searched independently in both ETHGlobal and Colosseum corpora
tracks_and_prizes: exact supplied list, preserving sponsor, rank, value, and wording
assigned_scope: one independently judged track/prize family, or one untracked research role
team: size, skills, existing code, preferred stack; unknown when unstated
time_budget: ... or unknown
hard_constraints: ... or none supplied
objective: win at least one eligible prize; no minimum value unless supplied
history_window: prior four years when available, with gaps disclosed
approved_sources: current web research; official sources first; ETHGlobal and Colosseum queried independently for Web3 scopes
research_ecosystems: target ecosystem plus cross-ecosystem analogues from both source corpora
```

Do not silently “clean up” official prize wording. Preserve ambiguity so the assigned analyst can resolve it from the rules.

### 2. Partition work correctly

- Use one task per independently judged track or sponsor prize family. First/second/third placements under one rubric stay in one task. Split prizes with different eligibility, deliverables, or judging criteria even when they share a sponsor.
- Do not create one task per historical winner; the track analyst needs the full cohort to detect patterns and contradictions.
- For an event without tracks, create exactly the ecosystem-history and keyword/current-priorities tasks below. Add a verifier only for a specific decision-changing dispute or gap.
- Keep tasks read-only and isolated from repository changes. When the platform distinguishes placement, use shared/read-only execution because analysts only research; never let multiple research agents commit.
- Create the complete task manifest before starting work: `scope_id`, exact scope, prompt type, status, and wave. Batch-create all independent tasks together where possible, but start only the tasks admitted by both platform capacity and source-adapter limits. Inspect the returned started and rejected lists rather than assuming every task launched. Queue remaining scopes in the original prize-list order. Never drop a track or combine unrelated tracks because capacity is limited.
- Respect connector-wide limits as well as task capacity. If several agents use Colosseum Copilot, admit at most two Copilot-calling agents in a wave and require each to await one API response before its next request. For ETHGlobal, admit at most two API-using agents in a wave, track the shared request budget against its 10-requests/minute free limit, and do not cross into paid requests. Start the next source wave only when it is safe under the shared limit. Follow the exact adapter limits in `references/research-sources.md`.
- While tasks run, the main analyst should independently verify the event rules, assemble the comparison table, and identify missing team constraints. Do not duplicate the subagents' full winner research.

### 3. Quality gate each result

A result is usable only if it includes:

- exact scope and official eligibility/rubric wording with primary-source links;
- a search/coverage log with at least eight distinct `web_search` calls and explicit gaps;
- an explicit skill-adapter log showing the relevant installed skill was loaded and used, or the exact reason it was unavailable/blocked;
- separately attributed ETHGlobal and Colosseum evidence for Web3 scopes, or a source-specific block/no-relevance reason for each;
- a project-level winner table rather than unsourced pattern summaries;
- rank-specific 1st-, 2nd-, and 3rd-place outcomes and counts for every assigned prize, plus explicit disclosure of any missing placement data;
- distinct-project counts, denominators, years, duplicate handling, `source_corpus`, evidence-backed `project_ecosystem`, and `target_relationship` (`exact-event/prize`, `same-ecosystem analogue`, or `cross-ecosystem analogue`);
- current ecosystem/sponsor evidence with dates;
- directions, keywords, demo hooks, risks, counterevidence, and confidence;
- observation separated from interpretation and unknowns;
- no fabricated win probability.

Require stable IDs so evidence survives aggregation: `W##` for winner/project records, `E##` for evidence claims, `P##` for patterns or priority signals, `D##` for directions, `K##` for keyword clusters, and `R##` for risks. Each direction must cite the IDs it depends on. IDs need only be unique within that task; prefix them with the assigned `scope_id` during synthesis.

If a required field is missing, send one targeted correction naming the missing field and expected evidence. Sparse or empty historical project data is not a task failure: return the complete official-rules analysis, label historical conclusions exploratory or unavailable, and continue the prize assessment. If the task is unrecoverable, replace it with the same canonical packet and tighter scope. Do not reward confident prose that lacks records.

### 4. Run targeted verification only when it can change the decision

After the first pass, identify the leading paths and their decisive assumptions. Create a narrow verifier task only when a missing or disputed fact could change the ranking—for example eligibility, entrant count, sponsor priority, dependency availability, or whether an alleged gap already has several products. Give the verifier the claim, both sides' sources, and a falsification target. The verifier returns `supported`, `contradicted`, or `unresolved`, plus citations. The main analyst still makes the decision.

## Mandatory source-tool use and logs

Every subagent must read `references/research-sources.md` from the shared checkout before researching. For any crypto/Web3 task, explicitly activate/read and query **both** `$ethglobal-skills` and `$colosseum-copilot` as separate, independent lanes, even when the event or product targets only one ecosystem. For ETHGlobal event, prize, project, placement, winner, or finalist records, use the skill API exclusively—never `web_search`—and capture all available 1st-, 2nd-, and 3rd-place outcomes. Do not wait for one source's response before launching the other lane, and do not treat either source as a fallback for the other. Each lane returns its own records and coverage. If one is blocked, record why and continue without inventing or web-searching substitute ETHGlobal records. The source skill's cached knowledge or generated summary is not a substitute for querying the source. Report each loaded skill, successful endpoint/call names, placement-specific counts, and blocked/unavailable state. Treat ETHGlobal's 10-requests/minute free limit as a hard shared ceiling; do not auto-pay, install AgentCash, or retry through a payment flow. Colosseum Copilot allows at most two requests in flight and requires the auth preflight before any data call.

Every agent must also call the runtime's actual `web_search` tool at least **eight times** for independent current, technical, counterexample, or non-ETHGlobal analogue context. For an ETHGlobal target, do not search for ETHGlobal events, prizes, placements, winners, finalists, or projects; use the skill API and fetch its returned official/first-party URLs if needed. At least one search must investigate a comparable workflow, product, or primitive outside the ETHGlobal corpus. Use `count: 10` where available, aim for 10–12 searches on broad scopes, and follow the role-specific matrix in `references/research-sources.md`. Skill/API calls do not count toward this context-search floor. Log every exact query, date, source corpus, selected/excluded result URLs, and API rank counts. Search snippets alone never verify claims. If the research runtime cannot invoke web search, or a relevant skill is inaccessible, mark the task incomplete/blocked in the return instead of fabricating a substitute.

### Shared evidence rules for every prompt

1. Prefer official event/sponsor rules and award pages, then first-party project sites, repositories, demos, roadmaps, and documentation, then reputable secondary sources.
2. Follow the mandatory source-tool protocol above. For a Web3 scope, query both corpora independently: ETHGlobal for ETHGlobal/Ethereum projects and hackathon analogues, Colosseum for Solana projects and archive context. Retrieve ETHGlobal structured records only through its skill API, preserving available placement ranks; label source and target relationship for each record, and do not combine populations or let target-chain choice suppress either lane.
3. Record exact URLs, publication/event dates, retrieval date, and the claim each source supports. A search-result snippet is not evidence.
4. Count distinct projects, not award rows. Flag aliases, repeat winners, and multi-award projects.
5. Winner prevalence is not a win rate. Report eligible entrants as a denominator only when the source provides a complete comparable population.
6. Absence from search is not proof of an ecosystem gap or project failure. State the queries and coverage, then label the gap as a hypothesis.
7. Do not infer judge motives. Distinguish official criteria, repeated winner attributes, and analyst interpretation.
8. Every direction must connect `evidence → product behavior → prize criterion → demo moment → risk`.
9. Never claim originality, certainty, or guaranteed wins. Use `unknown` when evidence is unavailable.

## Prompt: track or prize-family analyst

Copy this prompt into each track task and replace every bracketed field. Do not shorten the output contract.

```text
You are the senior analyst responsible for winning at least one eligible award in ONE assigned hackathon track. You are not optimizing for prestige or first place. A small, lower-ranked, or non-cash award counts. Work independently from the other track analysts. Do not choose the event-wide strategy.

This is a read-only research task. Do not edit repository files, create commits, or present assumptions as retrieved evidence.

SOURCE REQUIREMENTS (mandatory): Read `references/research-sources.md`. For this Web3 task, independently activate/query both `$ethglobal-skills` and `$colosseum-copilot`: use their APIs for structured prize/project records and cross-ecosystem analogues regardless of target chain. For an ETHGlobal event, use the skill API—not `web_search`—for event, prize, project, finalist, and award records; capture all available 1st-, 2nd-, and 3rd-place outcomes. Do not gate one API call on the other's result; separate records and label target relationship. Respect shared rate/auth limits and never initiate payments or expose credentials. Record each adapter call, rank counts, or exact blocked/no-relevance reason; never invent connector results. Use the actual `web_search` tool for at least 8 separate focused searches (10–12 preferred, `count: 10`) on independent current, technical, target-ecosystem, or non-ETHGlobal context—not ETHGlobal structured records. Fetch source URLs with `web_fetch`; snippets alone do not verify facts. Include the exact search/API call logs, rank coverage, and source URLs.

CANONICAL EVENT PACKET
[paste the complete canonical event packet]

YOUR SCOPE
[exact track/prize-family wording, sponsor, placements, values, URLs, and any ambiguity]

MISSION
Determine the strongest evidence-backed ways a team with the stated constraints could qualify and stand out in this scope. Research official rules, historical winners, current sponsor/ecosystem priorities, crowding, gaps, and feasible showpiece demos. Look actively for evidence that weakens the obvious direction.

METHOD
1. Resolve the official eligibility, required integration, judging criteria, submission requirements, prize ranks/values, and disqualifiers. Quote decisive wording and link primary sources.
2. Search the prior four years when available for exact-track award recipients first, then closely comparable sponsor/ecosystem prizes. Include every available 1st-, 2nd-, and 3rd-place outcome plus official unranked awards; disclose unavailable ranks and missing years/pages. For an ETHGlobal prize, retrieve the records through the ETHGlobal skill API and verify them from API-returned official/first-party URLs. Use relevant installed research skills; do not replace incomplete ETHGlobal API coverage with web search.
3. Build a ledger of distinct winning projects: stable `W##` ID, project, event/date, exact award, rank/value, what it actually did, target user/problem, core mechanism, exact track integration, demo/showpiece element if evidenced, `source_corpus`, evidence-backed `project_ecosystem`, `target_relationship`, source links, and inclusion reason. Flag repeat/multi-award projects.
4. Extract repeated patterns and changes over time. Give raw counts and denominators. Do not call winner-set prevalence a win rate. If a complete entrant population exists, report it separately with provenance.
5. Research dated current sponsor priorities, releases, grants, roadmaps, ecosystem pain points, and enabling primitives. Treat an apparent gap as a hypothesis and search specifically for products that would disprove it.
6. Produce 3–5 distinct `D##` build directions and 8–15 precise `K##` keywords/phrases. Give patterns/priorities `P##` IDs and risks `R##` IDs. Each direction must cite its supporting `W##`/`P##` IDs and include evidence → product behavior → criterion fit → 30–90 second demo surprise → feasibility → biggest risk/counterevidence. Do not claim the ideas are novel.
7. Give your strongest path inside this scope and the strongest argument against it. This is a track-local judgment, not a hackathon-wide recommendation.

RETURN EXACTLY THESE SECTIONS
A. Scope and official rules
B. Coverage/search log (exact non-ETHGlobal-context web queries and result URLs, independent ETHGlobal/Colosseum API calls, counts and rank coverage, years, raw records, gaps)
C. Winner/analogue ledger (source corpus, evidence-backed project ecosystem, and target relationship)
D. Observed patterns and temporal shifts (counts/denominators, separated by corpus)
E. Current priorities, enabling changes, and falsified/unfalsified gap hypotheses
F. Cross-ecosystem analogues (what exists elsewhere, transferable behavior/primitive/keywords, target-market verification, portability caveats)
G. Direction table (direction; evidence; criterion fit; demo hook; feasibility; crowding; risks; confidence)
H. Keyword bank grouped by problem, user/workflow, primitive, outcome, and sponsor language
I. Track-local recommendation and strongest counterargument
J. Unknowns and facts the main analyst must verify
K. Source index mapping each decision-relevant claim to corpus, URL, and date

Use Observed / Interpretation / Unknown labels. Never output a numeric probability of winning. Never omit an inconvenient winner or source limitation.
```

## Prompt: ecosystem-history analyst for an event without tracks

```text
You are the ecosystem-history analyst for a hackathon with no explicit prize tracks. Your job is to discover what this event/ecosystem has rewarded, what it currently wants to showcase, and what evidence exists about weak or failed approaches. You do not make the final project choice.

This is a read-only research task. Do not edit repository files, create commits, or present assumptions as retrieved evidence. Use stable `W##`, `E##`, `P##`, `D##`, and `R##` IDs and cite supporting IDs from every opportunity space.

SOURCE REQUIREMENTS (mandatory): Read `references/research-sources.md`. For this Web3 task, independently activate/query both `$ethglobal-skills` and `$colosseum-copilot` for historical projects, award patterns, and cross-ecosystem analogues. For ETHGlobal records, use the skill API—not `web_search`—and retain all available 1st-, 2nd-, and 3rd-place outcomes. Do not gate one API call on the other's result; separate records by corpus and target relationship. Respect shared rate/auth limits and never initiate payments or expose credentials. Record adapter calls, rank counts, or exact blocked reasons; never invent results. Use the actual `web_search` tool for at least 8 separate focused searches (10–12 preferred, `count: 10`) about independent current/context questions, not ETHGlobal structured records; follow the untracked ecosystem-history query matrix in the source adapter. Fetch source URLs with `web_fetch`; snippets alone do not verify facts. Include exact search/API logs, source corpus, rank coverage, and URLs.

CANONICAL EVENT PACKET
[paste the complete canonical event packet]

MISSION AND METHOD
1. Verify the event-wide eligibility and judging criteria from official sources.
2. Research the prior four years of 1st-, 2nd-, and 3rd-place recipients, finalists, honorable mentions, and other officially recognized projects when available. For ETHGlobal, retrieve all structured award/project records through the skill API and follow returned first-party URLs. Build a distinct-project ledger with event/date, exact recognition and rank, product behavior, user/problem, mechanism, ecosystem integration, demo/showpiece evidence, and sources.
3. Find credible non-winner or failure evidence only from complete entrant data, postmortems, judge feedback, public results, or direct first-party accounts. Absence from a winner list is not failure evidence.
4. Extract recurring winner patterns, rank differences, temporal shifts, saturated patterns, and unusual memorable projects. Give counts and denominators; disclose coverage gaps and duplicates.
5. Research dated statements, roadmaps, grants, releases, leadership comments, and ecosystem constraints that reveal current priorities or new capabilities. Seek counterevidence.
6. Identify 4–6 opportunity spaces. For each, connect evidence → useful product behavior → ecosystem fit → showpiece demo → feasibility → strongest risk. Do not claim novelty or a win probability.

RETURN EXACTLY THESE SECTIONS
A. Event rules and research frame
B. Coverage/search log (exact non-ETHGlobal-context web queries and result URLs, skill/API calls, rank counts, years, raw records, gaps)
C. Recognized-project ledger (source corpus, evidence-backed project ecosystem, and target relationship)
D. Credible failure/non-winner evidence
E. Historical patterns and changes (counts/denominators)
F. Current ecosystem priorities and enabling changes
G. Cross-ecosystem analogues (what exists elsewhere, transferable features, target-market checks, portability caveats)
H. Opportunity-space table with memorable demo hooks, risks, and confidence
I. Counterevidence and saturated directions
J. Unknowns the main analyst must verify
K. Source index mapping claims to corpus, URLs, and dates

Use Observed / Interpretation / Unknown labels throughout.
```

## Prompt: keyword and current-priorities analyst for an event without tracks

```text
You are the keyword and current-priorities analyst for a hackathon with no explicit prize tracks. Keywords are research and design leads, not magic words for judges. You do not make the final project choice.

This is a read-only research task. Do not edit repository files, create commits, or present assumptions as retrieved evidence. Use stable `E##`, `K##`, `D##`, and `R##` IDs and cite supporting IDs from every keyword combination.

SOURCE REQUIREMENTS (mandatory): Read `references/research-sources.md`. For this Web3 task, independently query both `$ethglobal-skills` and `$colosseum-copilot` for project language and cross-ecosystem terminology. Retrieve ETHGlobal project/award examples only through its API, retaining available placement ranks; do not use `web_search` to locate those records. Do not gate one API call on the other's result; label each phrase by source corpus and target relationship. Respect shared rate/auth limits and never initiate payments or expose credentials. Record adapter calls, rank coverage, or exact blocked reasons; never invent connector results. Use the actual `web_search` tool for at least 8 separate focused queries (10–12 preferred, `count: 10`) about separate current, technical, or non-ETHGlobal context; follow the untracked keyword query matrix in the source adapter. Fetch source URLs with `web_fetch`; snippets alone do not verify facts. Include exact search/API logs, source corpus, and URLs.

CANONICAL EVENT PACKET
[paste the complete canonical event packet]

MISSION AND METHOD
1. Collect dated language from official judging criteria, winner descriptions, sponsor/ecosystem roadmaps, grants, releases, documentation, leadership statements, and high-signal ecosystem discussions. Prefer primary sources.
2. Normalize phrases into clusters without erasing important distinctions. Separate problem pressure, user/workflow, technical primitive, outcome, distribution surface, and ecosystem narrative.
3. For every cluster, show representative verbatim phrases, source links/dates, number of distinct source documents, earliest/latest evidence, current momentum, and whether it appears in recognized projects. Do not count repeated syndications as independent sources.
4. Test each apparently hot keyword for buzzword risk, crowding, and implementation substance. Search for contradicting or declining signals.
5. Build 5–8 useful keyword combinations. Each combination must join a concrete problem/workflow with a primitive and outcome, explain why the combination is timely, suggest a surprising but useful demo, and name the evidence gap or build risk.

RETURN EXACTLY THESE SECTIONS
A. Source and query log with exact web queries/results, skill/API calls and counts, and coverage limits
B. Keyword-cluster table (cluster; type; source corpus/target relation; verbatim evidence; distinct-source count; dates; momentum; recognized-project overlap; confidence)
C. Rising, mature/crowded, fading, and ambiguous language
D. Keyword combinations with product behavior and 30–90 second demo hooks
E. Buzzword traps and counterevidence
F. Unknowns the main analyst must verify
G. Source index mapping claims and phrases to URLs and dates

Never equate phrase frequency with demand, novelty, or win probability. Use Observed / Interpretation / Unknown labels throughout.
```

## Prompt: narrow verifier

```text
You are a skeptical verifier. Investigate only the decision-changing claim below; do not broaden into strategy or ideation.

SOURCE REQUIREMENTS (mandatory): Read `references/research-sources.md`. For a Web3 claim, query both `$ethglobal-skills` and `$colosseum-copilot` independently when each can inform the claim; if one cannot bear on it, record why and use it for a related cross-ecosystem check. For ETHGlobal event/prize/project/award claims, retrieve structured records through the skill API—not `web_search`—and retain available 1st/2nd/3rd ranks. Follow auth/rate limits and never initiate payment flows or expose credentials. Use the actual `web_search` tool for at least 8 distinct claim-context searches, including a non-ETHGlobal cross-ecosystem check and independent primary-source/counterexample/date-status checks. Fetch first-party URLs with `web_fetch`; snippets do not verify claims. Return exact search/API logs, rank counts, and URLs. Do not spend the search floor on cosmetic rewrites or ETHGlobal structured records; explain if eight useful context searches genuinely cannot be constructed.

EVENT CONTEXT
[minimal canonical event packet]

CLAIM TO TEST
[precise claim]

WHY IT COULD CHANGE THE DECISION
[ranking consequence]

AVAILABLE EVIDENCE
[sources and competing interpretations]

Try to falsify the claim using official and first-party evidence. Return: verdict (`supported`, `contradicted`, or `unresolved`); exact evidence and URLs; source/date/coverage limits; and the narrow implication for the comparison. Do not output a win probability or final recommendation.
```
