# 00 — Product Charter, Invariants & Canonical Terms

## 1. Product objective

MBI exists to discover and explain **how Indonesian issuers and the market behave under different information and market conditions**, then measure how the user's decisions perform in those conditions.

It must help answer:

1. What information or condition was available, and when?
2. What was the observable market state before and during the event?
3. Which issuers, sectors or clusters were exposed, and what evidence supports that exposure?
4. How did price, volume, turnover, transaction flow, broker aggregates and liquidity respond?
5. Which historical episodes were comparable, including failures and counterexamples?
6. Which patterns are repeatable, under what conditions, and with what uncertainty?
7. How did the user's selection, setup and execution perform in comparable circumstances?
8. What should be watched next, and what evidence would confirm or invalidate the current reading?
9. What potentially relevant development, issuer, sector, cluster or unusual behavior exists outside the user's current radar, and why did MBI surface it?

MBI is not an autonomous stock picker, broker terminal, source of privileged information, insider-trading detector, causal oracle or auto-trading system.

## 2. Core design principle: rigid evidence, adaptive research

### Rigid core
- Immutable raw archive and traceable provenance.
- Correct timestamps, timezone, reporting period, publication/availability time and effective date.
- Explicit units, market segment, price adjustment basis and security identity.
- Deterministic metric contracts and tests.
- Separation of facts, observations, measurements, hypotheses, inferences and unknowns.
- Point-in-time historical replay with no look-ahead leakage.
- Versioned event taxonomies, feature definitions, detectors, playbooks and models.
- Reproducible predictions, outcomes, audit log, schema migrations and backup/restore.
- Human approval for material interpretation and production activation.

### Adaptive research layer
- New event types, issuer characteristics, market regimes and clusters.
- New market patterns and gameplay hypotheses.
- New supported metrics and detector operators.
- Revised playbooks and conditional recommendations.
- New source adapters after source/terms/format validation.

Adaptive means the research vocabulary and tested configurations can evolve. It does not mean unreviewed facts, auto-changing production code or automatic model promotion.

## 3. Canonical terms — do not use interchangeably

| Term | Exact meaning | Example | Not the same as |
|---|---|---|---|
| **Source / artifact** | Origin and original item collected | IDX daily batch file, issuer filing PDF, optional platform capture | Extracted fact |
| **Claim** | A statement made by a source or person | Management says capacity will double | Verified outcome |
| **Fact** | Source-backed statement with exact evidence and scope | Filing reports a share pledge on a stated date | Motive or future result |
| **Observation** | Directly recorded market/user observation | Bid queue changed at 10:17 WIB | Explanation of why |
| **Measurement / feature** | Deterministic value computed from identified inputs | Daily relative volume = 2.1x | AI estimate |
| **Event** | Time-bounded external or issuer occurrence/publication | BI rate decision, PUBEX, rights issue | Market response |
| **Market state** | Measured conditions around a timestamp/window | Weak breadth, high cluster turnover | A causal explanation |
| **Market episode** | Research container linking related context, observations, events, instruments and outcomes over time | Unusual activity before/after an issuer disclosure | One candle or one trade |
| **Discovery run** | A bounded scan/research process with a trigger, universe, coverage and rule version | Daily EOD sweep or a policy-triggered issuer search | Continuous monitoring unless data was actually collected |
| **Discovery candidate / research lead** | A potential issuer/theme/sector/cluster/event/pattern surfaced for investigation, with evidence and uncertainty | New regulation mapped to exposed issuers | Verified fact, trade signal or buy/sell call |
| **Exposure path** | Source-backed mechanism linking an external event to a sector/issuer | Policy → input cost → manufacturer | Keyword similarity or assumed beneficiary status |
| **Research brief / conviction support** | Evidence matrix containing support, contradiction, analogues, unknowns and monitoring conditions | Thesis evidence brief for an issuer | Automated conviction score or decision |
| **Hypothesis** | Testable proposed explanation/relationship | Turnover compression precedes breakouts in a cohort | Fact |
| **Research study** | Defined test of a hypothesis with sample/window/method | Event study over qualifying disclosures | Informal narrative |
| **Pattern** | Repeated measurable behavior supported by observations | High turnover + muted range followed by expansion | Guaranteed edge |
| **Detector** | Versioned rule/model that identifies a condition/pattern | Compression-expansion detector | Trading strategy by itself |
| **Gameplay** | A way the user or market participant may approach a market situation; initially can be informal | Wait for retest after unusual activity | Verified pattern |
| **Playbook** | Versioned operational decision procedure derived from a gameplay hypothesis | Eligibility, trigger, risk, exit, invalidation | Detector alone |
| **Signal** | Timestamped detector output with its inputs/version | Candidate pattern detected at 10:05 | Recommendation/order |
| **Prediction** | Estimate for a clearly defined future outcome and horizon | Target before invalidation within 5 sessions | Unqualified score |
| **Personal decision** | User's trade/no-trade/watch/reject choice with context snapshot | Entered, skipped, rejected | Market fact |
| **Outcome** | Result measured at a pre-defined horizon/rule | MFE/MAE, target/invalidation sequence | Retrospective opinion |

An episode may contain multiple events, observations, claims, hypotheses and securities. An observation may link to more than one episode. Links must be explicit; do not duplicate source facts to create separate narratives.

## 4. Epistemic status

Use these canonical evidence statuses:
- `SOURCE_CLAIM`: source says this; truth/achievement not independently established.
- `VERIFIED_FACT`: primary/source-backed fact checked for identity, date, unit and scope.
- `OBSERVATION`: direct observation without explanatory interpretation.
- `MEASUREMENT`: deterministic calculation from traceable inputs.
- `HYPOTHESIS`: possible explanation or relationship awaiting test.
- `INFERENCE`: result of a specified statistical/research method, with limitations.
- `UNKNOWN`: unavailable, stale, conflicting, ambiguous or insufficient.

Verification status is different from extraction confidence. A parser/LLM can be highly confident about what a PDF says while the underlying forward-looking statement remains an unverified claim about the future.

## 5. What a result must disclose

Every metric, pattern, signal, prediction or recommendation must show or link to:
- Source records and source timestamps.
- Data as-of time and publication/availability times where relevant.
- Universe and sample coverage.
- Required fields present/missing and known limitations.
- Formula, feature, detector, playbook or model version.
- Lookback, event window and outcome horizon.
- Sample size and independent episode count where applicable.
- Whether result is descriptive, inferential or calibrated prediction.
- Counterexamples, ambiguity and invalidation conditions.

`INSUFFICIENT_DATA`, `DESCRIPTIVE_ONLY`, `STALE`, `CONFLICTED`, `UNMAPPED` and `AMBIGUOUS` are valid results, not errors to hide.

## 6. Open-world discovery and analyst role

MBI must proactively surface relevant research leads outside saved watchlists through top-down catalyst-led and bottom-up anomaly-led discovery. It must state the exact universe and data coverage scanned; it cannot promise complete discovery of unknown opportunities. A candidate is not automatically a Market Episode, verified fact, probability or recommendation. See `17_OPEN_WORLD_OPPORTUNITY_DISCOVERY_AND_CONVICTION_SUPPORT.md` for the canonical candidate funnel and evidence brief.

The user remains the analyst. Keep source quality, measurement validity, market confirmation, historical evidence and the user's own conviction separate. MBI supplies evidence and counter-evidence; it must not manufacture a conviction score.

## 7. Independence and double-counting

A single market event can affect many tickers and trigger many detectors. Those ticker-signals are not automatically independent observations. Research must define the unit of analysis (event, episode, issuer-event, or trade) and cluster correlated observations before estimating sample size or confidence.
