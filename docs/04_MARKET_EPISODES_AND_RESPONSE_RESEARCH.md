# 04 — Market Episodes, External Context & Response Research

## 1. Market Episode is the main research unit

Open-world discovery may surface a candidate before a formal episode exists. The candidate-generation funnel is specified in Doc 17. This document defines the point at which a lead is scoped into an episode/study and how its chronology and market response are analyzed.

A Market Episode is a reviewable research container that links external context, issuer/market state, observations, hypotheses and subsequent outcomes over a defined time range. It allows MBI to learn from market behavior even when the user never traded it.

Episode origins:
- `EVENT_LED`: policy, macro, PUBEX, disclosure, result, corporate action, index or commodity event;
- `ANOMALY_LED`: unusual price, turnover, flow, orderbook or cluster behavior without a clear catalyst;
- `ISSUER_LED`: recurring behavior of one issuer around a lifecycle/condition;
- `CLUSTER_LED`: sector/conglomerate/theme rotation or leader/follower sequence;
- `USER_INCIDENT_LED`: something the user noticed while watching the market.

One episode can span several tickers and sources. If an event affects many related tickers, research must account for dependence; ticker count is not episode sample size.

## 2. Quick market incident capture

The user may notice something while watching a session that does not fit an existing classification. The first save must take seconds and must not force an explanation. Capture, where known:
- local capture timestamp (`Asia/Jakarta`) and separate observed-event timestamp;
- one or more tickers/security IDs, or mark market-wide;
- exact raw note in the user's words;
- directly observed fact vs interpretation vs question;
- screenshot/file/URL attachment and source;
- manually recorded price/volume/flow/orderbook values with units and timestamp;
- optional urgency/watch tag.

The raw observation is immutable. Later interpretations, source links and episode membership are appended. A queue disappearing can be recorded as an observation; "someone is absorbing" remains a hypothesis until independently operationalized and tested.

## 3. External context and issuer exposure mapping

Relevant context may include:
- global/Indonesia macro data and revisions;
- monetary/fiscal policy, APBN, regulation and government projects;
- commodity, FX, rates, trade and industry data;
- IDX disclosures, PUBEX, financial statements, RUPS and corporate actions;
- issuer management claims, contracts, customers/suppliers, capex and production disclosures;
- news reporting and clearly labelled public rumors.

Map a context to an issuer only through an evidence-backed exposure path, such as disclosed revenue sensitivity, production/input/output, asset, geography, contract, financing, ownership or regulatory applicability. A sector label alone is a weak proxy and must be marked as such.

## 4. Pre-event behavior and market reaction before public news

This is a first-class research question. MBI must compare:
- when unusual market activity was first observed;
- the earliest public timestamp found for a relevant information item;
- any known scheduled event or public precursor;
- rumor reports and their source/first-capture time;
- actual issuer/market reaction before, at and after public publication.

Possible classifications:
- `SCHEDULED_OR_EXPECTED_EVENT`;
- `PUBLIC_PRECURSOR_FOUND`;
- `PUBLIC_RUMOR_UNVERIFIED`;
- `PUBLIC_INFORMATION_FOUND_AFTER_ANOMALY`;
- `ANOMALY_NO_MATCH_IN_SEARCHED_SOURCES`;
- `DATA_OR_CAPTURE_ARTIFACT`;
- `TIMING_UNKNOWN`.

`PUBLIC_INFORMATION_FOUND_AFTER_ANOMALY` means MBI's captured source chronology shows the public item was published later than the anomaly timestamp. The **earliest public timestamp found is not necessarily the true first public availability**. It does **not** prove the information was unavailable elsewhere or that the anomaly was caused by leakage. `ANOMALY_NO_MATCH_IN_SEARCHED_SOURCES` must log which sources/queries were searched and when. It is not proof that no news existed.

Never attribute unexplained movement to a named holder, insider, manipulation or illegal conduct without independent verified evidence. Broker flow cannot identify the beneficial owner or holding period.

## 5. Market response measurements

For a defined event/episode, calculate where data permits:
- stock return and benchmark return;
- abnormal return under an explicit method (e.g. market-adjusted first; market-model only when enough valid history exists);
- turnover/relative-volume anomaly;
- range/realized volatility expansion;
- gap, intraday path and close location when source frequency permits;
- cluster breadth and leader/follower sequence;
- broker/foreign flow changes only when supplied;
- follow-through, reversal, time-to-reaction and persistence;
- MFE/MAE and target/invalidation sequence for a specified hypothetical setup.

Define event windows in trading sessions or clock time relative to a known publication/availability timestamp. Use trading calendars and market session boundaries. Event windows are configurable per event family; no one window is universal.

## 6. Research design and causal caution

Classify findings by strength:
1. **Temporal order:** anomaly happened before a found publication timestamp.
2. **Association:** similar events and responses co-occur in a defined sample.
3. **Predictive evidence:** the feature improves genuinely unseen outcome prediction versus a baseline.
4. **Causal evidence:** requires a defensible identification strategy, controls/assumptions and robustness checks; ordinary event chronology/correlation is not enough.

An event study alone does not prove causality. Consider confounders: simultaneous macro news, commodity shocks, sector rotation, market-wide stress, index rebalance, corporate action, suspension/resumption, prior trend, liquidity and overlapping disclosures.

Store alternative hypotheses and counterexamples. Do not select only episodes that support the original narrative.

## 7. Historical episode matching

Each study defines:
- episode inclusion/exclusion rules;
- independent unit of analysis;
- matching features and feature versions;
- lookback/market regime and exposure definition;
- event windows and outcome labels;
- handling of multiple securities per event;
- treatment of missing/conflicting data;
- train/validation/test or walk-forward period;
- multiple-testing / selection-bias controls.

Similar episodes are research analogues, not promises that the next episode will behave the same way.

## 8. Episode lifecycle

`CAPTURED → TRIAGED → EVIDENCE_LINKED → STUDY_DEFINED → ANALYZED → MONITORING → CLOSED / UNRESOLVED`

A candidate episode can remain unresolved indefinitely. New evidence appends a new review/version; it must not rewrite what was known at an earlier timestamp.
