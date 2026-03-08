# Skill: migrate.08.dedupe-execute-safe (controlled deduplication)

## When to use

Only when the user explicitly wants to dedupe during migration *and* risk is acceptable.

## Preconditions

- Tests and linting are reliable and fast enough to run repeatedly.
- The duplicated behavior is well understood.

## Strategy (safe sequence)

1) Add characterization tests (golden tests/fixtures) for the duplicated behavior.
   - If you cannot add tests, **do not dedupe now**.

2) Introduce a shared component API (no callers changed yet):
   - `components/<top_ns>/<shared_brick>/__init__.py`

3) Add a compatibility wrapper (shim) for the old API (optional but recommended).

4) Switch one call site at a time.

5) Remove the duplicated implementation.

## Verification (must pass at each sub-step)

- Run `RUN_TEST_CMD`
- If set: run `RUN_LINT_CMD`
- If set: run `RUN_TYPECHECK_CMD`

## Acceptance criteria

- Tests/lint/typecheck remain green throughout.
- Duplicate code is removed or clearly deprecated behind a shim.
- Shared component has a small, documented public API.
