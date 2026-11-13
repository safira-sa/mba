# 18 — Setup Instances, Parameter Learning & Cross-Issuer Comparison

**Status:** Canonical requirement. This document defines how MBI learns from repeated occurrences of a gameplay/setup across many securities and contexts. It connects, but does not replace, Market Episode research, open-world discovery, Stock Character or Personal Edge.

## 1. Purpose

MBI must evaluate a gameplay not only from the user's executed trades, but from **all eligible historical and newly observed occurrences across the legitimately covered universe**. The same setup may appear on different issuers, dates, sectors, conglomerates, ownership structures, liquidity profiles and market regimes. Entry conditions may also differ.

The system must learn from a structured population of setup occurrences, not just a list of wins/losses or a handful of hand-picked charts.

Example user idea:

> Price is above MA10 and MA20, with two consecutive green candles.

That sentence is a **gameplay hypothesis**, not yet an executable rule. MBI must clarify timeframe, MA type, price field, candle completion, adjusted-price basis, and whether the condition is measured at signal time or entry time. Preserve the user's wording verbatim.

The learning objective is to investigate:
- how often the condition occurs across eligible issuers;
- what technical and fundamental context accompanies each occurrence;
- how outcomes differ by entry distance, stock character, market regime, sector/cluster and documented ownership context;
- which conditions appear robust across issuers and which are issuer-specific;
- how the user's selection and execution compare with the market-wide opportunity population;
- whether a parameter range is robust, rather than merely optimal in one historical sample.

## 2. Keep the concepts separate

1. **Gameplay hypothesis:** the user's or MBI's high-level idea, preserved in natural language and classified by origin (`USER`, `DATA_DISCOVERED`, `HYBRID`).
2. **Setup family:** a named family of conditions being studied across securities and time, e.g. `MA_RECLAIM_TWO_GREEN_CANDLES`. It groups related definitions and experiments; it is not a signal.
3. **Setup definition/version:** the exact, machine-testable conditions, features, parameters, timeframe and eligibility rules for one tested version. Unsupported/vague conditions remain non-runnable.
4. **Setup scan/evaluation:** a reproducible scan of an explicit universe and period against a setup version or experiment. It records eligible, evaluated, matched, excluded and unavailable coverage.
5. **Setup instance:** one occurrence of the setup for a specific security and timestamp under a specific definition/version. It exists whether or not the user noticed or traded it.
6. **Entry scenario and outcome:** one or more hypothetical entry/management assumptions evaluated separately from the setup occurrence and separately from actual user fills.
7. **Playbook:** the broader decision procedure that may use a setup version plus entry, exit, risk, sizing, exception and execution rules.

A setup is not automatically a playbook; a setup instance is not an actual trade; and a parameter experiment does not modify a production rule.

## 3. Worked example — Gameplay A (assistant-created illustration only)

**Important:** This example was drafted by the assistant to demonstrate the generic setup-learning mechanism. It is not the user's actual gameplay, is not approved as a strategy, and is not a Gate -1 input or blocker. The generic system must work without this example and must accept future user-originated or data-discovered setup families.

Illustrative draft only; values below explain the data model and are not recommended thresholds.

### A. Raw hypothesis
- Price above MA10 and MA20.
- Two consecutive green candles.
- Compare entry cases at different distances above MA10 and/or MA20, for example 1–2%.

### B. Required formalization
The setup definition must explicitly choose:
- bar timeframe (daily, 1-hour, 15-minute, etc.);
- MA type (`SMA`, `EMA` or another defined method), period, input field and warm-up;
- whether the latest **completed** close must be above both MAs;
- green-candle definition (e.g. completed `close > open`) and whether candles are consecutive on the same timeframe;
- raw or corporate-action-adjusted price basis;
- evaluation timestamp and when each condition becomes knowable;
- security types/boards/universe eligibility and exclusions;
- required data fields and missing-data behavior.

Do not silently assume daily candles, SMA, regular-market-only data or a particular price-adjustment basis. Those choices belong in the versioned definition.

### C. Continuous technical features
For every matched or research-eligible observation, preserve actual numeric values, not only pass/fail labels. Examples:

- `signal_close_vs_ma10_pct = (signal_close / MA10_at_signal_asof - 1) × 100`;
- `signal_close_vs_ma20_pct = (signal_close / MA20_at_signal_asof - 1) × 100`;
- `scenario_entry_vs_ma10_pct = (scenario_entry_price / MA10_known_at_entry_asof - 1) × 100`;
- `scenario_entry_vs_ma20_pct = (scenario_entry_price / MA20_known_at_entry_asof - 1) × 100`;
- optional `distance_atr = (reference_price - MA_n) / ATR_n`, with ATR definition/timeframe fixed;
- MA slope/separation, candle body/range/wick, gap, relative volume/turnover, turnover, frequency, range expansion and other versioned features when supported.

MA10 and MA20 distances are separate measurements. Do not replace them with one nearest-MA value unless a study explicitly defines that derived metric. Signal-time distance and entry-time distance are also separate: if entry occurs later, use only the MA value that was knowable immediately before the hypothetical entry, not a value calculated with future bars. Percentage distance and ATR-normalized distance answer different questions and must not be conflated.

### D. Entry variants are experiments, not edits to history
A parameter experiment may compare, for example:
- entry at signal-bar close, only if the close was executable after the signal became known;
- next-session open;
- first valid retest;
- entry when price is 0–1%, 1–2%, or 2–3% above MA10;
- separately defined bands relative to MA20;
- a maximum-extension rule or volatility-normalized entry band.

These are alternative hypothetical scenarios. Each needs a precise trigger, earliest executable timestamp, fill convention, transaction-cost/slippage assumptions and data-resolution requirement. Define whether the MA reference is frozen at signal time or recalculated using only completed data available at the entry timestamp; do not mix the two conventions. For an entry at the next open, the same-day close/MA is not available at that open. A daily candle touch is not proof that an intraday order could fill at that price. If chronology or fill is ambiguous, mark it ambiguous or use a coarser defensible outcome; do not invent a fill.

The user's actual fill is stored only in the personal execution ledger and is never overwritten by a hypothetical scenario.

## 4. Every eligible occurrence matters — not only traded cases

For a defined setup version/experiment, MBI should evaluate every eligible security-session (or eligible intraday observation) within the recorded universe and coverage. Separate **universe eligibility** from **feature availability**: an issuer may be eligible but unavailable because required trigger data is missing; optional fundamental, broker, UBO or orderbook fields being unavailable must not erase an otherwise valid technical setup instance. Cohort studies using an unavailable optional field must report the smaller data-complete subgroup. This creates:
- matched occurrences (`SETUP_MATCHED`);
- evaluated non-matches (`SETUP_NOT_MATCHED`);
- exclusions with explicit reasons;
- unavailable/unknown rows where required data is missing.

Do not store only winning examples. Negative cases, false starts, failed breakouts, muted responses and ambiguous cases are essential. The scan must distinguish an explicit setup non-match from a row that could not be evaluated because a required trigger feature was unavailable; neither may be counted as a normal negative outcome.

For storage efficiency, full scan evaluation matrices may live in immutable, versioned Parquet outputs, while SQLite stores scan metadata, coverage summaries, matched `setup_instance` records and important links. The logical model must retain enough information to reproduce the scan and its denominator. Do not silently discard non-matches if the study needs them.

A setup instance is identified by security identity, evaluation timestamp, setup family/version and scan/experiment context. A ticker string alone is not a stable identity. Consecutive/overlapping matches for the same security and setup family can represent one continuing opportunity rather than many independent samples. Preserve each timestamped match, but group related matches into a `setup_occurrence_group` using an explicit gap/re-arm/cooldown rule. Studies must state whether their unit is a raw instance or an occurrence group and must not inflate sample size with repeated adjacent matches.

## 5. Feature snapshot at each setup instance

Each setup instance links to an immutable as-of feature snapshot containing or referencing the available dimensions below. Reuse canonical observations and feature records; do not copy facts into every module.

### A. Technical / price-action context
- OHLC and timeframe with source/price basis;
- MA levels, distances, slopes and cross/stack conditions;
- completed candle sequence and shape;
- ATR, realized volatility, range, gaps and price-limit/suspension state;
- relative strength and benchmark definition;
- volume, turnover and frequency relative to comparable issuer baselines;
- VWAP, Trade Flow, Broker Flow, Broker Summary and orderbook only if actually available at the relevant timestamp.

### B. Fundamental / financial context
Use actual numeric features where filings and definitions support them. Possible fields include:
- market capitalization and dated free float;
- revenue/profit growth and margins;
- ROE/ROA;
- leverage, debt maturity and interest burden;
- operating cash flow, capex and free cash flow;
- valuation ratios such as P/E or P/B where denominator and period are valid;
- segment, commodity, geographic or project exposure;
- corporate actions and material financial changes.

Every value needs source, unit/currency, period, publication/vintage time, consolidated vs parent basis, formula version if derived, and quality status. A later-published financial result must not leak into an earlier setup snapshot. Guidance/PUBEX targets remain claims, not achieved financial facts. Fundamental data can be stale relative to a daily setup; show its age and reporting period rather than pretending it is daily data.

### C. Issuer / stock character
Link the versioned Daily Session Character and Rolling Stock Personality, including liquidity profile, volatility, trend persistence, mean-reversion tendency, breakout follow-through, failure rates, gap behavior, post-IPO age/cohort and data coverage where available.

### D. Market and external context
Link IHSG/market regime, sector and overlapping cluster states, macro/commodity/FX/rate context, event/episode chronology, news/disclosure availability and relevant market-wide incidents.

### E. Ownership / conglomerate / UBO context
Link dated, typed relationship edges and disclosed ownership/beneficial-owner facts. Preserve status (`VERIFIED`, `REPORTED`, `INFERRED`, `UNVERIFIED`, `DISPUTED`, etc.), source and effective/publication dates. Unknown ownership stays unknown. Group/UBO context is an analysis dimension, not proof of coordination or a sufficient causal explanation. One issuer may belong to multiple overlapping economic/project/ownership networks; do not force mutually exclusive conglomerate labels.

### F. User context (separate)
If the user observed or traded the instance, link the user's decision snapshot and actual execution separately. Do not mix user confidence, position size or execution into the market-wide setup feature vector.

### G. Separate trigger rules from learning context

Every feature used in a setup study must be assigned a role:
- `TRIGGER_FEATURE`: determines whether a setup version matches (e.g. close above MA10/MA20, two completed green candles).
- `CONTEXT_FEATURE`: describes the case and may explain differences in outcomes (e.g. volatility profile, liquidity, valuation, leverage, sector, market regime, conglomerate/UBO context).
- `OUTCOME_LABEL`: future result used only after the outcome horizon matures; never part of the decision-time predictor snapshot.
- `ENTRY_OR_EXECUTION_FEATURE`: describes a hypothetical entry scenario or an actual user execution; keep those populations separate.
- `IDENTITY_OR_COHORT_DESCRIPTOR`: stable/effective-dated issuer, sector, cluster, ownership or lifecycle membership used to form cohorts.

A context feature does not become a hard gate just because an exploratory analysis finds an association. Promoting it into a trigger requires a new hypothesis/setup version and independent validation. A missing trigger feature makes that security/time row unevaluable; a missing optional context feature leaves the setup match intact but makes that particular context comparison unavailable. Do not let thousands of arbitrary fundamentals enter a model without a declared research question, metric contract and multiple-testing plan.

### H. Illustrative cross-issuer comparison

Assume the same Gameplay A technical trigger appears in three fictional issuers:

| Case | Issuer character | Signal distance | Fundamental / ownership context | Why compare it |
|---|---|---|---|---|
| A | Established, liquid, lower realized volatility | MA10 +0.4%; MA20 +1.1% | Positive cash flow; disclosed controller link to Group North | More liquid, established profile |
| B | Newly listed, high volatility, limited free float | MA10 +1.7%; MA20 +0.6% | Short post-IPO history; UBO information unavailable | Lifecycle and volatility differences |
| C | Cyclical, thinner liquidity, larger daily range | MA10 +0.9%; MA20 +1.8% | Higher leverage; reported project relationship to Group South | Financing and project exposure differences |

These are fictional examples, not issuer facts or tested outcomes. MBI first confirms which cases actually match the same setup definition. It then compares their market outcomes and context. The unknown UBO in Case B does not mean there is no UBO or disqualify the technical setup. The reported relationship in Case C remains `REPORTED`, not verified. The group labels do not imply coordinated trading.

This is the intended comparison: one setup family, multiple issuers, different numeric features and contexts, followed by outcome analysis to see where the setup behaves similarly or differently.

## 6. Cross-issuer and cross-context comparison

A study may compare the same setup across:
- all eligible covered issuers;
- the same issuer's own historical instances;
- peers matched by liquidity/volatility/size/age;
- sectors and subsectors;
- documented conglomerate, ownership or project networks;
- market regimes and macro conditions;
- event-led vs anomaly-led episodes;
- post-IPO lifecycle cohorts;
- entry-distance and execution variants;
- fundamental cohorts such as profitability, leverage, valuation, growth or operating exposure where point-in-time data supports them.

These are overlapping analytical dimensions, not mutually exclusive labels. Report each cohort's definition, period, raw sample count, independent event count, missingness and coverage. Do not assume a conglomerate relationship implies identical behavior.

**Dependence warning:** ten issuers moving together after one policy event are not automatically ten independent event samples. Multiple setup instances can share the same date, catalyst, cluster or market shock. Repeated consecutive matches in one issuer can also be one continuing setup occurrence, not independent opportunities. Research must define its independent unit and use time/episode/cluster-aware validation or uncertainty estimates where appropriate. Show raw instance count separately from effective independent sample count where estimable.

When data is sparse for one issuer, MBI may show broader peer/sector evidence, clearly labelled as a fallback. It must not present pooled results as issuer-specific history. More granular models may partially pool across issuers only when methodology, cohort and uncertainty are disclosed.

## 7. Parameter and condition learning

A parameter experiment can test a pre-defined, justified set of alternatives for a setup family. Parameters may include MA periods/type, candle count, distance-to-MA bands, relative volume/turnover thresholds, ATR extension, liquidity bands, entry timing, exit rules and holding horizon.

Required procedure:
1. Record the research question, original hypothesis and planned parameter ranges before inspecting final holdout results.
2. Define eligible universe, date range, security/session rules, source coverage and exclusions.
3. Freeze feature definitions, data vintages, corporate-action basis and setup-version semantics.
4. Establish a simple baseline (e.g. broad eligible universe or an unfiltered version of the setup).
5. Evaluate a limited, justified set of parameter variants; record all tested variants, including failures.
6. Separate training/development from time-based validation and untouched holdout or walk-forward periods. Prevent overlapping outcome windows from leaking across splits (purge/embargo when needed).
7. Control multiple testing and selection bias. Do not report only the best-performing combination.
8. Begin with predeclared, interpretable one-dimensional sensitivity checks; test interactions (e.g. entry distance × liquidity or regime × issuer character) only when justified and sample coverage supports them. Compare outcomes by issuer character, liquidity, sector/cluster, market regime, event context, fundamental profile and ownership network only when samples support it.
9. Prefer stable **performance regions/plateaus** across nearby parameter values and periods over a sharp isolated optimum.
10. Retain counterexamples, transaction costs, slippage, turnover capacity, drawdown and failure modes.
11. Publish the experiment as research evidence. Only an explicitly approved, tested playbook version may become active.

A result can be `DESCRIPTIVE_ONLY`, `EXPLORATORY`, `OUT_OF_SAMPLE_SUPPORTED`, `FRAGILE`, `REJECTED` or `INSUFFICIENT_DATA`. Do not call a setting "optimal" without specifying objective, constraints, comparison set, validation and robustness. There is no universally optimal MA/entry distance/holding period.

## 8. Market outcome vs personal outcome

For every market-wide setup instance, measure market-path outcomes independently of the user: returns/abnormal returns, MFE/MAE from declared reference points, follow-through, failure, time-to-event and horizon returns when data permits.

For the user, separately measure:
- whether the instance was noticed/selected/rejected/missed;
- actual entry distance and delay from signal;
- fills, partial fills and fees if known;
- actual exit/management and realized result;
- adherence to the intended playbook;
- risk and portfolio exposure.

This enables separate diagnosis of opportunity availability, setup quality, user selection, entry/exit execution and risk management. A market setup can work while a user's execution fails; an individual trade can win even if the broader setup has weak expectancy.

## 9. Probability and recommendation safeguards

Before displaying a probability, define exact outcome/horizon, cohort, independent sample unit, base rate, feature availability at decision time, time-based out-of-sample testing, calibration, uncertainty interval and applicability limits. Effective sample cannot be inflated by counting highly correlated issuers/instances as independent.

Otherwise display counts, distributions, quantiles, hit/failure rates with caveats, confidence intervals where justified, or `DESCRIPTIVE_ONLY` / `INSUFFICIENT_DATA`. Similarity, parameter score, user conviction and research priority are not probabilities.

## 10. Acceptance criteria

1. A user can later describe any gameplay in ordinary language and preserve the raw note. The illustrative Gameplay A fixture is not treated as user-provided content.
2. The system asks for or clearly marks undefined timeframe, MA type, candle completion, price basis and trigger timing; it does not invent them.
3. A setup version stores all exact numeric/boolean conditions and feature semantics.
4. A scan covers a declared security universe/time range and reports eligible, scanned, matched, non-matched, excluded and unavailable counts.
5. MBI can create setup instances across multiple issuers without requiring a user trade.
6. Each instance links to point-in-time technical, fundamental, issuer-character, market, cluster and ownership context where available.
7. MA10 and MA20 distances remain separate; entry distance variants are reproducible and separately evaluated.
8. Hypothetical entry scenarios never become actual fills or portfolio ledger events.
9. Positive, negative, ambiguous and no-fill cases remain in the study.
10. Cross-issuer studies report cohort definition, correlated event dependence and effective sample limitations.
11. Parameter research records every attempted variant and passes time-based out-of-sample/walk-forward checks before stronger claims.
12. The report can identify robust parameter regions or explicitly say no robust region was found.
13. Personal Edge links only the user's actual decisions/executions and keeps market-wide instances separate.
14. No automatic production playbook change occurs without review and approval.

## 11. Boundaries with adjacent documents

- Doc 03 owns issuer daily features and Stock Character definitions.
- Doc 04 owns Market Episode and response study construction.
- Doc 05 owns pattern, detector, gameplay, setup/playbook lifecycle at the conceptual level.
- This document owns cross-issuer setup occurrences, feature snapshots, parameter experiments and their evaluation.
- Doc 06 owns the user's decisions and Personal Edge.
- Doc 09 owns metric/statistical contracts and QA rules.
- Doc 14 owns logical entities and relationships.
- Doc 17 owns open-world candidate discovery; it does not own setup-instance evaluation.
