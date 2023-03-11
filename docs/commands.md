# Commands

<img width="640" alt="poly-info-example" src="https://user-images.githubusercontent.com/301286/203044825-b84371e8-caa8-4f85-8c94-4a5f3af887d6.png">

## Create a workspace
This will create a Polylith workspace, with a basic Polylith folder structure.

``` shell
poetry poly create workspace --name my_example_namespace --theme loose
```

### Options
`--name` (required) the workspace name, that will be used as the single top namespace for all bricks. __Choose the name wisely.__

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
