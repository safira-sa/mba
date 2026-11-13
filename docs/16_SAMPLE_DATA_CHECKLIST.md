# 16 — Gate -1 Sample Data Checklist (Zero-Budget Batch-First)

## 1. Purpose

Samples are only to prove source/file semantics and parser feasibility before building importers. They are not the production acquisition method and do not require one file per issuer. Prefer one cross-sectional file containing many securities for a trading date.

**User operating decisions:** Rp0 data budget; broad-market acquisition via user-triggered batch downloads; intraday only for selected issuers. No BIONS exports, personal portfolio files, paid TradingView upgrade or per-ticker market-data entry is required.

## 2. Primary sample: IDX Ringkasan Saham batch

First investigate the official page:
https://www.idx.co.id/id/data-pasar/ringkasan-perdagangan/ringkasan-saham/

The page currently exposes an `Unduh` control and displays daily fields such as ticker, name, previous/open/high/low/close/change, volume/value/frequency, bid/offer, listed/tradeable shares, foreign buy/sell and non-regular activity where shown. This is only route/field discovery. The actual export is not yet verified.

Obtain **one user-triggered file through the visible official download function**, if available and permitted. This should be one batch for a date/filter, not one download per ticker. Record:
- exact date/filter/board/market selection;
- original filename, file format, size and SHA-256;
- source URL and access date;
- total rows and unique securities;
- duplicate/blank/zero/no-trade rows;
- actual columns and raw units;
- whether rows cover the selected result set or only a page/subset;
- whether historical date selection exists and what dates are actually downloadable;
- terms evidence and any stated restrictions.

If no download file is available, do not scrape/crawl or reconstruct the table manually. Record `DOWNLOAD_ROUTE_NOT_VERIFIED` and ask the user for one browser-downloaded file only if the official UI makes it possible. If the route is unavailable or incomplete, research another permitted broad-market batch source or explicitly accept a reduced universe.

## 3. Public document batch

Use selected public, non-personal documents only:
- one IDX issuer disclosure (e.g. ownership change, affiliation/material transaction or exchange request response);
- one PUBEX presentation or press release, if downloadable;
- one financial statement PDF and matching XLSX if both are publicly available; XBRL package only if provided for that same filing;
- one BI statistical table/publication;
- one BPS WebAPI sample response only if a compliant registered application is available; otherwise use an official published table/file and leave API access conditional.

These samples test provenance, page/slide/sheet/cell locations, publication/effective/reporting-period semantics, units, revisions and parsing limitations. Do not fetch entire archives.

## 4. Intraday sample — optional, selected issuer only

Intraday is not broad-market scope and is not a Gate -1 blocker. If a lawful zero-cost sample is already available, one selected-issuer capture/file may be used to test timestamp/timeframe semantics. Stockbit screenshots/observations are optional and non-personal. Do not require TradingView export because the user's current Basic plan cannot export chart data. Do not upgrade the plan.

If no permitted sample exists, document `INTRADAY_SAMPLE_NOT_AVAILABLE` and defer intraday parser/analytics. Daily EOD functionality must still be independently testable.

## 5. Explicitly excluded samples

- No BIONS History Trade, Client Activity, History Cash or other broker statement.
- No personal portfolio database, account identifiers, balances or transaction history.
- No TradingView CSV requirement: the user is on TradingView Basic and chart export is unavailable. Do not request a paid plan upgrade.
- No Gameplay A user definition: Gameplay A in the examples is assistant-created and illustrative only, not the user's actual strategy and not a Gate -1 blocker.
- No paid provider files, trial accounts or licensed datasets under the current Rp0 decision.

## 6. Synthetic fixtures

Synthetic fixtures may test parser-independent calculations, edge cases, missingness, deduplication and validation logic. Label every synthetic row/file `SYNTHETIC_TEST_ONLY`. Synthetic fixtures never establish an external provider's actual file format, source permission, coverage or market-statistical validity.

## 7. Provenance manifest for each real sample

Record:
- sample ID and source ID;
- source URL / visible official route;
- terms URL, terms review date and allowed acquisition mode;
- publication/trading/reporting date where applicable;
- retrieval date/time and timezone;
- original file hash, size, MIME/format;
- selected filters, universe/coverage, row count and unique security count;
- raw field names, units, date/time semantics and missing-value conventions;
- known corrections/revisions and whether sample is representative;
- reviewer sign-off and unresolved questions.

Do not modify the raw original. Store normalized test copies separately.

## 8. User effort boundary

The user is not expected to collect all documents or all tickers manually. Codex/research may identify publicly available official sample routes and prepare an exact one-batch request. The only likely manual market-data action is a single official IDX Ringkasan Saham download if the visible download route is usable. If any user file is needed, request it explicitly with a reason and exact instructions. Never request personal BIONS or portfolio data.

## 9. Gate -1 acceptance checklist

Gate -1 can close when:
1. The broad-market batch route is either sample-verified or explicitly rejected with a documented fallback/coverage limitation.
2. The actual batch file's format, fields, units, date and row/security coverage have been inspected; no assumed full-market coverage remains.
3. At least one official issuer document and one official macro sample/route have provenance and parsing feasibility assessed. If inaccessible, the affected capability is explicitly deferred rather than represented as available.
4. Current access terms and acquisition mode are recorded per source; IDX website crawling/scraping is not used; BPS API use remains conditional on registration/terms/rate limits.
5. The broad-vs-detailed universe, history availability and initial coverage policy are explicit.
6. Intraday is either supported by one permitted selected-issuer sample or explicitly deferred; it does not block daily EOD.
7. Gameplay A is labelled illustrative only and does not appear as a user decision or Gate -1 blocker.
8. No physical schema/parser/backtest has been designed from guessed external fields.
