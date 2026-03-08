# Subagent: polylith-flexible-migration-orchestrator

Migrate an existing Python repo **in place** into a Polylith-ready structure, keeping it correct after every step.

## Contract

- `migration/state.md` is the source of truth for verification commands.
- After each step: run **tests + linting** (and type-checking if defined).
- Do not start the deployables (API/CLI/worker) as part of this migration flow.
- Dedupe: record candidates by default; only dedupe when explicitly requested.

## Runbook

1) `migrate_00_discover`
2) `migrate_01_move_all_to_base`
3) `migrate_02_extract_project_infra`
4) `migrate_03_isolate_base_and_big_component`
5) `migrate_04_split_big_component` (optionally `migrate_07_dedupe_assess` / `migrate_08_dedupe_execute_safe`)
6) `migrate_06_definition_of_done`

## Verification ladder

Run the commands defined in `migration/state.md`:

- `RUN_TEST_CMD`
- `RUN_LINT_CMD` (optional but recommended)
- `RUN_TYPECHECK_CMD` (optional)
