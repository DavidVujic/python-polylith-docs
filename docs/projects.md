# Projects & pyproject.toml

Projects are located in the _projects_ folder of a Polylith workspace. Each project has its own `pyproject.toml`,
where dependencies and project-specific things are defined. Just as in a mainstream __Poetry__ or __Hatch__ project.

What differs is how the Polylith components and bases (aka bricks) are referenced. 

## Poetry
Bricks are added in the _tool.poetry_ section as _packages_:

``` toml
[tool.poetry]
name = "my_example_project"
packages = [
         {include = "my_namespace/my_base", from = "../../bases"},
         {include = "my_namespace/my_component", from = "../../components"},
         {include = "my_namespace/my_other_component", from = "../../components"}
]
```

Note the _from_ attribute, where the base and components are referenced with relative paths.
The `bases` and `components` folders are located at the workspace root.
The project-specific `pyproject.toml` file is located in a subfolder of the `projects` folder.

## Hatch

### The pyproject.toml in the Workspace (i.e. the one in the root folder)
Add the `polylith-cli` to the workspace `pyproject.toml` configuration.
``` toml
[tool.hatch.envs.default]
dependencies = ["polylith-cli"]
```
Add configuration for a local virtual environment:
``` toml
[tool.hatch.envs.default]
type = "virtual"
path = ".venv"
python = "3.12"  # your preferred version here
```

Make Hatch aware of the Polylith structure:

``` toml
[tool.hatch.build]
dev-mode-dirs = ["components", "bases", "development", "."]
```

### The project-specific pyproject.toml file(s)
Bricks are added in the _tool.hatch.build.force-include_ section:

``` toml
[tool.hatch.build.force-include]
"../../bases/my_namespace/my_base" = "my_namespace/my_base"
"../../components/my_namespace/my_component" = "my_namespace/my_component"
"../../components/my_namespace/my_other_component" = "my_namespace/my_other_component"
```

The `bases` and `components` folders are located at the workspace root.
The project-specific `pyproject.toml` file is located in a subfolder of the `projects` folder.
