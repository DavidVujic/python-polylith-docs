# Packaging & deploying

## Poetry
Packaging and deploying Polylith projects is done by using the Poetry Multiproject plugin command (see [installation](installation.md)).

The `poetry build-project` command will make it possible to use relative package includes as how components and bases are added to Python Polylith projects. 
Relative includes are currently not possible by default in __Poetry__, that is where the __Multiproject__ plugin comes in.

## Hatch, PDM, Rye and uv
Hatch, PDM, Rye and uv support relative includes via the `[tool.poetry.bricks]` configuration.
Nothing extra needed other than the build hooks.

### Building source distributions (sdist)?
If you will distribute an `sdist` as the primary way to install your package,
you will need to add the path in the project-specific `pyroject.toml`.

If you only provide `wheel` distributions, this is optional.

#### Hatch, Rye and uv
```toml
[tool.hatch.build.targets.wheel]
packages = ["<your top namespace here>"]
```

#### PDM
```toml
[tool.pdm.build]
includes = ["<your top namespace here>/"]
```

#### Maturin
``` toml
[tool.maturin]
include = ["<your top namespace here>/**/*"]
```

## Packaging
To collect the components and bases that are needed for a specific project, the tool introduces a _build_ step. 
The tool will build a _wheel_ and an _sdist_ from the source code of a project.

This is the preferred way for Polylith projects.

### Packaging a service or app

#### Poetry
``` shell
poetry build-project --directory projects/the_project
```

#### Hatch
``` shell
cd projects/the_project

hatch build
```

#### PDM
``` shell
cd projects/the_project

pdm build
```

#### Rye
``` shell
cd projects/the_project

rye build --wheel
```

#### uv
``` shell
uv build --wheel projects/the_project
```

_Are you using the `uv` build backend (and not the recommended `hatch` build backend)?_
_If so, you need to run the `poly build setup` and `poly build teardown` commands before and after the `uv build` command._
_This is needed because of the missing build hook support of the uv build backend._

#### Maturin
``` shell
cd projects/the_project

# if not already activated a virtual environment
source .venv/bin/activate

poly build setup

maturin build

poly build teardown
```

This command will create a project specific _dist_ folder containing a _wheel_ with all the needed bricks.

## Deploying
You can use the built artifacts to install your service in your preffered way, just by running

``` shell
pip install the-built-artifact.whl
```

### Packaging a Library

The _Python tools for the Polylith Architecture_ has support for building libraries to be published at PyPI,
even if it isn't the main use case.

Important note: by default, the code in one library will share the same top namespace with other libraries that are
built from the same Polylith Monorepo. To solve this, there's a feature available that will organize code according to a custom top namespace and re-write the imports.

#### Poetry
You can choose a custom namespace to be used in the build process, by using the `--with-top-namespace` flag.
This is available for __Python 3.9__ and above.

The `build-project` command, with a custom top namespace:

```shell
poetry build-project --with-top-namespace my_custom_namespace
```

#### uv, Hatch, PDM, Rye and Maturin
A custom top namespace is defined in the project-specific `pyproject.toml`:

``` toml
[tool.polylith.build]
top-namespace = "my_custom_namespace"
```

#### Result
By using the Poetry build-project flag or the Hatch Build Hook, the built artifact will look something like this:
```shell
my_custom_namespace/
   /the_namespace
       /the_brick
           __init__.py
           my_module.py
```

And the Python modules will have the custom top namespace as a prefix to imports:
```python
from my_custom_namespace.the_namespace.the_brick import my_function
```

##### How is this done?
The command uses AST (Abstract Syntax Tree) parsing to modify source code.
The Python built-in `ast` module is used to parse and un-parse Python code.


#### Script Entrypoints
 If you have a script entrypoint,
remember to manually add the custom namespace in your `pyproject.toml`.

Example (Poetry):

``` toml
# manually adding the my_custom_namespace at the beginning

[tool.poetry.scripts]
my_script = "my_custom_namespace.the_namespace.the_brick.my_module:main"
```
