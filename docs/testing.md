# Testing
The code that you write in the bricks is just Python.
That means you can use your favorite tools as you would with any mainstream Python project.

## Testing Bricks
By default, tests are added when creating a new component or base with the `poly create` [command](commands.md).
This [is optional and can be turned off](configuration.md) in the Workspace configuration.
The tests are added in a `test` folder at the root of the workspace with the same kind of folder structure as the bricks.

### Example
Creating a new `parser` component. This will add a new brick to the `components` folder.

#### Poetry
``` shell
poetry poly create component --name parser
```

#### Hatch
``` shell
hatch run poly create component --name parser
```

#### PDM
``` shell
pdm run poly create component --name parser
```

#### Rye
``` shell
rye run poly create component --name parser
```

A corresponding unit test will also be created in the `test` folder:
``` python
from my_top_namespace.parser import core


def test_sample():
    assert core is not None
```

Add the proper assertions to your tests during development of the bricks.

## Running tests
Running Pytest from the workspace root:

#### Poetry
``` shell
poetry run pytest
```

#### Hatch
``` shell
hatch run pytest
```

#### PDM
``` shell
pdm run pytest
```

#### Rye
``` shell
rye run pytest
```

#### Running tests with pytest and the TDD Theme
This configuration ensures the entire namespace of your brick is included
when `pytest` does it's test lookup. Without it, `pytest` will raise errors
because of the default way it does module lookups. This occurs when using Polylith with the _TDD_ theme.

``` toml
[tool.pytest.ini_options]
addopts = [
    "--import-mode=importlib",
]
```

### Running tests for changed code
The __Python tools__ for the Polylith Architecture doesn't (yet) have a specific `test` command.
You can use `poly diff` and your favorite test runner to only run the corresponding tests for changed code.

The `diff` command has support for displaying the changed bricks by using `--bricks`.
Append the `--short` option for a scripting-friendly output.

You can use the output from the `poly diff` command to run specific tests.
Storing a list of bricks in a bash variable:

#### Poetry
``` shell
changes="$(poetry poly diff --bricks --short)"
```

#### Hatch
``` shell
changes="$(hatch run poly diff --bricks --short)"
```

#### PDM
``` shell
changes="$(pdm run poly diff --bricks --short)"
```

#### Rye
``` shell
changes="$(rye run poly diff --bricks --short)"
```

By having tests in the same kind of structure as the bricks,
you can use the output from the `poly diff` command to pick the tests to run.
You can also name the individual test functions to include brick names, or use decorators (such as `pytest` markers).

### Pytest
Transform the result of the `poly diff` command into a Pytest _keyword_ or _marker expression_.
(i.e. from __hello,world,something__ to __hello or world or something__).

- `-k` is for running tests by keyword expressions.
- `-m` is for running tests by marker expressions.

#### Poetry
``` shell
query="${changes//,/ or }"

poetry run pytest -k <<< echo "$query"
```

#### Hatch
``` shell
query="${changes//,/ or }"

hatch run pytest -k <<< echo "$query"
```

#### PDM
``` shell
query="${changes//,/ or }"

pdm run pytest -k <<< echo "$query"
```

#### Rye
``` shell
query="${changes//,/ or }"

rye run pytest -k <<< echo "$query"
```

## Manually testing and running services
The `bases` and `components` is plain Python,
and the code can be run as you would if it were a single project repository.
The bases are the __entry points__ to your services and apps.

### Example: running a FastAPI service locally
Start the service by specifying the namespace path to your entry point (i.e. the `base`).
``` bash
uvicorn my_top_namespace.my_fastapi_service.core:app
```

It is also possible to test out project-specific
entries that are defined in a `pyproject.toml` as you would in a mainstream Poetry project.
