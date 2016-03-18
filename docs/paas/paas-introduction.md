# Guidelines on Building Application
This section explains the steps necessary in building your application and deploying on the NUBOMEDIA cloud platform. 

# Building from Source Code

To build an application from the source code (an example can be found [here] (https://github.com/fhg-fokus-nubomedia/nubomedia-source-build-example)) the following steps are necessary:
* Create you Dockerfile with all the instructions to copy the source code inside a directory (like the ADD my-code-folder /tmp/my-code-folder)
* Move inside the directory and run all the commands required for build
* Declare an ENTRYPOINT with the command required for run.
* Create a git repository (private or public) and put inside your code plus Dockerfile
* Deploy the application on the PaaS-manager with the same procedure, if something on your Dockerfile is wrong the build will fail.

The maven command (and also the jdk 8) is already in the base-image so you don't need to install anything.

# Building from Jar
To build an application from the jar (an example can be found [here] (https://github.com/acheambe/nubomedia-app)) the following steps are necessary:

* In order to deploy a NUBOMEDIA application use NUBOMEDIA APIs to create an application (JAR file)
* Create a Dockerfile that describes the way of running the application in the platfom
* Upload JAR and Dockerfile to a GitHub repository
* Deploy the application using the PaaS GUI or the REST API

# Building from Public Repository
Build and packetize you application, make the jar package available on a public git repository.

## Write a Dockerfile
Write a Dockerfile to install and run the application on the PaaS, the file has to be located in the same repository of the application. Here is an example of a [sample nubomedia app](https://github.com/acheambe/nubomedia-app):

```
FROM nubomedia/apps-baseimage:v1

MAINTAINER Nubomedia

RUN mkdir /tmp/magic-mirror
ADD nubomedia-magic-mirror-6.2.2-SNAPSHOT.jar /tmp/magic-mirror/
ADD keystore.jks /

EXPOSE 8080 8443 443

ENTRYPOINT java -jar /tmp/magic-mirror/nubomedia-magic-mirror-6.2.2-SNAPSHOT.jar

```

Here are some explanations from the example above.

* ```nubomedia/apps-baseimage:v1``` is the Nubomedia base image to build and run the application, is based on alpine linux with java 8 and bash installed; it also have is internal packet manager to install other packages. We choose that because with ubuntu-based images we had build problems and very low performances
* ```MAINTAINER``` is the developer of the application (and the author of the Dockerfile)
* ``` RUN ``` is a Docker instruction that execute a command given as parameter, any misspelling or wrong parameters will result in a build failure
* ```ADD``` is a simple cut and paste from base directory to given directory
* ```EXPOSE``` is used to declare the ports that will be used from the docker container, which will also be used by OpenShift
* ```ENTRYPOINT``` is the command that docker will execute after the creation of the container.

For an extensive list of Docker commands and references use the following resources:
* [Docker Commands](https://docs.docker.com/v1.8/reference/builder/)
* [Docker Best Practices](https://docs.docker.com/engine/articles/dockerfile_best-practices/)

# Building from Private Repository
TODO

