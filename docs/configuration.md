# Configuration

The Polylith Workspace is configured using a __workspace.toml__ file at the root of the workspace.

A default configuration is created when running the `poly create workspace` command (see the [commands](commands.md) section).

> :material-information: As an alternative, you can put the configuration in the __top__ `pyproject.toml`.

Example of a workspace configuration:

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
Check for changes since a tag. Configure the tag pattern.

The preferred way of defining tag patterns is:
``` toml
[tool.polylith.tag.patterns]
stable = "stable-*"
release = "v[0-9]*"
```

By default, Polylith uses the `-committerdate` when fetching tags with the underlying `git` command.
The sorting might be incorrect when using _annotated tags_. To solve this, you can configure
the sorting.

Example, setting a different sorting option than the default:
``` toml
[tool.polylith.tag]
sorting = ["-creatordate"]
```

### Components and bases documentation
When `brick_docs_enabeld = true`, a README is added when creating a component or a base.

### Testing
The create component and brick [commands](commands.md) will also create corresponding unit tests when `enabled = true` in the test section of the workspace configuration.


### Custom command output path
The `poly info`, `poly deps` and `poly libs` commands have support for a `--save` option.
By default, the command output will be saved to `development/poly/<the command>.txt`.
But you can configure it, if you want to store the output in a different location.

Example configuration:

``` toml
[tool.polylith.commands]
output = "somewhere/else"
```

Or, configure output location by command:

``` toml
[tool.polylith.commands.deps]
output = "somewhere/else"
```
