---
name: trackmax-rate
description: Independently rate a hackathon idea against a supplied or independently researched winner corpus, verified prize rules, and current track trends. Use when a builder wants best/base/worst outcomes, a track-direction assessment, a course recommendation, and a transparent final rating.
---

# Trackmax Rate

Trackmax Rate is a **standalone analyst review for a fresh context window**. It evaluates the exact idea supplied against historical winners, current track direction, prize criteria, feasibility, and the idea's memorable demo potential. It does not depend on the earlier Trackmax conversation and does not promise a win.

## Run the protocol

1. Read [the complete rating protocol and agent prompts](references/rate.md). Freeze the supplied material into one packet; mark missing rules, corpus, or constraints `unknown` rather than assuming other chat context.
2. For each candidate prize track, run two independent research tasks: a historical-fit analyst and a trajectory/counterevidence analyst. Give both the same packet; do not show either agent the other's findings.
3. For every Web3 idea, each agent independently loads and queries both `$ethglobal-skills` and `$colosseum-copilot`, regardless of target chain. For ETHGlobal events, prizes, projects, finalists, and award placements, use `$ethglobal-skills` as the structured-record source; never use `web_search` to find or fill those records. Capture all available 1st-, 2nd-, and 3rd-place outcomes, and use `web_fetch` on URLs returned by the skill when first-party verification is needed. Keep the datasets separate. Each agent still makes at least eight focused `web_search` calls for independent current, technical, counterexample, or non-ETHGlobal context.
4. Validate eligibility, reconcile conflicting agent findings, rate the idea and track, and report best/base/worst cases, the strongest next move, the evidence limits, and the final score.

## Rating contract

The final `0–10` rating is a disclosed strategic-fit score, **not a probability**. A likelihood-style outlook must remain qualitative unless complete, comparable entrant data supports a carefully bounded historical base rate. Winner-only data cannot establish a personal probability of winning. Do not claim an outcome is certain; label the most supportable outlook and its confidence.

If the idea or candidate track is missing, ask only for that input. If the winner corpus or rules are missing, research them independently; ask only if the edition or another decision-critical fact cannot be verified. If raw winner data is supplied without a rating model, derive a model only from that evidence under the protocol; never invent a fallback rubric.
