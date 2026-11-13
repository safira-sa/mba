# ADR-004 — Zero-Budget, Batch-First Broad-Market Acquisition

- **Status:** Accepted product decision; source routes remain subject to Gate -1 evidence.
- **Date:** 13 November 2026
- **Decision owner:** Product owner

## Context

The product requires open-world discovery beyond the user's watchlist, but the initial data/API budget is Rp0. TradingView Basic cannot export chart data for this user; BIONS personal statements are not required and personal portfolio data must not be exposed to development agents. IDX website terms prohibit web scraping/crawling. Full-market intraday and broker-flow coverage is not established at zero cost.

## Decision

1. Use a **batch-first broad-market acquisition** strategy. Prefer one permitted, official cross-sectional file/API batch containing many securities for a date/range. No per-ticker manual market-data entry.
2. The primary EOD candidate is the official IDX Ringkasan Saham `Unduh` route. Its actual file format, row coverage, filters, historical date availability and terms must be verified before it is accepted as a source.
3. After a batch is placed in the local incoming folder and the user starts import, MBI should automatically archive, parse, validate, normalize, calculate affected features, update coverage and queue discovery/research.
4. No IDX website scraping/crawling or restricted/private endpoint access. If an approved API batch is used (e.g. BPS WebAPI), it must be documented, registered where required, rate-limited, cached and compliant with current terms.
5. Intraday analysis is limited to selected issuers/events and only uses data actually available from a permitted source. It is not a whole-market scanner requirement.
6. TradingView export, BIONS import, paid providers and paid subscriptions are not Gate -1 or MVP dependencies. Portfolio inputs are manual-first and local-only.
7. If broad-market historic EOD files cannot be obtained at Rp0 through an approved batch route, MBI may accumulate history prospectively. Historical research must display the actual available date range and remain unavailable until the sample/coverage requirement is met.

## Consequences

- The product can automate data processing and analysis without promising unattended acquisition from every source.
- A daily broad-market file may enable cross-sectional discovery, but not continuous intraday surveillance.
- Open-world discovery is limited to the imported/captured source corpus and actual security/field coverage.
- Source acquisition and data processing are separate product capabilities and must be reported separately.
- Paid or licensed feeds may be considered only through a future explicit product-owner decision; they are not included in the current budget.

## Rejected alternatives

- Scraping IDX pages to build a full archive: rejected due to published terms prohibiting scraping/crawling.
- Using TradingView Basic as the required OHLCV export source: rejected because the user's current plan cannot export.
- Requiring BIONS/account statements: rejected for privacy and because personal edge can start with manual journal/portfolio entries.
- Whole-market intraday/broker/orderbook capture at Rp0: rejected as unsupported by verified source evidence.
- Manual ticker-by-ticker market-data entry: rejected as contrary to product requirements.

## Revisit triggers

Revisit this decision only if: source terms/routes change; a new official/free batch feed is verified; broad EOD coverage proves insufficient for core discovery; or the product owner explicitly changes the data budget.
