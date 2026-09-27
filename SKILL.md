---
name: trackmax
description: Research hackathon winners, compare prize paths, and recommend an evidence-led strategy to maximize the chance of winning at least one prize. Use for hackathon track/prize analysis, winner research, and strategy for events with or without tracks.
---

# Trackmax

Trackmax is a senior-analyst workflow for **maximizing the chance of winning at least one prize**, not for chasing first place at any cost. Count every eligible award, including a lower-ranked or small cash prize such as $10; do not filter out a prize for being too small unless the user sets a floor. It treats prize selection as a greedy, evidence-led optimization problem: find the strongest feasible path to any award, then look for legitimate ways to cover additional prizes. It does not claim that an LLM can originate genuinely novel ideas on demand; its strength is research, comparison, and pressure-testing. Human taste and original insight remain valuable inputs.

## Choose the workflow

- **Hackathon with tracks/prizes:** Get the complete track and prize list, official rules, and any known build constraints. Read [the strategy protocol](references/strategy.md), [the subagent protocol](references/subagents.md), and [the research-source adapters](references/research-sources.md). Research every track independently, then compare the results centrally.
- **Hackathon without tracks:** Read the same three protocols. Delegate one investigation to ecosystem/winner history and another to high-value keywords and current priorities, then synthesize both.
- **One track/domain, historical analysis only:** Use `trackmax/analyse <datasource> <track-or-domain> <chain>` and read [the analysis protocol](references/analyse.md). This produces historical evidence, not a project recommendation.

Infer the intended workflow from the request. Ask only for missing information that materially changes eligibility or the recommendation; otherwise state reasonable assumptions and proceed. If the user asks for ideas, provide evidence-backed directions and multiple useful keywords as leads—not claims of originality.

## Operating principles

- Behave like a skeptical senior analyst: separate evidence, inference, and unknowns; look for counterevidence; disclose missing coverage; correct weak assumptions without politeness padding.
- Study the combination that can matter: past winners in this event or ecosystem, official prize criteria, gaps in the ecosystem, current winner patterns, sponsor priorities, and what can actually be built and demonstrated.
- Do not limit research to the target chain. For Web3 projects, run ETHGlobal and Colosseum as separate source lanes and look for transferable product patterns, keywords, and capabilities in the other ecosystem; validate any claimed local gap with target-specific sources.
- Treat memorability and showoff value as a major strategic factor. Seek a concrete, surprising live demonstration that makes people think, “Our ecosystem can do that?” A technically simple project can qualify; complexity alone is not a differentiator. Never call this a guaranteed winning formula.
- Winner-only records show what won, not what failed or why it won. Calculate award rates only when a complete comparable entrant denominator is available. Never invent probabilities or imply correlation is causation.
- Treat “gaps” as research hypotheses, not proof of unmet demand. Treat trend and keyword frequency as evidence of attention, not evidence of a winning idea.
- Prefer one coherent product that genuinely satisfies multiple prize criteria when the evidence and build scope support it. Do not stretch a weak fit across tracks to inflate coverage.

## Deliverable

For a strategy request, present the primary path, why it leads, the concrete demo/memorability opportunity, useful keywords, evidence and uncertainty, and the strongest alternatives not selected. Compare alternatives with observed counts, denominators, eligibility, prize values/ranks, and disclosed scoring assumptions where available. State why each was passed over and what evidence could change the decision. Do not present a relative score as a win probability.

For a historical-only analysis, follow its existing report contract in [the analysis protocol](references/analyse.md). For every workflow, link claims to sources and label what is observed, interpreted, or unknown.
