# Packaging & deploying

## Poetry
Packaging and deploying Polylith projects is done by using the Poetry Multiproject plugin command (see [installation](installation.md)).

The `poetry build-project` command will make it possible to use relative package includes as how components and bases are added to Python Polylith projects. 
Relative includes are currently not possible by default in __Poetry__, that is where the __Multiproject__ plugin comes in.

## Hatch and PDM
Hatch and PDM support relative includes via the `[tool.poetry.bricks]` configuration. Nothing extra needed other than the build hooks.

## Packaging
To collect the components and bases that are needed for a specific project, the tool introduces a _build_ step. 
The tool will build a _wheel_ and an _sdist_ from the source code of a project.

This is the preferred way for Polylith projects.

### Packaging a service or app

#### Poetry
``` shell
poetry build-project --directory path/to/project
```

#### Hatch
``` shell
cd path/to_project

hatch build
```

#### PDM
``` shell
cd path/to_project

pdm build
```

This command will create a project specific _dist_ folder containing a _wheel_ and an _sdist_.
You can use the available __build__ options with this command too.

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

#### Hatch and PDM
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
