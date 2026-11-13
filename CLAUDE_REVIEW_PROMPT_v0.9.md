# Claude Independent Review Prompt — MBI v0.9

Act as a skeptical, read-only reviewer. Review Codex's v0.9 audit report against the canonical specification. Do not edit the repository or perform broad rewrites.

Focus on:
1. Are Market Episode, Discovery Candidate, Setup Family, Setup Definition Version, Setup Scan, Setup Instance, Setup Occurrence Group, Entry Scenario, Parameter Experiment, Playbook, Trade Decision and Execution Fill correctly distinguished?
2. Does the design evaluate every eligible covered security/time occurrence, including non-matches and untraded setups, rather than learn only from executed trades?
3. Are technical conditions unambiguous (timeframe, MA type/period/input, completed bar, price basis, candle rule and signal timestamp)?
4. Are MA10/MA20 distances separately represented? Are percentage and ATR-normalized distances separate features? Are signal-time and entry-time MA references separate?
5. Can entry variants be compared as counterfactual scenarios without invented fills or contamination of the actual ledger?
6. Are technical, fundamental, issuer character, market regime, sector/cluster and ownership/UBO features point-in-time, sourced and versioned?
7. Are overlapping issuer/date/event/cluster/network cases treated as dependent rather than inflated independent samples? Are consecutive repeated matches grouped into one opportunity spell under a declared re-arm rule?
8. Does missing optional fundamental/UBO/broker context preserve a technically valid setup match while making only that subgroup comparison unavailable?
9. Are all parameter variants and negative results retained, with baseline, multiple-testing controls, time-based holdout/walk-forward, overlap purging/embargo and robustness-plateau analysis?
10. Does the model separate market-wide setup quality, user's selection, execution quality, risk behavior and realized outcome?
11. Does open-world discovery still find leads outside watchlists and feed episode/setup research without automatic promotion?
12. Are probabilities withheld until outcome definition, cohort, effective sample, out-of-sample validation and calibration are defensible?
13. Is the logical model minimal, non-duplicative and implementable on a local-first 8 GB laptop with no paid data dependency?

For each issue give severity (`BLOCKER`, `HIGH`, `MEDIUM`, `LOW`), exact file/line, evidence, consequence, reproduction/logic test and proposed minimal fix. Separate confirmed contradiction from design question.

Return final state: `ACCEPT_AUDIT` or `REVISE_AUDIT`. Do not edit. Do not claim a source/sample/test was checked unless it actually was. Do not approve a single opaque score, in-sample optimum or unsupported probability.
