### Building from Source Code

To build an application from the source code (an example can be found [here] (https://github.com/fhg-fokus-nubomedia/nubomedia-source-build-example)) the following steps are necessary:
* Create you Dockerfile with all the instructions to copy the source code inside a directory (like the ADD my-code-folder /tmp/my-code-folder)
* Move inside the directory and run all the commands required for build
* Declare an ENTRYPOINT with the command required for run.
* Create a git repository (private or public) and put inside your code plus Dockerfile
* Deploy the application on the PaaS-manager with the same procedure, if something on your Dockerfile is wrong the build will fail.

The maven command (and also the jdk 8) is already in the base-image so you don't need to install anything.

### Building from Jar

### Building from Private Repository

### Building from Public Repository
