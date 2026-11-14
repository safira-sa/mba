# MBI — Sprint 0 Proposal (Project Foundation Only)

**Status:** PROPOSED, **not started, not authorized** · **Date:** 14 Nov 2026 · **Author:** Claude (builder) · **Reviewer:** Codex · **Accepts:** owner
**Rule:** one bounded ticket. Nothing in Sprint 0 touches market data.

## 1. Purpose

Create a clean, safe project shell so later sprints cannot leak personal or raw data, drift in conventions, or lose reviewability. Sprint 0 delivers governance and tooling, not product behavior.

## 2. Start conditions (dependencies)

1. Owner approves the Decision Sheet (at minimum D-B, D-E, D-H; others do not block Sprint 0).
2. Codex has reviewed the five proposed reports.
3. P0-docs (docs-only spec patch) is applied and accepted. In particular N1 (roles), D3, D8, D9 and D14 must be in the documents, because Sprint 0 encodes them.
4. Environment check at start (not assumed): whether a Git repository already exists, the Windows/Python versions present, free disk space. Findings are reported before any installation.

No data gate (A–D) is required. Gates A–D start constraining work at Sprint 2.

## 3. In scope (tasks)

| # | Task | Output |
|---|---|---|
| S0-1 | **Repository structure.** Only the top-level skeleton from Doc 10 §2: `src/mbi/` (package marker only), `tests/`, `docs/` (existing), `config/`, `reports/` (reviewer/proposal outputs, untracked content rules below), `archive/` (existing superseded material), `migrations/` **not created** (no schema yet). Do not create empty module folders for later sprints. | Directory tree documented in README |
| S0-2 | **Environment & dependency management.** `pyproject.toml` with project metadata and a `dev` dependency group limited to the test runner and a linter. Virtual environment convention documented. Versions pinned **at execution time** from what installs successfully, then recorded. No UI, data, spreadsheet, database or HTTP libraries. | `pyproject.toml`; setup steps in docs; pinned versions recorded |
| S0-3 | **Git conventions.** Short-lived branch per ticket; one ticket per branch; commit message convention; diff-review checklist (private files, secrets, unrelated changes); Codex review before merge; owner acceptance recorded in the sprint report. | `docs/CONTRIBUTING-style` note (name per ADR conventions) |
| S0-4 | **`.gitignore`.** Ignore local data root contents, raw files and samples (`*.xlsx`, `*.csv`, `*.pdf`, `*.zip` under data/sample paths), databases, Parquet, logs, `.env*` except templates, local config, reports containing private content, editor/OS/cache files. Remove any tracked real sample (D9) and move it under the local data root. | `.gitignore`; evidence via `git check-ignore -v` |
| S0-5 | **Local-only data directories.** Per Doc 02 §5: `%LOCALAPPDATA%\MBI\{incoming, raw_archive, normalized, database, reports, backups, samples}` created by documented one-time commands, **outside the repo**. No committed script. | Documented commands; directories exist locally |
| S0-6 | **Configuration templates.** `config/mbi.example.toml` with: `data_root`, `external_llm_enabled = false`, `personal_data_may_leave_machine = false`, `network_connectors_enabled = false`, `acquisition_mode = "user_triggered_only"`. Local `config/mbi.local.toml` is ignored. No secrets stored in any template. | Template file; documented defaults |
| S0-7 | **Test runner.** Configured and runnable with one command. Only guard tests: (a) the config template defaults above are the safe values; (b) required ignore patterns exist; (c) no tracked file matches private/raw patterns. These are repository-hygiene tests, not application logic. | Command and passing result recorded |
| S0-8 | **Documentation & ADR conventions.** ADR template and numbering (continue after ADR-004), sprint report template, changelog format (newest first), reviewer report template, naming for proposed vs canonical docs. Record the intended-use statement (D-H) and the LLM default-off rule (D-E) in README/Doc 10. | Templates and README updates |
| S0-9 | **Sprint report.** Exact commands run, results, files changed, verified limitations, acceptance checklist. | `docs/sprint_reports/sprint_0.md` |

## 4. Out of scope (hard exclusions)

Parser or importer; database schema or migration; SQL; any network connector or HTTP library; scanner; backtest; UI (including Streamlit); trading or portfolio engine; metric code; reading, copying or committing the IDX sample; installing data-processing libraries (spreadsheet, dataframe, Parquet, database drivers); historical backfill.

## 5. Exit criteria (all must be true)

1. Fresh clone → documented setup → test command → passes. Exact commands and output are in the sprint report.
2. `git check-ignore -v` confirms each required ignore rule matches an example path; `git ls-files` shows no raw/sample/DB/env/private file.
3. The local data directories exist outside the repository; the repository contains none of their contents.
4. `config/mbi.example.toml` default values are the safe ones listed in S0-6, verified by test.
5. No application code exists beyond a package marker; no schema/migration/network library appears in `pyproject.toml`.
6. Role table, conventions and ADR/report templates are present and consistent with N1.
7. Codex diff review recorded with no open blocker; owner acceptance recorded.
8. Rollback documented: delete the branch; local data directories contain no data.

## 6. Risks and handling

| Risk | Handling |
|---|---|
| Repository state unknown (no repo, existing history) | Environment check first; report before changing |
| Tool versions unavailable on the owner's Windows machine | Pin what installs; record; never guess versions |
| Hygiene tests become de facto application code | Limited to the three guard tests; reviewer checks scope |
| The real IDX sample sits in a tracked folder | S0-4 removes and ignores it; hash recorded in the manifest |
| Scope creep ("while we're here") | Only the ticket; new ideas go through the feature-intake process (Doc 15) |

## 7. Roles

- **Claude:** implements S0-1…S0-9 on one branch; reports.
- **Codex:** independent diff review against this scope; no edits.
- **ChatGPT:** may help the owner judge acceptance criteria; no repository authority.
- **Owner:** approves start and accepts the result.

## 8. After Sprint 0

Sprint 1 (database framework, raw inbox, source registry) can be proposed; Sprint 2 (importer) additionally needs Gates A, B and C1 (see `GATE_STRUCTURE_PROPOSAL.md`).
