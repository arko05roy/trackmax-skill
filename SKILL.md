---
name: trackmax
description: Build a plain-English, long-horizon winner cheat sheet for a hackathon track or domain. Use for track-specific winner analysis; not for generic product advice or idea scoring.
---

# Trackmax

Trackmax is a **track-hijacking cheat sheet** for builders: establish what projects historically won a narrowly defined track/domain, what they actually made, how winner patterns changed over time, and which patterns were emerging, accelerating, mature, crowded, mutating, fading, or re-emerging at each point. Do not turn it into generic startup, UX, or hackathon advice.

## Invocation and inputs

By default, a request that says only `trackmax` means **analyse**. Infer the datasource, target, and chain from the surrounding request when they are supplied. If one of these essentials is missing, ask one short question for the missing information. Do not require the user to write `trackmax/analyse`.

Use these forms when the user wants to be explicit:

```text
trackmax/analyse <datasource> <track-or-domain> <chain>
```

`analyse` needs a datasource, target, and chain. A datasource may be a connected research source or an installed skill such as `$ethglobal:ethglobal-skills` or Colosseum Copilot. The track/domain is the exact prize sponsor/track when available (for example `The Graph`) or a defined domain (for example `SocialFi`). The chain is an inclusion filter, not decorative metadata.

Read [the analysis protocol](references/analyse.md) before running `analyse`.

## Non-negotiable evidence rules

- Do not claim that a pattern causes wins when only winner-only data is available. In plain English, call it a repeated winner pattern or historical similarity signal.
- Never substitute generic recommendations for project-level analysis.
- Keep the exact raw project record, award, event, year, source link, chain evidence, and inclusion tier for every included project.
- Every percentage must state its denominator and whether categories overlap.
- If the datasource cannot support a required filter, state the limitation in the analysis and exclude unsupported numerical claims. Do not silently fill gaps from memory.
- Do not discard inconvenient winner records to make a trend look stronger.

## Output contracts

### `trackmax/analyse`

Write exactly one durable report named `[name]-analysis.md`, using a lowercase kebab-case `name` derived from the target (for example `the-graph-analysis.md`). The report is an evidence artifact, not a proposal. It must contain the complete project ledger, per-project analysis, micro-trend ledger, temporal trend matrices, cohort comparisons, timing assessments, percentages, recency analysis, limitations, and a `TRACKMAX_RATE_MODEL` block specified in the analysis protocol.

Write for developers, not business analysts. Use short sentences and explain technical or research terms the first time they appear. Make the report easy to skim: name the products, say what each one made and how it worked, then show the repeated patterns, micro-trends, mutations, and timing states behind those patterns. The prose conclusion may describe recurring project archetypes and intersections, but must not say what the user should build.
