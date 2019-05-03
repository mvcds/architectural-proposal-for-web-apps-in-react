# Architectural Proposal for Web Apps in React

## Postulates

* "Software architecture" is the set of costly technical decisions which binds software elements and their relationships in a way that enables the code to be designed towards the software's goals
* Architectural decisions should be documented in a place close to the code
* Each architectural element should be self-contained i.e. it should be possible to move it to another project using same dependencies than original project without touching the code - not that we're planning to do it, but this means that each element's code does not interfere with each other
* An architectural element may have files and subfolders which are not listed as part of the proposal, once that sub-elements become necessary based on what is being built
* If an architectural element is not needed, it should not be created, as well as empty folders due to this e.g. `cli` folder is not not always necessary; In case of removal, recursive removal should be applied
* As each project may have different goals, and lessons are learnt as time goes by, decisions of this document might be overwritten if the team believes it helps the project. Such changes should be documented
* A module is a set of plugable functionalities tied to some related business capabilities
* Code should only "promoted" to reusable if similar/equal code is detected in at least three different parts, so we avoid premature optimizatins

## Inspiration

The [clean architecture](https://medium.freecodecamp.org/a-quick-introduction-to-clean-architecture-990c014448d2) is a N-layer architecture disposed in circles within circles. The closer to the center, the more abstract and fixed is the layer, whereas the further to the center, the more concrete and dynamic is the layer.

<img src="https://cdn-images-1.medium.com/max/1200/0*GtcSDT7dNFshDM7c" />

Each circle may have different components which are allowed to interact to each other BUT each layer is only allowed to interact to the one immediately within it. This way the more close to the center, more "isolated" of external system the layer becomes.

The core of the software is basically tied to business needs (the domain and its use cases), data storage and user interface are simply details of the project which enable users to interact with the software.

Using modules as to mimic even smaller programs means that the described structure will be repeated in each one of them.

## Filestructure

Inspired by [react clean architecture](https://github.com/eduardomoroni/react-clean-architecture)

### README

Explains how to build, run and deploy app.

It also contains information about the development phase like merging strategy and project conventions which could not be enforced by automation - which is preferable when possible.

### CHANGELOG

Filled automatically by a program after each deploy.

### docs

Contains all technical decisions which deviate from this document.

### (development-folders-and-files)

"Configuration" files for tools used by us, developers. We shouldn't be coding that much here - we should be, more like, tweaking values.

### src

Your code goes (mainly) here =D

#### infra

Where adapters live. It mostly means third-party services, like firebase for data management.

Each adapter offers the basic interfaces to be used by the applications. As we don't expose the real service, it is possible to change its content without affecting (much) consumers.

Other things like management of environment variables and standarized network calls also might live here.

#### web

Part of the UI which runs on browsers, thus is slightly tied to HTML/CSS/JS triad.

##### assets

Non-code artefacts, with one subfolder for each extension.

##### components

Reusable code following [atomic design](http://bradfrost.com/blog/post/atomic-web-design/) to some extent.

The proposed extent is to leave "pages" out of this folder, as for each module define their own.

HOCs should also be placed here.

##### modules

A "discoverable" part of the application which has a "route.jsx" file (which the program discovers) pointing to some pages - which can be acessible through URL or not, depends on what is needed.

###### domain

Based on [DDD](https://en.wikipedia.org/wiki/Domain-driven_design).

Each module is quite equivalent to a bounded context.

###### pages

Each page (and its sub-components) starts with its visual, and then either state or external tools are added to it. The index file points to the "top-level" component.

When necessary, the page may use a `view-model` to help encapsulate the desired behaviour, it acts like a DDD's aggregate root.

###### application

It concentrates all the module's use cases in one place so that it is not repeated elsewhere.

Business exceptions are thrown from this component.

###### adapters

Necessary files to the module to work, they define how to use the equivalent adapter from `infra` folder.

##### stylesheets

A composable set of styles according to [itcss](https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/) componentization. The result is used by every component's style.

## Other relevant info

* [Good abstract about DDD building blocks](https://blog.lelonek.me/ddd-building-blocks-for-ruby-developers-cdc6c25a80d2)
