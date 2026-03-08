# Skill: migrate.03.isolate-base-and-big-component

## Goal

Shrink the temporary migration base into thin base(s) + one big component:

- Bases contain only entrypoints/wiring (FastAPI endpoints/init; CLI wiring; consumer wiring).
- The big component contains everything else.

Target namespace for bricks is `TARGET_TOP_NS`.

Typical targets:

- `bases/<TARGET_TOP_NS>/api/` (if API exists)
- `bases/<TARGET_TOP_NS>/cli/` (if CLI exists)
- `bases/<TARGET_TOP_NS>/worker/` (if worker/event handler exists)
- `components/<TARGET_TOP_NS>/app/` (big component)

## FastAPI guidance (if applicable)

Keep in API base:

- `app = FastAPI(...)`
- middleware, router registration
- route handlers (endpoints) and request/response mapping
- startup/shutdown/lifespan wiring

Move into big component:

- domain/business logic
- persistence/repositories
- external integrations
- reusable parsing/validation

## Steps

1) Create big component:
- `components/<TARGET_TOP_NS>/app/`

2) Move non-entrypoint code from the base(s) → big component.

3) Define a minimal public API in:
- `components/<TARGET_TOP_NS>/app/__init__.py`

4) Update bases to import only from component APIs (prefer absolute imports):
- `from <TARGET_TOP_NS>.app import ...`

## Verify (stop/go gate)

- Run `RUN_TEST_CMD`
- If set: run `RUN_LINT_CMD`
- If set: run `RUN_TYPECHECK_CMD`
