# Configuration

The Polylith Workspace is configured using a __workspace.toml__ file at the root of the workspace.

A default configuration is created when running the `poetry poly create workspace` command (see the [commands](commands.md) section).

An example of a workspace configuration:

``` toml
[tool.polylith]
namespace = "my_example_namespace"

[tool.polylith.structure]
theme = "loose"

[tool.polylith.tag.patterns]
stable = "stable-*"
release = "v[0-9]*"

[tool.polylith.resources]
brick_docs_enabled = false

[tool.polylith.test]
enabled = true
```


### Tags
Check for changes since a tag. Configure the tag pattern in the `workspace.toml` file.

The preferred way of defining tag patterns is:
``` toml
[tool.polylith.tag.patterns]
stable = "stable-*"
release = "v[0-9]*"
```

It is also possible to define one single tag pattern in the `[tool.polylith]` section.
Note: if both exists, the `[tool.polylith.tag.pattern]` will override.

``` toml
[tool.polylith]
git_tag_pattern = "stable-*"
```

### Components and bases documentation
When `brick_docs_enabeld = true`, a README is added when creating a component or a base.

### Testing
The create component and brick [commands](commands.md) will also create corresponding unit tests when `enabled = true` in the test section of the workspace configuration.
