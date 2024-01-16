# Installation

## Poetry
### Add Poetry plugins
With the `Poetry` version 1.2 or later installed, you can add plugins. First, add the [Multiproject](https://github.com/DavidVujic/poetry-multiproject-plugin) plugin,
that will enable the very important __Workspace__ support to Poetry.
``` shell
poetry self add poetry-multiproject-plugin
```

Add the Polylith plugin:
``` shell
poetry self add poetry-polylith-plugin
```

Done!

## Hatch

No external tools needed other than project-specific dependencies (see the [Projects & pyproject.toml](projects.md) section),
and the (soon released) build plugin to add support for packaging libraries.
