# Configuration

The Polylith Workspace is configured using a __workspace.toml__ file at the root of the workspace.

A default configuration is created when running the `poetry poly create workspace` command (see the [commands](commands.md) section).

An example of a workspace configuration:

``` toml
[tool.polylith]
namespace = "my_example_namespace"
git_tag_pattern = "stable-*"

[tool.polylith.structure]
theme = "loose"

[tool.polylith.resources]
brick_docs_enabled = false

[tool.polylith.test]
enabled = true
```


### Components and bases documentation
When `brick_docs_enabeld = true`, a README is added when creating a component or a base.

### Testing
The create component and brick [commands](commands.md) will also create corresponding unit tests when `enabled = true` in the test section of the workspace configuration.
