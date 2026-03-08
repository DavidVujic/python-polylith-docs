# Skill: migrate.00.discover

## Goal

Create `migration/state.md` (the migration verification contract).

**Policy:** Verify changes via tests + linting (and optional type-checking).

## Top namespace decision (important)

Record:

- `ORIG_TOP_NS`: the current import namespace used by the repo (e.g. `myapp`)
- `TARGET_TOP_NS`: the top namespace you want in the Polylith-ready structure

Default (recommended):

- set `TARGET_TOP_NS = ORIG_TOP_NS`

Renaming the top namespace during migration implies widespread import rewrites; avoid unless you explicitly accept the added risk.

## How to discover the commands

Prefer commands that the repo already defines in config/docs:

1) `Makefile` targets (e.g. `make test`, `make lint`)
2) `pyproject.toml` task runner / scripts (tool-dependent)
3) `tox.ini` / `noxfile.py`
4) CI workflow steps (reference only)

When the repo uses a package manager runner, record the full command (e.g. `poetry run pytest`, `uv run ruff check .`, `hatch run pytest`).

## What to record

Write a minimal `migration/state.md` like:

```text
# verification
RUN_TEST_CMD=<command that runs tests>
RUN_LINT_CMD=<command that runs lint/format check>   # optional
RUN_TYPECHECK_CMD=<command that runs type checking>  # optional

# namespace
ORIG_TOP_NS=<current top-level import package>
TARGET_TOP_NS=<desired polylith top namespace>

# layout
LAYOUT=src|flat
NOTES=...
```

## Done when

- `RUN_TEST_CMD` succeeds.
- If `RUN_LINT_CMD` is set, it succeeds.
- If `RUN_TYPECHECK_CMD` is set, it succeeds.
- `ORIG_TOP_NS` and `TARGET_TOP_NS` are recorded (and usually equal).
