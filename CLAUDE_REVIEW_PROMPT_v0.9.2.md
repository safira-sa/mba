# Claude Independent Review Prompt — MBI v0.9.2 Gate -1

Act as a skeptical, read-only reviewer of Codex's Gate -1 closure report. The user has approved: Rp0 budget, batch-first broad-market acquisition, limited selected-issuer intraday, automated local processing after intake, no TradingView Basic export dependency, and no BIONS/personal portfolio import.

Review:
1. Does the source strategy distinguish acquisition from processing and avoid per-ticker market-data entry?
2. Does IDX Ringkasan Saham have actual evidence for the download file format, rows, filters, date history and coverage, rather than relying on the visible page/table?
3. Are IDX no-scraping/crawling terms respected? Is any automated route explicitly authorized and current?
4. Are BPS API registration, secret handling, rate limits and restrictions correctly reflected? Is BI treated as a publication/download source unless an actual API route is verified?
5. Are broad EOD, historical daily depth, selected intraday, broker flow and orderbook separated by real source coverage?
6. Are source statuses (`ROUTE_OBSERVED`, `TERMS_REVIEWED`, `SAMPLE_VERIFIED`, `CONDITIONAL`, `UNVERIFIED`, `DEFERRED`) applied consistently?
7. Does the universe derive from actual imported batch rows and a source-backed security master, not from the user's watchlist or a page's advertised row count?
8. Does the report acknowledge prospective accumulation if historical EOD is not available at Rp0, and avoid unsupported historical backtests?
9. Are TradingView Basic, BIONS and personal portfolio data correctly excluded from Gate -1 samples and source dependencies?
10. Is intraday optional/selected-issuer and non-blocking for the EOD baseline?
11. Is Gameplay A explicitly illustrative only and removed as a Gate -1 user-definition blocker?
12. Are acquisition batch, raw artifact, ingestion run, source coverage and universe coverage logically distinct without duplicate concepts?
13. Does the report list only necessary user actions, ideally one official batch download if required?
14. Are all claims supported by source evidence or labelled unverified?

For each issue, provide severity, exact file/line evidence, consequence and minimal correction. Do not edit files, infer access permissions, claim source/sample verification without actual evidence or recommend paid providers contrary to the Rp0 decision.

Return `ACCEPT_GATE_MINUS_1_REPORT` or `REVISE_GATE_MINUS_1_REPORT`. Gate approval remains the user's decision.
