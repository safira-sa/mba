# MBI Repository Instructions — Codex / Implementation Agent

## Active specification

The canonical specification is `README.md`, `GATE_MINUS_1_DECISION_RECORD.md`, `docs/00`–`docs/18`, and `docs/decisions/ADR-001` through `ADR-004`. `CODEX_START_HERE_v0.9.2.md` is the single active task instruction. Historical v0.5–v0.9.1 packs are archival; do not combine them with this package.

## Current authorization

**Gate -1 closure only; read-only.** Do not build application code, physical schema, SQL migrations, parsers, scanners, backtests or install packages. The user has selected Rp0, batch-first broad-market acquisition, limited selected-issuer intraday and automated local processing after batch intake.

## Product invariants
- Local-first Windows, IDX focus, no paid data/API dependency.
- Broad-market market data must be obtained as a permitted batch wherever possible; no manual ticker-by-ticker data entry.
- Acquisition and post-intake processing are distinct. A user-triggered official download can feed automated local parsing, validation, normalization and analysis.
- IDX website terms prohibit scraping/crawling. Do not crawl/scrape IDX, reverse-engineer private endpoints or evade quotas.
- BPS WebAPI is conditional on registration, token security, current terms and rate limits; no burdening polling or HTML scraping.
- Intraday is selected-issuer/event research only, not whole-market scope.
- TradingView Basic export is unavailable to this user; do not request upgrade/export or make it a dependency.
- BIONS and personal account exports are excluded. Portfolio is manual-first, local-only supporting context.
- Gameplay A is an assistant-created illustration, not user-provided gameplay or a Gate -1 blocker.
- Market learning and open-world discovery must work without a user-defined gameplay.
- Missing data is UNKNOWN/null, never zero. Facts, source claims, observations, measurements, hypotheses, inferences and unknowns remain distinct.
- No look-ahead, unsupported probabilities, inferred broker identity, unverified ownership or claimed whole-market coverage without evidence.

## Architecture invariants
- Modular monolith; no microservices/Docker/local LLM at start.
- UI has no SQL or financial formula logic.
- Batch/source adapters parse to staging; they cannot bypass validation.
- Analytics functions are deterministic and independently tested.
- Original files/notes remain immutable; derived records preserve lineage.
- One acquisition batch may contain many issuer rows. The logical model must distinguish acquisition batch, raw artifact, ingestion run and coverage snapshot.
- Avoid monolithic `app.py`, catch-all `utils.py` and arbitrary user-code execution.
- Every schema change uses a new sequential migration; never edit an applied migration.
- Every SQLite connection enables `PRAGMA foreign_keys = ON`.
- Personal data, raw files, DB, exports, reports, screenshots, `.env` and secrets stay outside Git.

## Required workflow after a future sprint is approved
1. Read sprint ticket, relevant docs, code/schema/samples and Git status.
2. Present a bounded plan and wait for user approval.
3. List assumptions, affected files, source/terms, tests, migration and rollback.
4. Implement one bounded ticket only.
5. Add real tests; do not use mock-only validation for core calculations.
6. Run tests and report exact commands/results.
7. Review diff for data leaks, hidden assumptions and destructive changes.
8. Update docs, ADR/backlog and changelog.
9. Stop at acceptance criteria.

## Current task boundary

Follow `CODEX_START_HERE_v0.9.2.md` exactly. It requests Gate -1 source/batch-feasibility closure only.
