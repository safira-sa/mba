# 10 — Software Architecture, Maintainability & Agent Governance

## 1. Architecture choice

Use a **modular monolith**: one local application/process with clear module boundaries. Do not start with microservices, Docker, local LLM infrastructure or cloud deployment. Target a Windows laptop with 8 GB RAM; prefer simple dependencies and lazy/optional analytics.

Initial likely stack (subject to Sprint -1 environment audit): Python, Streamlit UI, SQLite operational metadata, raw file archive, Parquet for larger time-series files. DuckDB is optional and should be introduced only after measured need. Do not pre-install a large ML stack.

## 2. Suggested project structure

```text
market-behavior-intelligence/
  AGENTS.md
  CLAUDE.md
  README.md
  pyproject.toml
  .gitignore
  src/mbi/
    app.py
    config/
    domain/          # typed entities and invariants
    ingestion/       # batch intake, raw archive, staging, validation, imports
    sources/         # source-specific adapters only
    documents/       # PDF/XLSX/XBRL extraction and provenance
    knowledge/       # claims, facts, notes, links, search
    market/          # prices, sessions, issuer dossier, features
    episodes/        # episode construction, event linkage, chronology
    research/        # event studies, matching, hypothesis tests
    learning/        # discovery, model evaluation, model registry
    gameplay/        # playbooks, detector contracts and versions
    personal/        # decisions, executions, outcome labels, edge
    portfolio/       # supporting account/exposure context
    storage/         # SQLite repos, Parquet IO, migrations, backup
    services/        # application use cases/orchestration
    ui/              # Streamlit pages/components only
  migrations/
  tests/
    unit/
    integration/
    fixtures/
  data/              # local only; never commit
  docs/
    decisions/
    sprint_reports/
```

This is a target boundary map, not a requirement to create empty folders all at once. Create modules when the relevant sprint arrives.

## 3. Dependency direction

- UI → application services → domain/analytics interfaces.
- Source adapters → staging contracts; they do not write directly into canonical analytics tables.
- Document parsers → extraction records with source spans; reviewed facts are created through review services.
- Analytics functions are pure where possible and do not query UI/session state.
- Repositories/storage implement persistence; domain logic does not contain SQL.
- Configuration is validated declarative data. Never execute user YAML/JSON as code.

Avoid catch-all `utils.py`, giant `app.py`, generic managers, circular imports and premature abstraction. Each module should have one reason to change.

## 4. Storage responsibilities

- **SQLite:** source registry, import runs, securities, events, claims/facts, observations, episodes, hypotheses/studies, playbook/detector versions, personal decisions, executions and audit metadata.
- **Raw archive:** original CSV/XLSX/PDF/HTML/ZIP/image/notes, unchanged and content-hashed. A batch can contain a cross-section of many securities; all rows are processed without per-ticker user entry.
- **Parquet:** normalized time-series bars/features at scale, with a clear schema/version and provenance reference.
- **Local search index:** full-text over imported/extracted content only.

Do not add DuckDB or another server until real query volume justifies it. Keep backups for DB and raw archive together.

## 5. Code organization rules

- UI contains display/input only, not SQL or financial formulas.
- One source adapter per source/format; no general scraper bypass. Acquisition modes are explicit: official UI batch download, approved finite API batch, document batch import or selected local capture. Never build an IDX/BPS website crawler.
- One metric per well-scoped function/contract; pure input/output and explicit units.
- Separate event chronology from event interpretation.
- Separate market episode analysis from personal trade outcome analysis.
- Use Decimal for money/price/cost calculations; do not use binary float for authoritative account arithmetic.
- Use typed models and explicit timezone-aware timestamps.
- Add dependencies only for a concrete task and explain why.
- No monolithic prompt asking an agent to build the entire MBI.

## 6. Schema/migration rules

- Actual existing database schema is the source of truth for migration planning.
- Use sequential immutable migrations; never edit an applied migration.
- Before migration: verified backup, integrity check, migration dry-run on a copy and restore test.
- Keep raw artifacts and old extraction/metric/model versions.
- Prefer additive migrations; destructive cleanup requires explicit user approval and verified export/restore.
- Every SQLite connection sets `PRAGMA foreign_keys = ON`.
- Import idempotency and auditability are mandatory.

No database migration is supplied or approved by this specification pack. Do not invent database tables before the schema-gap audit is accepted. The logical model now includes an `acquisition_batch` concept; physical design must be informed by the actual first batch sample.

## 7. Agent roles

### Codex — sole implementation owner
- Inspect project/schema/tests and current sprint ticket first.
- Plan before coding; list files, assumptions, data contracts and test plan.
- Implement one bounded issue only after explicit approval.
- Run actual tests and report exact command/output.
- Update docs, changelog, ADR/backlog and sprint report.

### Claude — independent reviewer
- Read-only by default; review requirements, diff, formula contracts, source semantics, edge cases, tests and migration safety.
- Do not concurrently edit the builder branch or rewrite entire modules.
- Report severity, file/line, reproduction and suggested fix.
- Codex applies accepted fixes; Claude re-reviews.

If using Claude chat rather than Claude Code, provide `CLAUDE.md`, task/diff and relevant docs explicitly; a normal chat does not automatically read repository files.

## 8. Required task workflow

`Inspect → Plan → Approve → Implement bounded task → Test → Diff review → Independent review → Fix → Commit → Update docs`

Before implementation state:
- user problem and non-goals;
- modules/files affected;
- data/source/terms implications;
- metric/accounting implications;
- compatibility/migration plan;
- privacy/security risks;
- acceptance tests and rollback/disable path.

## 9. Security and operations

Never store broker login credentials, PIN, OTP, SID/SRE/RDN identifiers unless an explicitly approved use case truly requires non-sensitive identifiers; default is not to collect them. `.env`, reports, screenshots, DB and raw data are local-only and gitignored. No automatic order execution or private endpoint scraping.

Every release needs database integrity, foreign-key checks, clean-start test, backup **and restore** test, raw archive backup, sample-data regression and documented source coverage. The zero-budget baseline is user-triggered batch acquisition plus automated local processing; no always-on crawler/live feed is implied.
