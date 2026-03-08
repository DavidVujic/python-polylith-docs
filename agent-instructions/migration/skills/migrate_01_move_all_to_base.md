# Skill: migrate.01.move-all-to-base

## Goal

Move all application code into a **temporary migration base** while keeping the repo green under tests/lint.

Target shape:

- `bases/<TARGET_TOP_NS>/<base_name>/...` (often `<base_name>=app`)

## Inputs

From `migration/state.md`:

- `ORIG_TOP_NS`
- `TARGET_TOP_NS` (default: same as `ORIG_TOP_NS`)
- `RUN_TEST_CMD` (+ optional `RUN_LINT_CMD`, `RUN_TYPECHECK_CMD`)

## Steps

1) Create the base directory:
- `bases/<TARGET_TOP_NS>/<base_name>/`

2) Move application packages/modules under the base.

Quick guidance:
- `src/` layout: move `src/<pkg>` under the base (preserve package structure as much as possible).
- flat layout: move `<pkg>/` under the base.

3) Fix imports *minimally* so tests/lint pass.

### If you changed the top namespace

If `TARGET_TOP_NS != ORIG_TOP_NS`, you have two options:

- **Preferred (lower risk):** keep `ORIG_TOP_NS` as a compatibility shim that re-exports from `TARGET_TOP_NS`, then migrate call sites gradually.
- **Higher risk:** rewrite all imports to the new namespace in one go.

### Shim tactic (use when imports break everywhere)

If moving code changes import paths widely, add **temporary shims** instead of doing a large refactor:

- Keep the old module path as a thin wrapper.
- Import from the new brick API (`__init__.py`) using **absolute imports** (explicit namespace) and re-export the same names.

Track shims in `migration/shims.md`.

## Verify (stop/go gate)

- Run `RUN_TEST_CMD`
- If set: run `RUN_LINT_CMD`
- If set: run `RUN_TYPECHECK_CMD`
