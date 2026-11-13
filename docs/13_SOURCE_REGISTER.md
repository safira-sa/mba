# 13 — Source Register & Feasibility Status

**Decision baseline:** 13 November 2026. **Operating mode:** Rp0, batch-first broad EOD acquisition, limited selected-issuer intraday. These notes are evidence/status records, not a legal opinion or guarantee that a route remains unchanged. Recheck terms immediately before enabling an adapter.

Status meanings:
- `ROUTE_OBSERVED`: visible public route/page exists; downloaded bytes/coverage may still be unverified.
- `TERMS_REVIEWED`: current published terms reviewed for the intended use; this is not a license beyond those terms.
- `SAMPLE_VERIFIED`: an actual downloaded file/response has been inspected, hashed and field-checked.
- `CONDITIONAL`: use requires registration, terms confirmation, visible user action or additional review.
- `UNVERIFIED`: insufficient evidence; disabled by default.
- `DEFERRED`: not part of the current operating baseline.

## 1. Indonesia Stock Exchange (IDX)

| Item | Route / evidence | Status | MBI use and limits |
|---|---|---|---|
| Terms of Use | https://idx.co.id/id/syarat-penggunaan/ | `TERMS_REVIEWED` on 13 Nov 2026 | IDX terms say website information may be cited for non-commercial use with complete source and access date, and prohibit web scraping/crawling. Do not build a crawler. |
| Ringkasan Saham | https://www.idx.co.id/id/data-pasar/ringkasan-perdagangan/ringkasan-saham/ | `ROUTE_OBSERVED`; actual export `UNVERIFIED` | Page shows an `Unduh` control and daily fields including ticker/name, previous/open/high/low/close/change, volume/value/frequency, bid/offer, listed/tradeable shares, foreign buy/sell and non-regular activity where displayed. Actual downloaded file format, rows, all-market coverage, date selector/history and exact filters remain to be verified from one user-triggered download. Candidate primary batch for broad EOD, not yet accepted as source until sample passes. |
| Disclosure | https://block.idx.id/en/listed-companies/disclosure | `CONDITIONAL` | Use selected official documents downloaded through visible permitted route. Do not crawl categories or bulk-harvest. Preserve publication date, event/effective dates and source URL. |
| Public Expose Live | https://pubexlive.idx.co.id/ | `CONDITIONAL` | Selected presentations/session documents only when officially available. Do not assume all issuers have material/transcripts. |
| Financial statements / annual reports | https://www.idx.co.id/id/perusahaan-tercatat/laporan-keuangan-dan-tahunan | `CONDITIONAL` | Selected user-triggered PDF/XLSX/XBRL package. Validate file formats and filing vintages; no automated site crawl. |
| XBRL reference | https://www.idx.co.id/id/perusahaan-tercatat/xbrl | `CONDITIONAL` | Taxonomy/guide is versioned; validate each filing schemaRef and contexts. |
| IDX Data Services | https://data.idx.co.id/ | `DEFERRED` for Rp0 baseline | Real-time/delayed/EOD/historical product entitlements are not assumed free. No paid dependency under current decision. |

## 2. Other official/public macro and policy sources

| Publisher | Route | Status | MBI use and limits |
|---|---|---|---|
| Bank Indonesia | https://www.bi.go.id/ | `CONDITIONAL` | Prefer official statistical publications/tables downloaded in batches. Verify each series' period, publication/vintage, file/API route and terms. No continuous polling assumption. |
| BPS WebAPI | https://webapi.bps.go.id/developer/ | `CONDITIONAL` | WebAPI is described as free for compliant use but requires application registration/approval and a secret token; rate limits apply. Terms prohibit scraping/harvesting/polling or automated collection patterns that burden service. Only documented, bounded API batch requests with caching and attribution may be considered; never scrape HTML or evade quotas. |
| BPS service information | https://ppid.bps.go.id/app/konten/0000/Layanan-BPS.html | `TERMS_REVIEWED` on 13 Nov 2026 | Describes free WebAPI registration and distinguishes paid statistical products. Recheck current API terms and app approval. |
| Kementerian Keuangan | https://www.kemenkeu.go.id/ | `CONDITIONAL` | User-triggered official APBN/fiscal document/table downloads; machine-readable route must be sample-verified. |
| OJK | https://www.ojk.go.id/ | `CONDITIONAL` | Selected official regulations/statistics; store promulgation/effective dates and amendments. |
| ESDM | https://www.esdm.go.id/ | `CONDITIONAL` | Selected energy/mineral policy and statistics; distinguish publication, policy effective date and operational impact. |

## 3. Secondary, issuer and platform sources

| Source | Status | MBI use and limits |
|---|---|---|
| Issuer investor relations | `CONDITIONAL` | Selected reports/presentations; issuer claims remain attributed claims until independently verified. User-triggered document batch only. |
| Reputable news/public web | `CONDITIONAL` | User/assistant-captured public research or a specifically documented permitted feed. No guaranteed free unlimited news API/crawler. Local search covers indexed corpus only. |
| TradingView Basic | `DEFERRED` | User confirms current Basic plan cannot export chart data. No upgrade requested; not a Gate -1 requirement or acquisition dependency. |
| Stockbit | `CONDITIONAL / SELECTED_INTRADAY` | Optional screenshot/observation for selected issuers if user's visible entitlement permits. No verified public bulk API. Never use for whole-market automated capture. Preserve panel type, filters, timestamp and update semantics. |
| BIONS | `DEFERRED` | Not required. Personal portfolio is manual-first; no BIONS report, credentials or personal account data is requested. |
| DataCedar / other free third-party APIs | `UNVERIFIED / OPTIONAL` | Not a primary/broad-market source. May be evaluated for limited selected-issuer research only after current terms, rate/quota, fields, history and retention permissions are checked. No archive replication or quota circumvention. |

## 4. Source evidence and terms references

- IDX Terms of Use: https://idx.co.id/id/syarat-penggunaan/
- IDX Ringkasan Saham: https://www.idx.co.id/id/data-pasar/ringkasan-perdagangan/ringkasan-saham/
- IDX Disclosure: https://block.idx.id/en/listed-companies/disclosure
- IDX Public Expose Live: https://pubexlive.idx.co.id/
- IDX financial statements: https://www.idx.co.id/id/perusahaan-tercatat/laporan-keuangan-dan-tahunan
- IDX Data Services: https://data.idx.co.id/
- BPS WebAPI developer and terms: https://webapi.bps.go.id/developer/
- BPS service information: https://ppid.bps.go.id/app/konten/0000/Layanan-BPS.html

A page with an `Unduh` button is not yet a verified import source. Gate -1 must inspect the actual downloaded file, field definitions, row count, selected filters, coverage and terms. Do not infer a complete market universe from the page's visible row count.

## 5. Source registry metadata

Every source/route version must store:

`source_id`, publisher, source type, route URL, acquisition mode, terms URL/version/review date, permitted purpose, authentication/registration, rate/quota, caching/retention rule, fields/units, timezone, publication/update cadence, observed coverage, sample hash, parser version, last verification date, known limitations and manual fallback.

## 6. Open-world discovery coverage

Open-world discovery may use top-down official macro/policy/document batches and bottom-up broad EOD cross-sectional batches. Selected-issuer intraday captures add research depth but never expand the declared broad-EOD denominator. Candidate briefs must cite the exact imported source artifacts and disclose which issuers/fields/timeframes were covered. A no-match result is only a no-match in the indexed/captured source scope.
