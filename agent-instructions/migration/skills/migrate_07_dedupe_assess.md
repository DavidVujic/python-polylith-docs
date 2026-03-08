# Skill: migrate.07.dedupe-assess (identify duplication candidates safely)

## When to use

During Step 4 (splitting the big component), after extracting a potential new component.

Use this when you suspect the extracted code duplicates functionality from another repo that has already been migrated.

## Goal

Capture dedupe opportunities **without taking on dedupe risk** during the structural migration.

## Default policy

- **Do not dedupe by default.**
- Record candidates and proceed with the migration.
- Only run `migrate.08.dedupe-execute-safe` when the user explicitly opts in.

## Assessment checklist (quick)

- Is it pure/deterministic? (lower risk)
- Does it touch I/O? (higher risk)
- Do we have tests/golden tests? (if not: defer)
- Any semantic drift between repos? (if yes: defer)

## Artifact

Append an entry to `migration/dedupe_candidates.md`.

## Done when

- Candidate is recorded with confidence + risk.
- Migration continues without introducing cross-repo dependencies.
