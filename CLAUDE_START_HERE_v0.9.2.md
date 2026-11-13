# MBI v0.9.2 — Claude Code Start Here

## Purpose
This file is the entry point for Claude Code when working on the MBI v0.9.2 specification package.

## Source of truth
Treat this v0.9.2 package as the current authoritative product specification. Read `README.md`, `AGENTS.md`, `CLAUDE.md`, `GATE_MINUS_1_DECISION_RECORD.md`, `CHANGELOG.md`, `docs/00_PRODUCT_CHARTER_AND_TERMS.md`, `docs/01_END_TO_END_LEARNING_FLOW.md`, `docs/02_DATA_SOURCES_AND_INGESTION.md`, `docs/09_METRIC_CONTRACTS_AND_VALIDATION.md`, `docs/10_SOFTWARE_ARCHITECTURE_AND_AGENT_GOVERNANCE.md`, `docs/11_ROADMAP_AND_ACCEPTANCE_GATES.md`, `docs/12_QA_TEST_MATRIX.md`, `docs/13_SOURCE_REGISTER.md`, `docs/14_LOGICAL_DATA_MODEL.md`, `docs/15_PRODUCT_EVOLUTION_AND_COMPATIBILITY.md`, `docs/16_SAMPLE_DATA_CHECKLIST.md`, `docs/17_OPEN_WORLD_OPPORTUNITY_DISCOVERY_AND_CONVICTION_SUPPORT.md`, and `docs/18_SETUP_INSTANCE_AND_PARAMETER_LEARNING.md`. Follow references to other documents where needed.

## Current operating decisions
- Data budget: Rp0.
- Broad-market acquisition: batch-first. User-triggered lawful downloads/imports are acceptable; processing after intake should be automated.
- Intraday: limited to selected issuers/candidates, not the whole market.
- TradingView Basic export is unavailable and is not required.
- BIONS export and personal portfolio data are not required and must not be requested.
- Actual portfolio entry, if implemented later, is manual-first and local-only.
- No IDX scraping/crawling or private/authenticated endpoint reverse engineering.
- No cloud transmission of personal data by default.
- Synthetic data must be explicitly labelled and cannot validate a real provider's file format.
- Gameplay A is an illustrative example only. It is not the user's actual strategy and is not a Gate -1 blocker.
- MBI must support user-originated, data-discovered, and hybrid gameplay. Open-world discovery must work without a user-provided setup.

## Agent rules
1. Do not start application implementation while Gate -1 is blocked.
2. Do not create physical database schemas, migrations, ingestion code, scanners, backtests, or UI before the relevant gate is explicitly approved.
3. Do not invent source access, data coverage, fields, licensing rights, API endpoints, prices, or sample files.
4. Distinguish verified facts, provider claims, assumptions, unknowns, and blocked capabilities.
5. Never describe batch EOD as live/intraday or as complete market coverage unless the evidence supports that exact claim.
6. Preserve provenance: source, original URL, publication/effective date, retrieval timestamp, file hash, terms evidence, coverage, and limitations.
7. Keep personal data outside the repository. Never request actual portfolio exports, BIONS statements, credentials, account identifiers, or private transaction history.
8. Keep changes small, traceable, and documented. Do not rewrite unrelated documents or silently remove requirements.
9. If documents conflict, report the conflict and propose the smallest correction. Do not choose silently.
10. After every task, report files read, files changed, checks run, results, unresolved issues, and whether the gate is passed or blocked.

## First task
Run the Gate -1 audit described in `prompts/CLAUDE_GATE_MINUS_1_PROMPT.txt`. Do not code. Gate -1 must establish real public-source feasibility and sample evidence before Sprint 0 implementation planning.
