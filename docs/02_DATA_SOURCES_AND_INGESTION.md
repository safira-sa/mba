# 02 — Data Sources, Acquisition & Fallbacks

## 1. Approved operating strategy

The product owner's current operating decision is:

- **Data/API budget: Rp0.** No paid provider, subscription, trial that converts to paid, or paid data entitlement is a dependency.
- **Broad-market acquisition: batch-first.** Prefer one official, user-triggered file/API batch that contains many securities for a declared date/range. Never require manual ticker-by-ticker market-data entry.
- **Intraday intelligence: limited.** Intraday capture is selected-issuer/event research only, not a whole-market scanner requirement.
- **Processing and analysis: automatic after intake.** Once an approved file/batch is placed in the local incoming area and the user starts import, MBI should parse, validate, normalize, calculate, update dossiers/coverage and run eligible research automatically.
- **Acquisition is not the same as processing.** The user may need to click an official download button or start an approved API batch. MBI must not claim unattended source acquisition where the source terms or route do not permit it.

The goal is **automated batch processing without per-ticker manual data entry**, not unrestricted crawling or an unlicensed live feed.

## 2. Acquisition modes

Every source registration must declare one acquisition mode:

1. `OFFICIAL_UI_BATCH_DOWNLOAD`: user-triggered download through a source's visible, permitted download function. One file may cover many issuers. No automated browser scraping/crawling.
2. `APPROVED_API_BATCH`: finite, documented API requests using the user's own approved credentials, within current terms and rate limits. Prefer explicit user-triggered batch runs and caching; no continuous polling or quota workarounds.
3. `DOCUMENT_BATCH_IMPORT`: one or more user-selected public documents copied/downloaded into the local incoming folder; MBI processes the entire batch.
4. `LOCAL_CAPTURE_SELECTED`: a selected-issuer observation or screenshot for limited intraday/flow research. Not a broad-market source.
5. `DEFERRED_OR_UNAVAILABLE`: no supported Rp0 route has been verified. Do not build a dependency on it.

The UI may provide `Import batch now` and/or process a local incoming folder on application start. A persistent background watcher is optional and not required for the first implementation; avoid a separate Windows service unless measured need justifies it.

## 3. Source strategy matrix

| Source | Intended use | Rp0 route under this strategy | Coverage expectation | Status / restriction | Fallback |
|---|---|---|---|---|---|
| IDX Ringkasan Saham | Broad EOD cross-section: price, volume, value, frequency and other displayed fields | User-triggered official `Unduh` action; import the resulting file as one batch | Potentially many/all rows shown by the selected page/filter/date; actual file row coverage must be measured | Download control and fields are visible; exported format, full-row inclusion, date selection and history depth remain unverified. IDX website terms prohibit web scraping/crawling. | If download does not provide a reusable batch, record the limitation; do not scrape pages or enter tickers manually. Seek another permitted public batch source or accept reduced coverage. |
| IDX issuer disclosure | Corporate events, ownership changes, affiliated/material transactions, exchange notices | User-triggered official document download or saved official document URL where permitted; multi-file local batch import | Selected documents, not automatic full archive | Public availability does not authorize crawling or bulk harvesting. Terms and access route must be checked. | Add URL/source citation and manually download the selected document; otherwise mark unavailable. |
| IDX Public Expose Live | Issuer presentation, profile, session materials and press release | User-triggered official document download; document batch import | Selected issuer/session documents | Do not assume transcript or downloadable deck for every issuer. No site crawler. | Record event/session page and capture available official material. |
| IDX financial statements / XBRL | Historical and point-in-time fundamental features | User-triggered official PDF/XLSX/XBRL download; batch import of selected periods/issuers | Selected filings initially; wider coverage only if official bulk download is proven | Format and availability vary by filing. No crawling. XBRL taxonomy/schema references must be preserved. | PDF/manual extraction with source spans, or mark concept unavailable. |
| IDX market data services | Licensed EOD/delayed/real-time and historical feeds | Reference only | Not part of Rp0 baseline | Treat as licensed unless explicit free entitlement is documented. | Exclude licensed fields; do not simulate them. |
| BI statistical publications | Monetary/macro series | User-triggered download of official tables/publications; batch import | Periodic macro series, varying by indicator | Publication cadence and revisions differ; machine-readable route must be verified per series. | Import official PDF/XLSX/table and preserve vintage. |
| BPS WebAPI | Structured macro/statistical observations | Optional finite API batch after application registration/approval, stored local token, terms/rate-limit review | Only requested datasets/endpoints; not an unrestricted scrape | WebAPI is described as free, but requires registration/credentials, applies rate limits and prohibits abusive automated harvesting/polling patterns. Use documented API requests only. | Official downloadable table/publication; otherwise mark series unavailable. |
| Kemenkeu / OJK / ESDM | Fiscal, financial regulation, energy/mineral policy and statistics | User-triggered official document/table downloads; document batch import | Selected relevant releases | No general crawler or assumption of structured APIs. | Archive source URL and selected original document. |
| Company investor relations | Issuer reports, presentations, operational updates | User-triggered selected document import | Selected issuers and documents | Issuer statements are claims; independently verify material facts where possible. | Cross-link official IDX disclosure or label issuer-reported. |
| Reputable news / public web | External context, chronology, reported catalyst | User/assistant research capture or specifically documented permitted feed, imported in batch | Captured/indexed sources only | No guaranteed free unlimited news/search API; no broad crawling. | Local full-text search over captured corpus and explicit searched-source record. |
| TradingView | Optional chart reference/export if plan supports it | **Not required.** User's current Basic plan cannot export chart data. | None in Gate -1 baseline | Do not request upgrade; do not make TradingView a dependency. | IDX EOD batch is the primary candidate for daily cross-section; other fields remain unavailable if absent. |
| Stockbit | Selected issuer Trade Flow/Broker Flow/Broker Summary/Orderbook observations | `LOCAL_CAPTURE_SELECTED` only where user feature entitlement allows | Selected tickers/timestamps, partial | No verified public bulk API or permission for full-market automated capture. | Keep as observation/manual evidence; never infer missing values. |
| BIONS | User's own transaction statements | **Deferred and not required.** Portfolio is manual-first. | None | No BIONS export or personal account data is required or requested. | User may manually enter positions/decisions into local MBI later. |
| DataCedar or other third-party free APIs | Optional selected-issuer research query only | Disabled by default; enable only after current terms/quota/coverage are checked | Selected queries, not broad archive replication | Not a broad-market source; no systematic archive replication, quota workarounds or assumption of permanent free tier. | Rely on approved IDX batch or mark field/history unavailable. |

## 4. Broad-market batch contract

The primary EOD research input is a **cross-sectional batch**, not per-ticker rows typed by the user.

For every accepted market batch, MBI must preserve:
- `source_id`, source route and terms-review version/status;
- `trading_date` as stated by source;
- `published_at` if stated and `retrieved_at` at local acquisition;
- original filename, SHA-256, file size, MIME/format and unmodified raw artifact;
- filter/board/market-segment settings, if available;
- total source rows, unique security codes, duplicate rows, rejected rows and accepted rows;
- actual fields/units found, missing/unknown fields and normalization map version;
- observed securities and coverage against any separately verified security master;
- whether the file appears to cover all rows, a page/filter, or an unknown subset.

Do not infer that a download covers every listed security just because the web page displays a large row count. Verify the actual file bytes, row count, symbols, filters and date. If that cannot be established, label the batch `CROSS_SECTIONAL_COVERAGE_UNVERIFIED`.

A daily cross-section can support daily issuer comparison for the fields present in the file. It does not provide multi-year history by itself. If historical daily files are unavailable at Rp0, MBI starts accumulating validated batches prospectively. Historical learning/backtests must show the available date range and cannot pretend a long archive exists.

## 5. Local batch processing contract

Recommended local-only locations (configurable):

```text
%LOCALAPPDATA%/MBI/
  incoming/
    idx_market_summary/
    issuer_documents/
    macro/
    selected_intraday/
    optional_platform_capture/
  raw_archive/
  normalized/
  database/
  reports/
  backups/
```

The project repository must not contain these directories or their personal/raw contents.

Processing sequence:
1. User downloads/copies one or more permitted files into the corresponding `incoming` folder.
2. User starts one batch import (or the app offers a transparent import-on-start option).
3. MBI archives original bytes and calculates SHA-256.
4. It detects format and parser candidate; unsupported formats remain archived.
5. It parses all rows/items in the batch to staging, preserving original row/page/sheet/cell coordinates.
6. It validates field names, units, date semantics, timezone, market segment, ticker identity, duplicates and coverage.
7. Valid rows become canonical records; ambiguous rows are quarantined for mapping/review.
8. MBI automatically recalculates affected issuer daily dossiers/features and queues discovery/learning runs that have sufficient inputs.
9. A batch report states accepted/rejected rows, securities, dates, coverage, errors and derived jobs completed.

The user must not enter each ticker or each market row manually. A manual correction is only for ambiguous mapping or genuinely unavailable source information, not ordinary batch maintenance.

## 6. Intraday policy — selected issuers only

Intraday is explicitly limited to selected candidates, active research leads, user incidents or specific Market Episodes.

- No all-issuer intraday scanner in the Rp0 baseline.
- No TradingView export dependency (current Basic plan cannot export).
- Stockbit is an optional capture/observation source subject to visible feature entitlement; its distinct flow panels and update timing must be recorded separately.
- A documented, permitted free selected-ticker API may be evaluated later, but it is not assumed or required.
- Intraday captures must store actual source timestamp, capture/retrieval time, timeframe, session, mode/filter and gaps.
- A screenshot/orderbook observation is not a continuous tick/queue history. EOD bars cannot reconstruct intraday sequence, broker flow or fill priority.
- Missing intraday data does not invalidate an EOD discovery candidate; it limits the follow-up research depth.

## 7. Deterministic automation boundary

Once a valid batch has been imported, the following should not require repeated manual entry:
- row parsing and normalization;
- issuer/security mapping when identity is verified;
- source and field coverage calculation;
- daily price/activity metrics present in the data;
- issuer/date dossier update;
- Stock Character features supported by the accumulated history;
- broad cross-sectional anomaly scan for available EOD fields;
- candidate creation and links to relevant indexed entities/documents;
- batch health/error reporting.

Event extraction from PDFs/news may create proposals and source spans; claims/ownership/exposure links that require interpretation remain reviewable. Deterministic calculations do not require Codex, Claude or an LLM at runtime. LLM use is optional for extraction/research language, not arithmetic.

## 8. Data tiers

- `OFFICIAL_STRUCTURED`: official source with known fields/units and an approved download/API route.
- `OFFICIAL_DOCUMENT`: official PDF/XLSX/XBRL/HTML document with parsing and review requirements.
- `PLATFORM_OBSERVATION`: optional TradingView/Stockbit screen or exported record; platform definitions and entitlements apply.
- `USER_RECORDED`: manual incident, trade decision or personal observation.
- `SECONDARY_REPORTING`: reputable reporting; not a substitute for primary disclosure.
- `UNVERIFIED_PUBLIC`: rumor/social post; hypothesis context only.

Authority is field-specific. Every derived record retains source lineage and source-time semantics.

## 9. Timestamps, vintages and identity

Preserve, where applicable:
- `event_at`, `published_at`, `first_observed_public_at`, `retrieved_at`;
- `effective_from` / `effective_to`;
- `period_start` / `period_end` and revision/vintage;
- source timezone and normalized `Asia/Jakarta` timestamps for IDX sessions;
- security identity and effective-dated ticker/issuer aliases.

Unknown timestamps remain null with a quality flag. Do not backdate publication to event date. Corrections/revisions create new source artifacts/vintages; never silently overwrite prior facts.

## 10. Fallback matrix

| Failure | Correct behavior | Forbidden behavior |
|---|---|---|
| Official download unavailable | report `SOURCE_DOWNLOAD_UNAVAILABLE`; keep prior valid data with stale date | silently switch to an unverified scraper |
| Download file omits some rows | calculate actual observed coverage; keep unseen securities unavailable | call it whole IDX coverage |
| Export format changes | quarantine and request mapping/sample update | guess columns by position |
| Unit unknown (shares/lots/IDR/thousand/million) | preserve raw value, set `UNIT_UNKNOWN` | assume a multiplier |
| History unavailable | begin prospective accumulation; disclose date range | fabricate prior bars or backfill with unrelated source without provenance |
| Selected intraday feed unavailable | retain EOD research; mark intraday unavailable | infer intraday sequencing from daily candles |
| Broker/orderbook data missing | mark unavailable | fill with zero or infer broker identity |
| PDF scan/unreadable | archive and mark review required | invent extracted text |
| Conflicting filing versions | retain both and link correction/supersession | overwrite earlier filing |
| No news found | log sources/query/time and label search scope | assert no public information existed |
| API quota/terms uncertain | disable adapter until reviewed | rotate keys/accounts or bypass quota |
| LLM unavailable | deterministic import/analytics remain usable | block core pipeline |
| Local data corruption | stop writes, restore verified backup and run integrity checks | silently rebuild/drop data |

## 11. Search capability boundary

At Rp0, baseline search is over the local imported/indexed corpus plus public research the user/assistant has explicitly captured through a permitted route. MBI has no guaranteed free unlimited internet/news search service. Discovery coverage must identify indexed sources, retrieval window and last index time.

## 12. Data-access approval gate

Before any automated API/download adapter is enabled, record exact route, terms/license, permitted purpose, account/registration requirement, rate limits, caching, fields, units, cadence, universe/date coverage, error behavior, last verified date and manual fallback. If any item is unknown, retain user-triggered batch import or disable the adapter. Website visibility alone is not permission for scraping/crawling or systematic harvesting.
