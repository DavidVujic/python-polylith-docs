# Commands

![poly info example](img/poly-info.png)

## Create a workspace
This will create a Polylith workspace, with a basic Polylith folder structure.

``` shell
poetry poly create workspace --name my_example_namespace --theme loose
```

### Options
`--name` (required) the workspace name, that will be used as the single top namespace for all bricks. __Choose the name wisely.__
Have a look in [PEP-423](https://peps.python.org/pep-0423/#respect-ownership) for naming guidelines.

`--theme` the structure of the workspace (see below).

### Themes
Themes are an exclusive Python Polylith feature, and defines what kind of workspace structure to use.

#### loose (recommended)
A theme to use for a more familiar structure for Python: _components/namespace/package_ and will put a _test_ folder at the root of the repository.

#### tdd
The default and will set the structure according to the original Polylith Clojure implementation, such as:
_components/package/src/namespace/package_ with a corresponding _test_ folder.

#### What's the deal with the .keep files?
When creating a new workspace, the Polylith tool will add `.keep` files in the newly created folders.
These are added for any initial commits of the folder structure, and can safely be removed when adding source files.

## Create a component
This command will create a component - i.e. a Python package in a namespaced folder.

``` shell
poetry poly create component --name my_example_component
```

### Options
`--name` (required) the name of the component.

`--description` adding a docstring to the base.
It will also be added in the README, when enabled in the configuration. See [configuration](configuration.md).

## Create a base
This command will create a base - i.e. a Python package in a namespaced folder.

``` shell
poetry poly create base --name my_example_base
```

### Options
`--name` (required) the name of the base.

`--description` adding a docstring to the base.
It will also be added in the README, when enabled in the configuration. See [configuration](configuration.md).

## Create a project
This command will create a project - i.e. a pyproject.toml in a project folder.

``` shell
poetry poly create project --name my_example_project
```

### Options
`--name` (required) the name of the project.

`--description` adding a _pyproject.toml_ description.


## Info
Show info about the workspace:

``` shell
poetry poly info
```

### Options
`--short` Display a view that is better adjusted to Workspaces with many projects.

## Diff
Shows what has changed since the most recent stable point in time:

``` shell
poetry poly diff
```

The `diff` command will compare the current state of the repository, compared to a `git tag`.
The tool will look for the latest tag according to a certain pattern, such as `stable-*`.
The pattern can be configured in `workspace.toml`.

The `diff` command is useful in a CI environment, to determine if a project should be deployed or not.
It is also useful when running tests for changed bricks only.


### Testing
Example, how to run __pytest__ for changed bricks only:
``` shell
# store the comma-separated list of bricks in a bash variable
changes=$(poetry poly diff --bricks --short)

# transform it into a pytest query,
# i.e. from "hello,world,something" to "hello or world or something"
query=${changes//,/ or }

# run the test, filtered by keyword expression
poetry run pytest -k <<< echo $query

# or run the test, filtered by pytest markers
poetry run pytest -m <<< echo $query
```

### Options
`--short` Useful for determining what projects has been affected by the changes in CI.

`--bricks` Useful for displaying changed bricks only. It will print a comma-separated list of bricks when using it with the `--short` option.



## Libs
Show info about the third-party libraries used in the workspace:

``` shell
poetry poly libs
```

This feature relies on installed project dependencies, and expects a `poetry.lock` of a project to be present.
If missing, there is a Poetry command available:
``` shell
poetry lock --directory path/to-project
```

### Options
`--directory`
Show info about libraries used in a specific project.

`--strict`
A more narrow way of comparing third-party libraries and the actual imports.
This is useful to rule out possible false positives.

`--alias`
Useful when an import differ from the library name.

Example: the library "opencv-python" and the actual import "cv2".
The poly libs command will likely report the "cv2" as a missing dependency.

Using `--alias opencv-python=cv2` will make the command treat the alias as a third-party import.


## Check
Validates the Polylith workspace, checking for any missing dependencies (bricks and third-party libraries):

``` shell
poetry poly check
```

This feature is built on top of the `poetry poly libs` command,
and expects a `poetry.lock` of a project to be present.


### Options
`--directory`
Show info about libraries used in a specific project.

`--strict`
A more narrow way of comparing third-party libraries and the actual imports.
This is useful to rule out possible false positives.

`--alias`
Useful when an import differ from the library name.

Example: the library "opencv-python" and the actual import "cv2".
The poly check command will likely report the "cv2" as a missing dependency.

Using `--alias opencv-python=cv2` will make the command treat the alias as a third-party import.


## Sync
Keep projects in sync with the actual usage of bricks in source code.

``` shell
poetry poly sync
```

This feature is useful for keeping projects in sync. The command will analyze code and add any missing bricks to the projects, including the development project.

- projects: will add missing bricks to the project specific _pyproject.toml_, when imported by any of the already added bricks.
- development: will add __all__ missing bricks to the development _pyproject.toml_.

### Options
`--directory`
Synchronize a specific project.
