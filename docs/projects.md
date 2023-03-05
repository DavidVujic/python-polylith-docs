# Projects & pyproject.toml

Projects are located in the _projects_ folder of a Polylith workspace. Each project has its own `pyproject.toml`,
where dependencies and project-specific things are defined. Just as in a mainstream __Poetry__ project.

What differs is how the Polylith components and bases (aka bricks) are referenced. Bricks are added in the _tool.poetry_ section as _packages_:

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
