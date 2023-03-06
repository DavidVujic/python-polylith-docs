# The Polylith Workspace

The  workspace is the root directory of a repository.
The basic structure of a Polylith Workspace would look something like this:

``` shell
workspace/
  bases/
  components/
  development/
  projects/
  
  poetry.lock

  pyproject.toml
  workspace.toml
  
  README.md
```

The building blocks are found in the _bases_ and _components_ folders. That's where the Python code is put.

## bases
> Bases are the building blocks that exposes a public API to the outside world. [^1]

A base is a special kind of Polylith _brick_. It is where you would add the entry point of an app or a REST API.
It is a bridge between the outside world and the actual features.

## components
> A component is an encapsulated block of code that can be assembled together with a base (it's often just a single base) and a set of components and libraries into services, libraries or tools. Components achieve encapsulation and composability by separating their private implementation from their public interface. [^1]

A component is also a _brick_, a building block for a feature. 
It can be a "tech" brick, such as containing dictionary filtering helper functions, or a parser.
It can also be a wrapper for a third-party library. A brick is also very likely a combination of other bricks.
The combination of bricks becomes a feature. Very much like LEGO.

## development
The development folder is where you can put code that you write to experiment or try out features.
It is similar to the __scratch__ files in JetBrains PyCharm!

The development folder is part of the _development project_, that is defined in the `pyproject.toml` and the `poetry.lock` file of the root folder.
Here, you add __all__ dependencies and bricks. This will make it possible to have the entire code-base available in one and the same virtual environment.

In this folder, it is quite common that developers keep their scratch-style Python modules. It is perfectly fine to version control them.
It is a place for REPL Driven Development and Jupyter Notebooks!


``` shell
development/
   sofia.py
   david.py
   data.ipynb
```

## projects
> A project is the result of combining one base (or in rare cases several bases) with multiple components and libraries. [^1]

Each project lives in a subdirectory of the `projects/` folder. A project is the deployable artifact: a microservice, a serverless function, a CLI or any kind of application.

``` shell
projects/
   my_aws_lambda/
   my_fast_api_service/
```

In the project-specific directory, you will find a project-specific `pyproject.toml` that defines dependencies, packages and everything needed for the actual project.
It is not recommended to put Python code in here. Just add the necessary the project infrastructure, such as Dockerfiles and project-specific deploy scripting.

[^1]: From the official [Polylith Architecture documentation](https://polylith.gitbook.io/polylith/)
