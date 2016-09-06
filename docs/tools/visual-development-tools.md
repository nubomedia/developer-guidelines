# Visual Development Tool

The NUBOMEDIA Visual Development Tool is composed of two different applications:

- The **Graph Editor** is a desktop application that lets a developer visually create and edit component graphs and save them as json-based files with the `.ngeprj` file extension.

- The **Code Generator** is a command-line application that takes `.ngeprj` files and generates source code to implement these graphs in new applications.

The overall architecture is similar to that of [Flux](https://facebook.github.io/flux/), which is an Application Architecture for Building User Interfaces, built by Facebook. It is a set of patterns building larger applications on top of the React component library.

## Graph Editor

The Graph Editor is an HTML app based on Facebook's [React](https://facebook.github.io/react/) and Dan Abramov's [Redux](https://github.com/reactjs/redux) libraries. For more detailed information please visit the [Graph Editor documentation](http://nubomedia-graph-editor.readthedocs.org/).
