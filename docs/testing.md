# Testing
The code that you write in the bricks is just Python.
That means you can use your favorite tools as you would with any mainstream Python project.

## Testing Bricks
By default, tests are added when creating a new component or base with the `poly create` [command](commands.md).
This [is optional and can be turned off](configuration.md) in the `workspace.toml`.
The tests are added in a `test` folder at the root of the workspace with the same kind of folder structure as the bricks.

### Example
Creating a new `parser` component. This will add a new brick to the `components` folder.
``` shell
poetry poly create component --name parser
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
``` shell
poetry run pytest
```

### Running tests for changed code
The __Python tools__ for the Polylith Architecture doesn't (yet) have a specific `test` command.
You can use `poly diff` and your favorite test runner to only run the corresponding tests for changed code.

The `diff` command has support for displaying the changed bricks by using `--bricks`.
Append the `--short` option for a scripting-friendly output.

``` shell
poetry poly diff --bricks --short
```

You can use the output from the `poly diff` command to run specific tests.

Storing a list of bricks in a bash variable:
``` shell
changes="$(poetry poly diff --bricks --short)"
```

By having tests in the same kind of structure as the bricks,
you can use the output from the `poly diff` command to pick the tests to run.
You can also name the individual test functions to include brick names, or use decorators (such as `pytest` markers).

### Pytest
Transform the result of the `poly diff` command into a Pytest _keyword_ or _marker expression_.
(i.e. from __hello,world,something__ to __hello or world or something__).

- `-q` is for running tests by keyword expressions.
- `-m` is for running tests by marker expressions.

``` shell
query="${changes//,/ or }"

poetry run pytest -k <<< echo "$query"
```