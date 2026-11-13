# 09 — Metric Contracts, Statistical Validation & No-Hallucination QA

## 1. Metric contract required for every numeric feature

Each metric needs:
- stable `metric_id` and semantic version;
- business definition and what it does **not** mean;
- formula or algorithm;
- input fields and source types;
- unit, currency, timezone and market segment;
- price adjustment basis;
- timeframe/lookback/warm-up;
- eligibility, exclusions and sample rules;
- missing/zero/invalid behavior;
- rounding/precision;
- output quality flags;
- tests with independently hand-checked fixtures.

A formula without known input semantics is not ready to implement.

## 2. Example definitions (illustrative contracts)

- **Simple daily return:** `close_t / previous_close - 1`, only where both prices share the selected raw/adjusted basis and no unresolved corporate-action discontinuity exists.
- **Daily range:** `high - low` in price units. If high/low missing or invalid, return unknown.
- **Close location:** `(close - low) / (high - low)`. Undefined if `high == low`; clamp only if source values are validated, otherwise flag invalid OHLC.
- **True Range:** `max(high-low, abs(high-prev_close), abs(low-prev_close))`, with consistent price basis and valid previous close.
- **Daily relative volume:** current completed-session volume divided by the median of prior eligible completed sessions over a declared lookback. Do not include the current session in its own baseline.
- **Intraday relative volume:** current cumulative volume at a given session time bucket divided by a statistic of prior sessions at the same comparable time bucket. Requires intraday history and consistent session/timezone.
- **Relative turnover:** same approach as relative volume but uses actual source-reported traded value. Never substitute estimated `close × volume` without labelling it a proxy.
- **Market-adjusted return:** issuer return minus benchmark return over the same window. Descriptive benchmark, not causal effect. A beta-adjusted model requires enough history and separately versioned assumptions.
- **Orderbook imbalance:** `(bid_depth - ask_depth) / (bid_depth + ask_depth)` for a declared price band/levels and exact snapshot time. Undefined when denominator is zero. Snapshot only; not executed flow.
- **VWAP:** use actual transaction price × quantity / total quantity when trade-level data exists. If derived from bars using typical price × volume, label `BAR_ESTIMATED_VWAP`, not exchange VWAP.
- **MFE/MAE:** direction-aware maximum favorable/adverse excursion over a defined horizon, reference entry and available price resolution. Ambiguous path ordering must be flagged.

These are starting definitions, not permission to calculate when source units/semantics are unknown.

## 3. Technical setup features and parameter semantics

For setup-learning metrics, exact feature semantics are part of the metric contract:
- moving-average method (`SMA`, `EMA`, etc.), period, input price field, timeframe, completed-bar rule, warm-up and adjustment basis;
- candle definition (e.g. completed `close > open`), sequence count and bar timeframe;
- `distance_pct = (reference_price / MA_n - 1) × 100`, calculated separately for each MA;
- optional `distance_atr = (reference_price - MA_n) / ATR_n`, with ATR definition/timeframe fixed;
- entry reference and signal/as-of timestamp separated from later hypothetical fill price;
- parameter interval endpoints and inclusivity explicitly specified.

Never collapse MA10 and MA20 distance into one value without a named derived metric. Keep signal-time distance distinct from entry-time distance, using only information available at each timestamp. A 1–2% distance and an ATR-normalized distance are not interchangeable. An incomplete candle cannot satisfy a completed-candle rule. If setup requires `close > MA10` and `close > MA20`, both comparisons must be separately testable.

## 4. Point-in-time fundamental and relationship features

Fundamental features used in a historical setup snapshot must be the latest values publicly available at that setup's `input_as_of` timestamp, not simply the latest values in today's database. Store reporting period, publication/vintage time, revision/supersession, units/currency, consolidated/parent context, source locator and formula version. Derived ratios require compatible numerator/denominator periods and definitions. Distinguish stock variables (e.g. balance-sheet values at a date) from flow variables (e.g. quarterly/YTD revenue/cash flow).

Ownership, conglomerate and UBO context must be effective-dated and source-backed. Unknown, reported, inferred, disputed and verified statuses remain distinct. Do not retroactively apply a relationship that was not publicly known/effective at the historical decision time. Group membership is context, not evidence of coordinated trading or causality by itself.

A setup scan distinguishes an issuer eligible for the universe but unevaluable due to missing required trigger features from an evaluated non-match. Missing optional context (e.g. UBO, fundamentals, broker flow) must not erase a setup; it only limits the corresponding subgroup analysis.

Fundamental data are often quarterly/annual and stale relative to a daily setup. Carry forward only as the latest known published fact with explicit age/reporting period; never fabricate daily fundamental observations or treat stale data as current without a flag.

## 5. Units and normalization

Explicitly distinguish:
- shares vs lots vs contracts;
- IDR vs thousands/millions of IDR vs USD;
- gross vs net broker values;
- volume vs value;
- transaction frequency vs count of individual trades;
- regular vs negotiated/cash market segments;
- raw vs adjusted prices;
- instant vs duration financial facts;
- consolidated vs parent-only financials.

Do not infer unit from magnitude. If unknown, preserve the original raw field and set `UNIT_UNKNOWN`.

## 6. Calculation and data quality behavior

- Missing value → null/unknown with reason, never zero.
- Invalid denominator → undefined + quality flag.
- Stale data → show as stale and block time-sensitive conclusions.
- Conflicting sources → preserve both; do not average/choose silently.
- Insufficient lookback → no value or explicitly partial value; do not shorten the required sample invisibly.
- Corporate-action discontinuity → use verified adjustment or exclude/flag the window.
- Suspended/no-trade session → distinguish from zero volume where market data supports it.
- Unresolved ticker/security mapping → quarantine rather than join.
- Daily bars cannot resolve intraday target/stop order. If both are touched in the same bar, `AMBIGUOUS_INTRABAR` unless finer data resolves chronology.

## 7. Financial statement/XBRL validation

Retain exact concept/QName, taxonomy version, unit, context, dimensions, instant/duration period, consolidated/parent basis, decimals/precision, reporting period, filing publication time and source locator. Do not combine facts with different contexts or dimensions. Distinguish YTD cumulative from discrete quarter; derive discrete values only with documented, tested calculations. Keep revised filings as new versions and link supersession; do not overwrite.

Management guidance and PUBEX statements are claims/targets, not achieved facts. To track outcomes, link later disclosed actuals as separate facts and compare period/definition/unit.

## 8. Event, setup and model validation

For event/episode and setup-instance research:
- define unit of analysis: event episode, setup instance, issuer-session or user decision; do not interchange them;
- record eligible, scanned, matched, non-matched, excluded and unavailable denominators for each setup scan;
- define publication/availability cutoff and windows;
- prevent future information leakage;
- account for overlapping events and correlated issuers;
- compare against relevant baselines;
- retain negative/counterexample results;
- correct for multiple testing/selection where material;
- account for dependence among setup instances sharing issuer, market day, cluster, ownership network or external event;
- group consecutive matches into opportunity spells when appropriate so a continuing setup is not counted as many independent samples;
- use time-based holdout/walk-forward rather than only random splits;
- preserve model version and predictions before outcomes mature.

For probability outputs:
- exact event and horizon;
- eligible sample and class balance/base rate;
- out-of-sample calibration;
- uncertainty interval;
- baseline comparison;
- current applicability and coverage.

Without these, show descriptive statistics only. Do not output probabilities from an LLM narrative. Parameter experiments must record all attempted variants, use time-based holdout/walk-forward validation, avoid overlapping-label leakage, compare against a baseline and prefer stable parameter regions over isolated best results.

## 9. Personal learning QA

Separate opportunity selection, setup quality, execution quality, risk behavior and outcome. Actual trades, no-trades, rejected ideas and hypothetical missed opportunities are separate populations. Do not use future chart data in the decision-time snapshot. Do not infer fill prices from candles.

## 10. Discovery and evidence-brief validation

- A discovery priority/sort dimension must have a transparent definition and cannot be labelled expected return, probability or conviction.
- Keep source authority, exposure-path verification, measurement quality, market confirmation, historical comparability and user playbook fit separate.
- Unknown dimensions remain unknown; do not coerce them to zero in a weighted score.
- Discovery coverage denominators (intended, eligible, scanned, excluded) must be reproducible for each run.
- Candidate usefulness metrics must distinguish user interest from objective outcome quality; user acceptance/rejection is not a truth label.
- Candidate briefs must contain both supporting and contradicting evidence, alternatives, unknowns and source links.

## 11. Formula approval checklist

No metric enters production until:
- definition and intended use approved;
- field units/semantics confirmed by real sample or authoritative spec;
- edge cases listed;
- hand-calculated fixture exists;
- unit/integration tests pass;
- formula version and migration impact recorded;
- UI labels distinguish actual/estimated values.
