# ADR-003 — Setup Instances & Cross-Issuer Parameter Learning

- **Status:** Proposed; requires user approval before implementation.
- **Date:** 13 November 2026.
- **Depends on:** ADR-001 (Market Episode as core), ADR-002 (Open-World Discovery).

## Context

The user wants a gameplay such as "price above MA10 and MA20 with two green candles" evaluated across many securities and historical occurrences, not only cases the user traded. Cases differ by issuer character, fundamental state, liquidity, sector, conglomerate/UBO links, market regime and entry distance. Entry scenarios may vary (e.g. price 1–2% above MA10 or MA20). The system must compare conditions and parameter ranges without hindsight, invented fills or overfitting.

## Decision

Add **Setup Family**, immutable **Setup Definition Version**, **Setup Scan**, **Setup Instance**, **Entry Scenario**, **Parameter Experiment/Variant** and **Setup Outcome** as distinct logical concepts.

- A Market Episode is a context/event/anomaly research container.
- A Setup Instance is one security/time occurrence of a versioned setup condition.
- A user Trade Decision and actual Execution Fill are separate personal records.
- Market-wide setup outcomes must be evaluated even when the user did not notice or trade the instance.
- Each setup instance links to a point-in-time feature snapshot; it does not duplicate canonical technical/fundamental/ownership facts.
- Parameter experiments are research-only. Only an explicitly reviewed new version can become active.

## Consequences

- The logical model and roadmap require setup scan/coverage, setup occurrence, scenario and experiment concepts.
- A setup scan must preserve denominator and negative cases across the declared eligible universe.
- Technical, fundamental, issuer-character, macro/market, sector/cluster and dated ownership/UBO context become conditional analysis dimensions.
- Cross-issuer cases sharing event/date/cluster/network require dependence-aware evaluation.
- Robust parameter plateaus are preferred over isolated in-sample optima.
- No physical schema or SQL migration is approved by this ADR.

## Rejected alternatives

1. **Learn only from executed trades:** creates selection bias and misses untraded market opportunities.
2. **Treat every ticker row as an independent sample:** inflates evidence when issuers share events, dates or clusters.
3. **Optimize one best threshold:** encourages multiple-testing and backtest overfitting.
4. **Treat hypothetical entries as actual fills:** corrupts personal portfolio and execution records.
