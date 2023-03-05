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
