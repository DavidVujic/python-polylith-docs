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

## Hatch, PDM and Rye

No globally added tools needed. Add the project-specific dependencies (see the [Setup](setup.md) and [Projects & pyproject.toml](projects.md) section),
and the build hook plugins to add support for the Polylith structure and when packaging libraries.
