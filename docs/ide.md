# Configuring your Python IDE

## MyPy

```toml
[mypy]
mypy_path = components, bases
namespace_packages = True
explicit_package_bases = True
```

## Pyright

`.vscode/settings.json`

```json
{
  "python.analysis.extraPaths": [
    "bases",
    "components"
  ]
}
```

`pyproject.toml`

```toml
[tool.pyright]
extraPaths = ["bases", "components"]
```


## .venv
It is recommended to create the virtual environment locally, for a great code editor experience.
By default, `Poetry` will create a `venv` outside of the repo. You can override that behaviour by adding a configuration in a `poetry.toml` file:

``` toml
[virtualenvs]
path = ".venv"
in-project = true
```
