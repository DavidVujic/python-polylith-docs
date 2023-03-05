# Home


## What's Polylith?
From the [official docs](https://polylith.gitbook.io/polylith/):

>... Polylith is a software architecture that applies functional thinking at the system scale. It helps us build simple, maintainable, testable, and scalable backend systems. ...

Polylith is an architecture, with tooling support, originally built for Clojure.

This project brings __Polylith to Python!__

### An Architecture well suited for Monorepos
Polylith is using a components-first architecture. Similar to LEGO, components are building blocks.
A component can be shared across apps, serverless functions and microservices.

## Polylith for Python?
The tooling is built as a __Poetry__ plugin. The plugin will add Polylith specific support to Poetry.

### Use cases

#### Microservices and apps
The main use case is to support having one or more microservices (or apps) in a Monorepo, and share code between the services.

#### Libraries
Polylith for Python has support for building libraries to be published at PyPI, even if it isn't the main use case.
By default, the code in one library will share the same top namespace with other libraries that are
built from the same Polylith Monorepo.

There's a simple solution available in the tooling: a feature that organizes code according to
a custom top namespace and will re-write imports in the actual source code.
