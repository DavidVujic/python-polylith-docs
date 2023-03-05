# Home


## What's Polylith?
From the [official docs](https://polylith.gitbook.io/polylith/):

>... Polylith is a software architecture that applies functional thinking at the system scale. It helps us build simple, maintainable, testable, and scalable backend systems. ...

Polylith is an architecture, with tooling support, originally built for Clojure. This project brings __Polylith to Python!__

### An Architecture well suited for Monorepos
Polylith is using a components-first architecture. You can think of it as building blocks, very much like LEGO bricks.
The Python code is separated from the infrastructure and the building of artifacts. This may sound complicated, but it isn't.

In short, Polylith is about:

- Viewing code as bricks, that can be combined into features
- Making it easy to reuse code across apps, tools, serverless functions and services
- Keeping it simple

### Structure for simplicity
Organizing, sorting and structuring things is difficult. Is there one folder structure to rule them all?

> There should be one-- and preferably only one --obvious way to do it. [^1]

A good folder structure is one that makes it simple to reuse existing code and makes it easy to add new code.
You shouldn't have to worry about these things. The Polylith Architecture offers a way to organize code that is simple,
framework agnostic and scalable as projects grow.


### It is all about the bricks
The main takeaway is to view code as small, reusable bricks, that ideally does one thing only.
A brick is not the same thing as a library. So, what's the difference? A library is a full blown feature. A brick can be a single function, or a parser. It can also be a thin wrapper around a third party tool.

> Simple is better than complex. [^1]


In Python, a file is a module. One or more modules in a folder becomes a package.
A good thing with this is that the code will be namespaced when importing it.
Where does the idea of bricks fit in here? Well, a brick is a namespaced Python package. Simple as that.

> Namespaces are one honking great idea -- let's do more of those! [^1]


## Polylith for Python?
The __Python tools for the Polylith Architecture__ is built as a __Poetry__ plugin. The plugin will add Polylith specific support to Poetry.

### Use cases

#### Microservices and apps
The main use case is to support having one or more microservices (or apps) in a Monorepo, and share code between the services.

#### Libraries
Polylith for Python has support for building libraries to be published at PyPI, even if it isn't the main use case.
More details about how to package libraries in [Packaging & deploying](deployment.md).


[^1]: From the Zen of Python
