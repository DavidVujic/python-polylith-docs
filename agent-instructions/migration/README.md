# Migration (in-place)

Migrate an existing Python repo into a **Polylith-ready** structure (bases/components/projects) in small steps.

## Start here

- `subagents/polylith_flexible_migration_orchestrator.md`

## Verification policy

The migration is verified using **tests + linting** (and optionally type-checking), as defined by the repo’s configuration files.

These playbooks intentionally avoid starting/running the deployables (API/CLI/worker) during the migration steps.

## Steps (high level)

1) Discover test/lint commands from config files (`migration/state.md`).
2) Move all code into a temporary migration base.
3) Move deployable infra under `projects/<deployable>/`.
4) Isolate “outside world” wiring into base(s) and move the rest into a big component.
5) Split the big component into smaller components iteratively.

## Minimal Polylith conventions

- Put brick APIs in brick-level `__init__.py`.
- Do **not** add `__init__.py` at namespace roots (`bases/<TARGET_TOP_NS>/__init__.py`, `components/<TARGET_TOP_NS>/__init__.py`). Leaving it absent enables namespace-package behavior.
- Prefer **absolute imports** (explicit namespace) over relative imports (`from .foo import bar`).
- In project-specific `pyproject.toml`, reference bricks via `[tool.polylith.bricks]`.
- Keep all third-party + dev/test/tooling dependencies in the workspace root `pyproject.toml` (projects only keep deployable-specific runtime deps).

## Dedupe policy

- Record duplication candidates during extraction.
- Dedupe only when explicitly requested and backed by strong tests.
