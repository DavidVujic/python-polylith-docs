# Dependencies
Projects are located in the _projects_ folder of a Polylith workspace.
Each project has its own `pyproject.toml`,
where the project-specific dependencies are defined.

## Bricks (your Python code)
Bricks are added to the `packages` section of `[tool.poetry]` or the `[tool.hatch.build.force-include]`,
described in [Projects & pyproject.toml](projects.md).

To keep a project up-to-date with needed bricks, there is a `poly sync` command available.
Usage is described in [commands](commands.md). The command is there for convenience, you can also add the bricks manually.

## Libraries (third-party dependencies)
The recommended workflow for Polylith is to use `poetry add`, or manually adding, to install the needed libraries to the __Development__ `pyproject.toml`.
The Development `pyproject.toml` is located at the Workspace root.
The Development environment should have __all__ dependencies added: all bricks and all the third-party libraries.
The dev-dependencies (such as Mypy, Black, Flake8, Ruff etc.) are also added to the Development `pyproject.toml`.

__A Project__ should only include the dependencies that are needed for it to run in a production environment.

Libraries are added separately into the project-specific `pyproject.toml` (pick the depencencies based on what's already in Development). 
_The Python tools for the Polylith Architecture_ has a keep-things-simple approach to dependencies:
just copy the needed dependencies from the development project, or use `poetry add` into a project.

Use the `poly libs` and/or `poly check` [command](commands.md) to verify all that all dependencies have been added.

Both commands support the `--directory` option (coming from Poetry).
This means that you can run the commands from the workspace root, but for a specific project:

### Poetry
``` shell
poetry poly check --directory projects/my-project
```

### Hatch
``` shell
hatch run poly check --directory projects/my-project
```

### PDM
``` shell
pdm run poly check --directory projects/my-project
```

### Rye
``` shell
rye run poly check --directory projects/my-project
```

### uv
``` shell
uv run poly check --directory projects/my-project
```

### Maturin
``` shell
# if not already activated a virtual environment
source .venv/bin/activate

poly check --directory projects/my-project
```
