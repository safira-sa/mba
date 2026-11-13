# 15 — Product Evolution, Feature Intake & Backward Compatibility

## 1. Core rule

MBI is expected to evolve because Indonesian market mechanics, user gameplay, external sources and research questions evolve. New ideas are welcome, but they do not automatically become code, schema or active production behavior.

Every request is first classified and assessed for evidence, dependencies, data rights, calculation impact, compatibility and rollback.

## 2. Request categories

1. **Research question / hypothesis:** asks whether a relationship or behavior exists. Starts as research, not application code.
2. **Configuration/playbook change:** uses existing, supported operators and fields. May need examples/tests but not necessarily a migration.
3. **New metric/feature:** requires a metric contract, confirmed inputs/units, formula version and golden tests.
4. **New detector/model:** requires inputs, outcome definition, sample design, leakage checks, baseline and shadow evaluation.
5. **New source/adapter:** requires source access/terms review, sample, parser contract, rate/quota/fallback and quality tests.
6. **New document type/parser:** requires representative samples and source-span preservation.
7. **New data concept/entity:** may require schema migration and a logical-model update.
8. **UI/workflow change:** should consume existing services and not duplicate domain logic.
9. **Portfolio rule:** requires account-specific semantics, Decimal arithmetic and independently hand-calculated examples.
10. **Bug/data correction:** requires reproducible input, affected records, correction lineage and regression test.

## 3. Change proposal before implementation

For each accepted request, document:
- original user wording and underlying problem;
- expected output/use;
- category and priority;
- evidence/sample data and what remains unknown;
- modules/data concepts affected;
- source terms/access and privacy implications;
- metric, event, learning or accounting implications;
- schema and backwards-compatibility impact;
- non-goals;
- acceptance tests including negative/counterexamples;
- feature flag/disable plan;
- migration/backup/restore plan if relevant;
- owner approval and target sprint.

## 4. When a schema change is actually needed

Do not create a database column/table for every new idea.

- If a new idea can be expressed through an existing supported playbook operator and existing typed fields, use a versioned configuration.
- If a new measurement is required but its inputs already exist, add a metric contract and tested calculation; only persist a new output if there is a defined need and storage design.
- If a new source supplies an existing canonical concept, add an adapter/mapping and source provenance; do not create a duplicate canonical field per provider.
- If a genuinely new business concept or relationship is required, update the logical model first, then propose a sequential migration.
- If a new event/playbook taxonomy label is needed, use extensible reference data with an unknown-safe fallback; do not hardcode a one-off branch in UI code.

## 5. Compatibility and migration

- Every schema migration is numbered, immutable once applied and tested on a copied database.
- Existing records and source artifacts remain readable after upgrades.
- New enum/category values must be handled safely by older screens or the release must declare an upgrade requirement.
- Additive nullable changes are preferred; destructive changes require explicit owner approval and verified export/restore.
- Feature flags allow optional modules to be disabled without corrupting core data.
- Reprocessing a document creates a new extraction run/version.
- New metric, detector, model and playbook versions never overwrite historical outputs.
- Deprecation includes a date/version, replacement path and migration plan; old data is not silently deleted.

## 6. Product evolution workflow

`RAW_REQUEST → TRIAGE → RESEARCH_OR_SPEC → IMPACT_REVIEW → TEST_PLAN → OWNER_APPROVAL → BOUNDED_IMPLEMENTATION → INDEPENDENT_REVIEW → ACCEPTANCE → RELEASE`

A request may be rejected, deferred, kept as a hypothesis or resolved by user workflow/configuration rather than code. The backlog records the reason. No agent may expand an active sprint because a related idea appears during implementation.

## 7. Agent boundary

Codex is the single builder for a bounded ticket. Claude reviews independently. A feature request template is stored at the repository root. Accepted architecture changes get an ADR; shipped changes get a changelog entry and tests.
