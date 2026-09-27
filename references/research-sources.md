# Trackmax research-source adapters

This protocol keeps research reproducible and prevents an agent from substituting remembered or generated claims for fetched evidence. Load it for every Trackmax strategy task.

## Source routing

Treat ETHGlobal and Colosseum Copilot as **independent research corpora**, not mutually exclusive event selectors. For every crypto/Web3 Trackmax task, each research agent must independently query both skills—even when the hackathon or proposed product belongs to only one ecosystem. Use ETHGlobal for Ethereum/ETHGlobal project and prize history and cross-ecosystem hackathon analogues; use Colosseum for Solana builder/project history, archives, and cross-ecosystem analogues. Do not wait for one source's results before querying the other or skip one because the target chain differs. Only skip both for a clearly non-crypto task; if one corpus has no relevant coverage, record the attempted queries and say so. For every agent, record each source corpus, its exact calls, result counts, and any blocked source separately.

For every record, keep three distinct fields: `source_corpus` (`ETHGlobal`, `Colosseum`, or `web/official`), `project_ecosystem` (only what the project evidence establishes), and `target_relationship` (`exact-event/prize`, `same-ecosystem analogue`, or `cross-ecosystem analogue`). Do not infer project ecosystem from the corpus that returned it. Cross-ecosystem records can reveal transferable workflows, primitives, keywords, and gaps; they are never winners of the target prize. Keep each corpus's counts and denominators separate. Before calling something absent from the target ecosystem, search target-specific official sources, products, repositories, and demos; absence from either API corpus alone is not proof of absence.

### ETHGlobal skill/API

Use the `$ethglobal-skills` adapter for exact ETHGlobal awards and event rules, and as an independent cross-ecosystem project/prize-history source for every crypto/Web3 task.

1. Explicitly activate/read `$ethglobal-skills` before research. Follow its exact event-name and sponsor-name rules; do not improvise API parameters from memory.
2. Resolve the exact event name from that skill's list **only when the target event is ETHGlobal**. If a venue has multiple years and the intended year is unknown, ask the main analyst for clarification; do not silently pick one. Do not call the unsupported `/api/events` endpoint.
3. For an ETHGlobal target, use the event-wide prize list and exact rules included in the canonical packet when the coordinator has already fetched them; otherwise retrieve them with `GET https://ethglobalskills.vercel.app/api/prizes?event=<exact-event-name>`. For a cross-ecosystem analogue search, independently query `GET /api/projects?keyword=<specific problem-or-workflow>&include=description,how_its_made&limit=100`. When an exact sponsor is meaningful, resolve its exact name with `/api/sponsors?keyword=<term>` before querying sponsor prizes/projects. Preserve exact event, award, description, and qualification fields.
4. If the target event is ETHGlobal, retrieve exact winners with exact event, sponsor, and prize filters, using `include=description,how_its_made&limit=100` where useful. Query finalist records separately when useful. Set `pool=true` only when pool prizes are within scope. For any other target event, use the supported keyword or sponsor/prize query to retrieve ETHGlobal analogues; do not imply they won the target event. Every Web3 task must run a distinct cross-ecosystem analogue search in this corpus in addition to any exact-event lookup. Label analogues and retain real event/award names. Record result counts; disclose if the 100-result cap or missing fields prevents full coverage.
5. Treat API results as an index to records, not proof of unstated behavior. Follow each decision-relevant project's returned project URL, GitHub URL, or demo URL. Verify official eligibility and key implementation claims against the official event/sponsor page or a first-party project source.
6. After the first API call, inspect the `X-Skill-Version` response header as the skill requires; report a newer version to the main analyst. Use `web_search` independently to discover and cross-check official pages, past project pages, current sponsor documentation, analogues, and counterexamples. Log this search separately from API retrieval.

The API allows 10 free requests per minute. Budget across all agents as one shared quota, leave headroom, and queue calls before the limit. For tracked-event research, use no more than two ETHGlobal API-using agents in a wave and keep each agent to at most three API requests per rolling minute; reuse the verified event/prize packet rather than repeating identical list calls. A `402` can require a paid request. Never auto-install AgentCash, initiate payment, or make a paid retry; stop, mark the source blocked, and let the main analyst ask the user whether to proceed. Use this adapter's exact endpoints and API semantics only after reading the installed skill; the list above is the Trackmax minimum, not a replacement for its full instructions. If the skill/API is unavailable or returns no records, record that and continue only with sources actually retrieved. Never invent an ETHGlobal result.

### Colosseum Copilot / Solana project corpus

Use `$colosseum-copilot` for Colosseum/Solana-specific history, and as an independent cross-ecosystem project/archive source for every crypto/Web3 task—even if the target project is on Ethereum or another chain. Its corpus does not determine target-event eligibility.

1. Explicitly activate/read `$colosseum-copilot` and its API reference. Treat it as a corpus/search adapter, not an authority on official prize wording or current facts.
2. Before any Copilot API request, follow its required auth preflight: check only whether `COLOSSEUM_COPILOT_PAT` is set (never print or inspect its value), set the non-secret `COLOSSEUM_COPILOT_API_BASE` to `https://copilot.colosseum.com/api/v1` only if it is absent, then call `GET /status`. If the PAT is absent or `/status` is unauthorized, stop Copilot API calls and report `blocked: credentials/unavailable`. Never ask an agent to reveal, print, paste into a prompt, or log the PAT. Never send the PAT to the main analyst. If Copilot is essential to the decision and no adequate fallback exists, ask the main analyst to request access through the secure environment-variable flow; do not ask for a token in chat or a task prompt.
3. Call `GET /filters` to resolve exact hackathon and track keys and canonical `startDate` values. Use only returned slugs/keys and chronology; do not infer years or ordering from event names.
4. Run at least two materially different `POST /search/projects` searches per agent. Search both the proposed product behavior and the underlying user/problem; translate the query into Solana-relevant terminology when the target product is on another chain. Query winners with `winnersOnly: true`; query the broader corpus separately when comparing winner patterns to projects overall. Use `acceleratorOnly: true` when making portfolio/crowding claims. Use a tag-filter follow-up based on relevant result tags when the initial results warrant it. For every Web3 task, run these as an independent Solana/cross-ecosystem evidence lane, not as a substitute for any exact-event research.
5. When useful, run `POST /analyze` for the exact event cohort with `winnersOnly: true` and a separate `winnersOnly: false` cohort; request relevant dimensions such as `tracks`, `problemTags`, `solutionTags`, `primitives`, or `techStack`. Label these as counts in the Copilot corpus, not complete event entrant denominators unless completeness is independently documented. Do not treat search `totalFound` estimates as exact populations.
6. For key projects, fetch `GET /projects/by-slug/:slug` to get full project descriptions, date, track, prize, and first-party links. Verify prize and feature claims on event pages, repositories, or demos.
7. For non-trivial opportunity analysis, perform at least one focused `POST /search/archives` query. Use the Copilot skill's 3–6 keyword guidance, inspect `searchTier`, `publishedAt`, and snippet relevance, and cite only relevant documents. Archive matches are context, not hackathon-winner records.
8. After the first API call, inspect the `X-Copilot-Skill-Version` response header as the skill requires; report a newer version to the main analyst. Use `web_search` independently for current primary sources, event/winner pages, product status, sponsor priorities, and falsification. Do not let Copilot project or archive summaries replace web search.

Follow the Copilot skill's rate/concurrency limits, retry rules, and endpoint schemas. Its API allows at most two concurrent requests per user: make calls serially inside each agent and coordinate across agents so no more than two Copilot requests are in flight globally. If authorization, rate limits, or service availability block a call, record the exact endpoint/status without secrets, stop retrying beyond the documented guidance, and use other verifiable sources. Never fabricate Copilot results or treat its corpus as complete by default.

### Non-Web3 scopes or unavailable adapters

For a clearly non-crypto scope, do not invoke ETHGlobal or Colosseum merely to fill a checklist; use relevant event/sponsor records, first-party project pages, documentation, and the web-search protocol below. For Web3 scopes, a different target chain is not a reason to skip either corpus: run the cross-ecosystem query first, then report if it returned no relevant records. If an applicable installed skill is unavailable or blocked, state that explicitly and continue only with retrievable evidence; the main analyst must retain the source gap in the final answer.

## Mandatory verbose web-search protocol

Every research agent must use the runtime's actual `web_search` tool directly. Connector/API calls are additional retrieval paths, not substitutes. Make **at least eight separate, focused `web_search` calls per agent**; aim for 10–12 for a broad event. One call per distinct query—do not pack multiple searches into one query or count API calls as web searches. Request `count: 10` where supported. If a category below has no applicable evidence, replace it with a distinct falsification or historical query and explain why.

### Track/prize analyst query set

Use separate searches for these angles, tailored with the exact event, sponsor, track, years, and at least one cross-ecosystem equivalent of the core workflow:

1. Official event track rules, eligibility, and judging criteria.
2. Official sponsor prize wording, qualifications, technical docs, and required integrations.
3. Exact-track winners at the current event/edition.
4. Exact or comparable prize winners from earlier editions/years.
5. What the most relevant winning project actually built (project page/demo).
6. The winner's first-party repository, technical write-up, or implementation detail.
7. Dated sponsor/ecosystem roadmap, grant, launch, or leadership priority.
8. Relevant technical primitive/standard release and the capability it enabled.
9. Cross-ecosystem products/projects implementing the same user workflow or primitive, to identify transferable features and keywords.
10. Existing target-ecosystem products or hackathon projects that could disprove the proposed gap.
11. Crowding, rejected submissions, judge feedback, or non-winner evidence if publicly available.

### Untracked ecosystem-history analyst query set

Search separately for official event criteria; recent winners; older winner cohorts; finalist/recognized project details; ecosystem roadmap/leadership priorities; grants and current launches; relevant new technical capabilities; analogous projects/features in the other ecosystem; target-ecosystem products that counter a claimed gap; public judge feedback/postmortems; and evidence of crowded or declining patterns.

### Untracked keyword analyst query set

Search separately for exact official ecosystem terminology; current sponsor/leadership language; roadmaps/grants; recent product or protocol releases; relevant technical documentation/standards; wording in winners' project descriptions; past winner demo/project pages; independent current ecosystem discussions; analogous keywords and product behaviors from the other ecosystem; existing target-ecosystem products using the proposed phrases; and contrary/declining signals. Record exact phrases, dates, source diversity, and which corpus each came from; do not count syndicated copies as independent evidence.

### Narrow verifier query set

Run separate, claim-focused searches for: the exact claim and event; the official rule/statement owner; the current official page; the relevant first-party project/docs/repository; older dated versions or announcements; a plausible counterexample; independent corroboration; and direct contradiction/withdrawal/changed status. If a query cannot be grounded in the claim, replace it with another falsification angle rather than issuing cosmetic rewrites.

### Search logging and source verification

For each `web_search` call, log the exact query, call date, requested result count, returned result count when visible, and the URLs/titles selected or excluded with a short reason. Use at least five substantive results across each agent's total search set when the queries return them, with primary sources prioritized. Do not pad with irrelevant results. Search snippets only discover sources: `web_fetch` the most important official/first-party pages before treating a claim as verified. Fetch at least three independent primary/first-party pages per agent where they exist; if fewer exist, log the attempted searches and coverage limit. Follow citations from project pages to code, demo, documentation, or official award confirmation.

A search call is a research action, not proof. Every material statement must map to a fetched source, a clearly attributed source/API record, or an explicit `unknown`. Include a query ledger in the agent return so the main analyst can verify that research actually happened.
