# Skill: migrate.06.definition-of-done

## Done when

### Structure

- Temporary migration base is gone or thin, and you have base(s) per deployable (as applicable):
  - `bases/<top_ns>/api/`
  - `bases/<top_ns>/cli/`
  - `bases/<top_ns>/worker/`

- Bases contain only entrypoints/wiring.
- All non-entrypoint code lives in components:
  - `components/<top_ns>/<brick>/...`

### Interfaces

- Each component defines its public API via brick-level `__init__.py`.
- Bricks import each other via those APIs (no deep imports).

### Verification

- `RUN_TEST_CMD` passes.
- If set: `RUN_LINT_CMD` passes.
- If set: `RUN_TYPECHECK_CMD` passes.

### Infra

- Deployable infra is organized under `projects/<deployable>/`.

### Dependencies

- Workspace root `pyproject.toml` contains all third-party dependencies used anywhere in the workspace.
- Workspace root `pyproject.toml` contains dev/test/tooling deps (lint, format, test, typecheck).
- Project `pyproject.toml` contains only deployable-specific runtime deps (if any).
