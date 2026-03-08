# Skill: migrate.04.split-big-component

## Goal

Split the big component (`components/<top_ns>/app/`) into multiple focused components.

## Extraction loop

Repeat:

1) Choose a slice (integration, storage, domain capability).
2) Create `components/<top_ns>/<brick>/`.
3) Move code into it.
4) Define public API in `components/<top_ns>/<brick>/__init__.py`.
5) Update callers to import only from the brick API (prefer absolute imports; avoid `from .` style relative imports).
6) If duplication suspected: run `migrate.07.dedupe-assess`.
7) Verify.

## Verify (stop/go gate)

- Run `RUN_TEST_CMD`
- If set: run `RUN_LINT_CMD`
- If set: run `RUN_TYPECHECK_CMD`
