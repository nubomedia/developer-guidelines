# Visual Development Tool

The NUBOMEDIA Visual Development Tool is composed of two different applications:

- The Graph Editor is a desktop app that lets a developer visually create and edit component graphs and save them as json-based files with the `.ngeprj` file extension.

- The Code Generator is a command-line app that takes `.ngeprj` files and generates source code to implement these graphs in new applications.

The overall architecture is similar to that of Flux, as illustrated in the following figure:

![Flux architecture](../img/visual-tool-arch.png)

*Flux architecture*

Flux is an "Application Architecture for Building User Interfaces", built by the team at Facebook. It is a set of patterns building larger applications on top of the React component library.
