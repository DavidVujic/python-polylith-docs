# Skill: migrate.02.extract-project-infra

## Goal

Move deployable-specific infra under `projects/<deployable>/` (Dockerfiles, k8s/Helm, deploy scripts, etc.) and prepare project packaging config.

This separates:

- **code** (bases/components)
- **deployable infra** (projects)

## Steps

1) Create `projects/<name>/` directories as needed (e.g. `projects/api`, `projects/cli`, `projects/worker`).

2) Move infra into the matching folder.

This includes project-level task runners when they are deployable-specific:

- `Makefile`
- `justfile`

If the repo currently has a single root `Makefile`/`justfile` orchestrating multiple deployables, split it so each deployable has its own file under its `projects/<deployable>/` directory.

3) **Project `pyproject.toml`: include bricks via `[tool.polylith.bricks]`.**

When you create a project-specific `pyproject.toml` under `projects/<name>/`, reference the base and component bricks using the Polylith mapping style:

```toml
[tool.polylith.bricks]
"../../bases/<TARGET_TOP_NS>/<base>" = "<TARGET_TOP_NS>/<base>"
"../../components/<TARGET_TOP_NS>/<component>" = "<TARGET_TOP_NS>/<component>"
```

Notes:

- This `[tool.polylith.bricks]` mapping is the common approach for Hatch/PDM/Rye/uv/Maturin-style setups.

### Poetry: include bricks via `[tool.poetry].packages`

Poetry projects typically include bricks by listing each base/component under `packages` with a `from` path.

From a project-specific `pyproject.toml` located at `projects/<name>/pyproject.toml`, you usually reference:

- bases from `../../bases`
- components from `../../components`

Example:

```toml
[tool.poetry]
name = "<project_dist_name>"
packages = [
  { include = "<TARGET_TOP_NS>/<base>", from = "../../bases" },
  { include = "<TARGET_TOP_NS>/<component>", from = "../../components" },
  { include = "<TARGET_TOP_NS>/<other_component>", from = "../../components" },
]
```

Rules of thumb:

- `include` is the package path *inside* the brick, e.g. `<top_ns>/<brick_name>`.
- `from` is the directory where that package lives, relative to the project dir.
- Be explicit: list each brick you want in the deployable. Don’t rely on wildcard packaging.

Keep truly shared infra at repo root if it orchestrates multiple deployables (e.g. root `docker-compose.yml`, shared CI workflows).

## Dependencies policy (3rd-party + dev/test)

When transitioning to a Polylith-style workspace structure:

- **Workspace root `pyproject.toml`** should contain:
  - all third-party dependencies used anywhere in the workspace (union)
  - all dev/test/tooling dependencies (lint, format, test, typecheck)

- **Project `pyproject.toml`** under `projects/<deployable>/` should contain:
  - only deployable-specific runtime dependencies (if any)
  - and brick references (via `[tool.polylith.bricks]` or Poetry `packages`)

Practical guidance:

- Remove dev/test dependencies from project `pyproject.toml` once the workspace root is the canonical dev environment.
- Keep (or add) deployable-specific runtime deps in the project when packaging/building that deployable requires it.

## Verify (stop/go gate)

- Run `RUN_TEST_CMD`
- If set: run `RUN_LINT_CMD`
- If set: run `RUN_TYPECHECK_CMD`
