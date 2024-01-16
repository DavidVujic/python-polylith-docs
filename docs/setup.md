# Setup

## 1. Create a repository
Create a directory for your code, initialize it with __git__ and create a basic __Poetry__ or __Hatch__ setup:

``` shell
git init
```

### Poetry
``` shell
poetry init
```

From the [Poetry docs](https://python-poetry.org/docs/cli/#init):
> This command will help you create a pyproject.toml file interactively by prompting you to provide basic information about your package. It will interactively ask you to fill in the fields, while using some smart defaults.


### Hatch
``` shell
hatch new --init
```

From the [Hatch docs](https://hatch.pypa.io/latest/cli/reference/#hatch-new):
> Create or initialize a project. 

Note: using `--init` to avoid creating any boilerplate code.


## 2. Create a workspace
Ceate a workspace, with a basic Polylith folder structure.

### Poetry
``` shell
poetry poly create workspace --name my_namespace --theme loose
```

### Hatch
``` shell
hatch run poly create workspace --name my_namespace --theme loose
```

`--name` (required) the workspace name, that will be used as the single top namespace for all bricks.
__Choose the name wisely.__ Have a look in [PEP-423](https://peps.python.org/pep-0423/#respect-ownership) for naming guidelines.

`--theme` the structure of the workspace, `loose` is the recommended structure for Python.

## 3. Create a virtual environment
Create a virtual environment for the workspace.

### Poetry
``` shell
poetry install
```

Besides creating a virtual environment, this command will make the virtual environment aware of the `bases` and `components` folders.

### Hatch
Add this to the `pyproject.toml` that was added in the first step to make Hatch aware of the Polylith structure.

``` toml
[tool.hatch.build]
dev-mode-dirs = ["components", "bases", "development", "."]
```

Create a Hatch virtual environment:
``` shell
hatch env create
```

You will find more documentation about the available commands in the [commands](commands.md) section.
