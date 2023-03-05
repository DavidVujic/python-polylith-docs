# Commands

## Create a workspace
This will create a Polylith workspace, with a basic Polylith folder structure.

``` shell
poetry poly create workspace --name my_example_namespace --theme loose
```

### Options
`--name` (required) the workspace name, will be used as a top namespace for all bricks.

`--theme` the structure of the workspace (see below).

### Themes
Themes are an exclusive Python Polylith feature, and defines what kind of workspace structure to use.

#### loose (recommended)
A theme to use for a more familiar structure for Python: _components/namespace/package_ and will put a _test_ folder at the root of the repository.

#### tdd
The default and will set the structure according to the original Polylith Clojure implementation, such as:
_components/package/src/namespace/package_ with a corresponding _test_ folder.

## Create a component
This command will create a component - i.e. a Python package in a namespaced folder.

``` shell
poetry poly create component --name my_example_component
```

### Options
`--name` (required) the name of the component.

## Create a base
This command will create a base - i.e. a Python package in a namespaced folder.

``` shell
poetry poly create base --name my_example_base
```

### Options
`--name` (required) the name of the base.

## Create a project
This command will create a project - i.e. a pyproject.toml in a project folder.

``` shell
poetry poly create project --name my_example_project
```

### Options
`--name` (required) the name of the base.

`--description`
Add a brick description. The description will be added as a docstring.
Also in the brick-specific README (when set to enabled in the `resources` section of the workspace config).


## Info
Show info about the workspace:

``` shell
poetry poly info
```

## Diff
Shows what has changed since the most recent stable point in time:

``` shell
poetry poly diff
```

The `diff` command will compare the current state of the repository, compared to a `git tag`.
The tool will look for the latest tag according to a certain pattern, such as `stable-*`.
The pattern can be configured in `workspace.toml`.

The `diff` command is useful in a CI environment, to determine if a project should be deployed or not.
The command has a `--short` flag to only print a comma separated list of changed projects to the standard output.


Useful for CI:
``` shell
poetry poly diff --short
```


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
