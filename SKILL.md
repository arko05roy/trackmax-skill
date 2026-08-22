---
name: trackmax
description: Reverse-engineer a hackathon track or domain from a named winner datasource, then score an idea only against the resulting historical pattern. Use for track-specific winner analysis or evidence-bound track-fit scoring; not for generic product advice.
---

# Trackmax

Trackmax has two explicit modes. The objective is **track hijacking**: establish what projects historically won a narrowly defined track/domain and evaluate resemblance to that evidence. Do not turn either mode into generic startup, UX, or hackathon advice.

## Invocation and inputs

Use one of these forms:

```text
trackmax/analyse <datasource> <track-or-domain> <chain>
trackmax/rate <path-to-name-analysis.md> <path-to-idea.md>
```

`analyse` requires all three inputs. A datasource may be a connected research source or an installed skill such as `$ethglobal:ethglobal-skills` or Colosseum Copilot. The track/domain is the exact prize sponsor/track when available (for example `The Graph`) or a defined domain (for example `SocialFi`). The chain is an inclusion filter, not decorative metadata.

Read [the analysis protocol](references/analyse.md) before running `analyse`. Read [the rating protocol](references/rate.md) before running `rate`.

## Non-negotiable evidence rules

- Do not claim that a pattern causes wins when only winner-only data is available. Call it a **winner-set prevalence** or **historical similarity signal**.
- Never substitute generic recommendations for project-level analysis.
- Keep the exact raw project record, award, event, year, source link, chain evidence, and inclusion tier for every included project.
- Every percentage must state its denominator and whether categories overlap.
- If the datasource cannot support a required filter, state the limitation in the analysis and exclude unsupported numerical claims. Do not silently fill gaps from memory.
- Do not discard inconvenient winner records to make a trend look stronger.

## Output contracts

### `trackmax/analyse`

Write exactly one durable report named `[name]-analysis.md`, using a lowercase kebab-case `name` derived from the target (for example `the-graph-analysis.md`). The report is an evidence artifact, not a proposal. It must contain the complete project ledger, per-project analysis, trend matrices, percentages, recency analysis, limitations, and a `TRACKMAX_RATE_MODEL` block specified in the analysis protocol.

The prose conclusion may describe recurring project archetypes and intersections, but must not say what the user should build.

### `trackmax/rate`

Read the analysis report and idea document. Score only the observable predicates and weights in that report's `TRACKMAX_RATE_MODEL`. Do not add a personal rubric, judge novelty, reward polished writing, or penalize an idea for factors absent from the model.

If the inputs are valid, the final answer must contain exactly one token in this format:

```text
7.4/10
```

No explanation, heading, markdown, or rationale may accompany the score. If evidence in the idea document is missing, mark the relevant predicates unmet; do not infer hidden implementation.
