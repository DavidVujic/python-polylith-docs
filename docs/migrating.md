# Migrating to Polylith

If you decide to give Polylith a try with existing code, the suggestion is to:

- create a new repo and workspace. See [setup](setup.md) and [commands](commands.md).
- choose one of your existing services or apps to migrate into the newly created Polylith repo.
- create a base with the poly tool and put all of the existing Python code in there. Don't worry, we will fix this in next steps.
- create a new project with the poly tool - this will be the project infrastructure for the service or app that currently lives in the _base_ you added in the prevous step.

The entrypoint for your app would be something like:

## Poetry
``` toml
[tool.poetry]
packages = [
    {include = "your_namespace/your_app", from = "../../bases"}
]

[tool.poetry.dependencies]
# insert the needed 3rd party libraries here
```

## Hatch
``` toml
[project]
dependencies = [] # insert the needed 3rd party libraries here

[tool.hatch.build.force-include]
"../../bases/your_namespace/your_app" = "your_namespace/your_app"
```


You should now be able to run the service or app locally.

## Next step: migrating code to bricks
Now you are ready for the actual migration: identifying parts of the source code that could be grouped into what Polylith refers to as components.
Moving them - one by one - out of the base and into new components. Add the components to the `pyproject.toml` according to the example in [Projects & pyproject.toml](projects.md).

### When is the migration finished?
The base would ideally contain the entry point, and import all the needed components.
You can test out the service or app, run the `info`, `libs` and `check` commands to verify all things are in place. See [commands](commands.md).

Continue with your next existing service or app! By now, you should already be able to share components between your services.


## Migrating away from Polylith?
This step is simple.

### Poetry
``` shell
poetry build-project --directory path/to/project
```

### Hatch
``` shell
cd path/to/project

hatch build
```

The output is a `wheel` and, more importantly, an `sdist` (a source distribution). It is essentially a _zip_ file containing all source code used in the project.

That's all!
