# 03 — Issuer Daily Dossier & Stock Character

**Acquisition scope:** The baseline daily layer is populated from source-verified cross-sectional batches. One accepted batch should update every valid issuer row it contains; the user does not enter daily price/activity data ticker by ticker. The actual file rows and verified source universe determine coverage. Intraday layers are selected-issuer/event-only and optional.

## 1. Exact meaning of daily data per issuer

For each **covered security × IDX trading date**, MBI builds an Issuer Daily Dossier. It is a collection of independently sourced layers, not one flattened score. A dossier may be partial; coverage must be visible.

The **eligible universe**, **observed universe** and **covered universe** are maintained separately. A ticker appearing in an imported file is not proof that all IDX issuers were scanned. Daily full-universe coverage is possible only on dates and fields for which an actual broad-market source file is available and permitted. Show a coverage ratio and list excluded/unresolved securities. Historical universe membership must be effective-dated and include delisted/suspended securities where records exist; otherwise disclose survivorship limitations.

## 2. Dossier layers

### A. Security identity and status
- stable internal `security_id`;
- ticker and effective-dated ticker aliases;
- security type (ordinary share, warrant, rights, ETF, etc.);
- listed/delisted, suspension/resumption and corporate-action status;
- market segment/board and trading calendar;
- listing date and actual history coverage.

Never join historical data by ticker text alone when tickers can change or be reused. Keep security identity separate from issuer/legal entity identity.

### B. Price and chart behavior
Where source data exists:
- OHLC, previous close, return, gap, range and candle body/wicks;
- raw vs adjusted price basis;
- trend/range/compression/expansion descriptors;
- moving averages, ATR, RSI/MACD only with fixed, versioned definitions;
- relative strength to IHSG/sector/peer benchmark with benchmark definition;
- gap, close location, intraday path only at the source's real timeframe.

EOD OHLC cannot reveal the order of intraday high/low, stop/target touches, or minute-by-minute behavior.

### C. Activity and liquidity
Keep source fields separate:
- volume and native unit (shares/lots/unknown);
- turnover/value traded and currency;
- frequency/number of transactions only as source defines it;
- transaction count, if explicitly supplied;
- median turnover/value and relative volume/turnover;
- free-float turnover ratio only with valid dated free-float denominator;
- spread/depth only from time-stamped orderbook snapshots.

Do not calculate exact traded value as `close × volume` unless the source's quantity and value definitions justify it. If used as a proxy, label it `ESTIMATED_PROXY` and never mix it with actual turnover.

### D. Transaction and order-flow layers
Keep separate:
- Trade Flow / aggressor-like summary, with source's own definition;
- Broker Flow, including volume/value mode, date range, selected broker filters and update time;
- Broker Summary, including gross/net mode and market segment;
- Foreign buy/sell/net only when explicitly provided;
- running trades only when actual prints are available;
- orderbook depth snapshots, timestamp and level count.

Broker code aggregates are not beneficial-owner identities or evidence of holding period. Orderbook queues are not executed transactions. A missing field is not zero.

### E. Issuer/ownership/document context
- disclosures, financial results, PUBEX and annual reports;
- controller/beneficial-owner disclosures with source/date/effective period;
- public shareholding and pledge/collateral disclosures;
- corporate actions, RUPS, related-party transactions and project/customer/supplier statements;
- management claims and guidance separated from achieved results.

A disclosed share pledge is not proof of default/collateral enforcement. A corporate relationship is not proof of coordinated trading.

### F. External and market context
- IHSG return, breadth, turnover and volatility where available;
- sector/cluster behavior and benchmark membership version;
- macro variables, commodity, FX, rates and policy events;
- event calendar and information available at the dossier's as-of time.

## 3. Fundamental and ownership context for cross-issuer learning

The dossier may link point-in-time fundamental features and dated entity/ownership relationships for research cohorts. These are not necessarily daily observations. Each carries its own reporting period, publication/vintage timestamp, effective period, source, unit, formula version and staleness/quality status. A setup or historical replay may use only values publicly available as of its decision timestamp.

Fundamental and ownership context should be available as independent analytical dimensions; do not collapse them into an opaque issuer score. Missing UBO/controller information is unknown, not evidence of no relationship. Relationship graphs may overlap and may include separate ownership, project, supplier/customer, financing and thematic links.

## 4. Stock Character has two distinct levels

### Daily Session Character
Describes the current session using only available data, e.g.:
- directional trend / range / reversal / gap-and-go / gap-fade;
- compression or range expansion;
- close near high/low or mid-range;
- relative turnover/volume abnormality;
- intraday VWAP relation only if intraday bars/transactions support it;
- observed flow/orderbook changes only when captured.

This is a descriptive state, not a permanent label or buy/sell signal. Do not collapse it into one opaque bullish/bearish character score; preserve separate axes and show which axes are unavailable.

### Rolling Stock Personality
Describes historical behavior over explicit rolling windows:
- realized volatility and range distribution;
- turnover/frequency/liquidity profile;
- trend persistence vs mean-reversion tendency;
- breakout follow-through and failed-breakout frequency;
- pullback depth and recovery distribution;
- gap behavior and price-limit/suspension history;
- event-response profile;
- sensitivity/correlation to market, sector, commodities or macro variables;
- leader/follower behavior within documented clusters.

Every personality attribute must show its lookback, sample size, exclusions, stability and source coverage. Character may differ by regime; avoid a single timeless label. A personality profile is multi-dimensional, not one opaque scalar. If evidence changes materially, show the profile/version transition instead of overwriting the old profile.

## 5. Dossier coverage contract

Each layer has its own status:
- `AVAILABLE_VERIFIED`
- `AVAILABLE_SOURCE_LIMITED`
- `MANUAL_OBSERVATION`
- `STALE`
- `PARTIAL`
- `NEEDS_MAPPING`
- `CONFLICTED`
- `UNAVAILABLE`

Show layer-level as-of timestamp and source. Overall dossier coverage is a summary, not a substitute for seeing missing components. A price chart may be complete while broker data is unavailable.

## 6. Comparable baselines

Daily relative metrics compare an issuer with its own previous completed valid sessions and a defined lookback. Intraday relative metrics compare like-for-like time buckets across valid prior sessions. Do not compare a partial current session with full-day history. Handle suspensions, zero-trade sessions, corporate actions and missing bars explicitly.

## 7. Relationship to market episodes

A dossier can be linked to zero, one or many Market Episodes. Its daily observations are reusable evidence. Do not copy them into each episode as new independent measurements.


## 8. Relationship to setup learning

Setup Instances reuse this dossier's versioned technical, activity, issuer-character, fundamental, ownership and external-context features through immutable feature snapshots. They must not copy or recalculate these features independently. See Doc 18 for cross-issuer setup and parameter learning.
