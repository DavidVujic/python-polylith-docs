# Agent instructions (ECA-style): migration

This folder contains ECA-style prompts (skills + a subagent) focused on **migrating an existing Python repository** into a Polylith-ready structure.

## Verification policy (important)

These prompt verify changes by running:

- tests
- linting / formatting / type-checking

…using commands discovered from the repo’s configuration files (e.g. `pyproject.toml`, `Makefile`, `Justfile`, `tox.ini`, `noxfile.py`, CI config).

## Where to start

- `agent-instructions/migration/README.md`
- `agent-instructions/migration/subagents/polylith_flexible_migration_orchestrator.md`
