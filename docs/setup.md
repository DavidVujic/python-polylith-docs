# Setup

##  Create a Polylith Workspace
Create a directory for your code, initialize it with __git__ and create a basic __Poetry__, __Hatch__ or __PDM__ setup:

``` shell
git init
```

### Poetry
``` shell
poetry init
```

From the [Poetry docs](https://python-poetry.org/docs/cli/#init):
> This command will help you create a pyproject.toml file interactively by prompting you to provide basic information about your package. It will interactively ask you to fill in the fields, while using some smart defaults.


Create a workspace, with a basic Polylith folder structure.

``` shell
poetry poly create workspace --name my_namespace --theme loose
```

`--name` (required) the workspace name, that will be used as the single top namespace for all bricks.
__Choose the name wisely.__ Have a look in [PEP-423](https://peps.python.org/pep-0423/#respect-ownership) for naming guidelines.

`--theme` the structure of the workspace, `loose` is the recommended structure for Python.

Create a virtual environment for the workspace.

``` shell
poetry install
```


### Hatch
``` shell
hatch new --init
```

From the [Hatch docs](https://hatch.pypa.io/latest/cli/reference/#hatch-new):
> Create or initialize a project. 

Note: using `--init` to avoid creating any boilerplate code.

Add the Polylith CLI as a dev dependency, and configuration for the virtual environment in the `pyproject.toml` file:

``` toml
[tool.hatch.envs.default]
dependencies = ["polylith-cli"]
type = "virtual"
path = ".venv"
python = "3.12"  # your preferred version here

[tool.hatch.build]
dev-mode-dirs = ["components", "bases", "development", "."]
```


Create a Hatch virtual environment:
``` shell
hatch env create
```

Create a workspace, with a basic Polylith folder structure.

``` shell
hatch run poly create workspace --name my_namespace --theme loose
```

`--name` (required) the workspace name, that will be used as the single top namespace for all bricks.
__Choose the name wisely.__ Have a look in [PEP-423](https://peps.python.org/pep-0423/#respect-ownership) for naming guidelines.

`--theme` the structure of the workspace, `loose` is the recommended structure for Python.


### PDM
``` shell
pdm init -n --backend pdm-backend minimal
```

From the [PDM docs](https://pdm-project.org/latest/usage/template/)
>  Initialize with the builtin "minimal" template, that only generates a pyproject.toml.

and [PDM init](https://pdm-project.org/2.12/reference/cli/#init)

> Specify the build backend, which implies --dist

#### Add a workspace hook
Make `PDM` aware of the Polylith structure, by adding the `pdm-polylith-workspace` hook to the newly created `pyproject.toml`.

The build hook will add an additional `pth` file to the virtual environment,
with paths to the Polylith source code folders (bases, components).

``` toml
[build-system]
requires = ["pdm-backend", "pdm-polylith-workspace"]
build-backend = "pdm.backend"

```

#### Add the polylith-cli
Add the Polylith CLI as a dev dependency and setup the virtual environment paths.

``` shell
touch README.md

pdm add -d polylith-cli

pdm install

```

Next: create a Polylith workspace, with a basic Polylith folder structure.
The `poly` command is now available in the local virtual environment.
You can run commands in the context of `pdm run` to make Polylith aware of the development environment.

``` shell
pdm run poly create workspace --name my_namespace --theme loose
```

`--name` (required) the workspace name, that will be used as the single top namespace for all bricks.
__Choose the name wisely.__ Have a look in [PEP-423](https://peps.python.org/pep-0423/#respect-ownership) for naming guidelines.

`--theme` the structure of the workspace, `loose` is the recommended structure for Python.


### Rye
``` shell
rye init my_repo  # name your repo

cd my_repo

rye sync  # create a virtual environment and lock files
```

Create a workspace, with a basic Polylith folder structure.

``` shell
rye run poly create workspace --name my_namespace --theme loose
```

`--name` (required) the workspace name, that will be used as the single top namespace for all bricks.
__Choose the name wisely.__ Have a look in [PEP-423](https://peps.python.org/pep-0423/#respect-ownership) for naming guidelines.

`--theme` the structure of the workspace, `loose` is the recommended structure for Python.


#### Edit the configuration
The default build backend for Rye is Hatch. Add the `hatch-polylith-bricks` build hook plugin to the `pyproject.toml` file.
``` toml
[build-system]
requires = ["hatchling", "hatch-polylith-bricks"]
build-backend = "hatchling.build"

[tool.hatch.build.hooks.polylith-bricks]
# this section is needed to enable the hook in the build process, even if empty.
```

Make Rye (and Hatch) aware of the way Polylith organizes source code:
``` toml
[tool.hatch.build]
dev-mode-dirs = ["components", "bases", "development", "."]
```

Run the `sync` command to update the virtual environment:

``` shell
rye sync
```

Finally, remove the `src` boilerplate code that was added by Rye in the first step:
``` shell
rm -r src
```


You will find more documentation about the available commands in the [commands](commands.md) section.
