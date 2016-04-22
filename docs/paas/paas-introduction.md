# Guidelines on Building Application

This section explains the steps necessary in building your application and deploying on the NUBOMEDIA cloud platform.

# Building from Source Code

To build an application from the source code the following steps are necessary:

- Create you Dockerfile with all the instructions to copy the source code inside a directory (like the ```ADD``` my-code-folder /tmp/my-code-folder)

- Move inside the directory and run all the commands required for build

- Declare an ENTRYPOINT with the command required for run.

- Create a Git repository (private or public) and put inside your code plus Dockerfile

- Deploy the application on the PaaS-manager with the same procedure, if something on your Dockerfile is wrong the build will fail.

Maven and JDK 8 is already in the base-image so you don't need to install anything.

An example of this kind of deployment can be found [here](https://github.com/fhg-fokus-nubomedia/nubomedia-source-build-example)). In addition, the tutorials described in this documentation follow also this approach: [magic-mirror](./tutorial/nubomedia-magic-mirror.md), [repository](./tutorial/nubomedia-repository.md), [room](./tutorial/nubomedia-room.md).

# Building from Jar

To build an application from the jar (an example can be found [here](https://github.com/acheambe/nubomedia-app)) the following steps are necessary:

- In order to deploy a NUBOMEDIA application use NUBOMEDIA APIs to create an application (JAR file)

- Create a Dockerfile that describes the way of running the application in the platform

- Upload JAR and Dockerfile to a GitHub repository

- Deploy the application using the PaaS GUI or the REST API

# Building from Public Repository

Build and package you application, make the jar package available on a public Git repository.

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

- ```nubomedia/apps-baseimage:v1``` is the Nubomedia base image to build and run the application, is based on alpine linux with java 8 and bash installed; it also have is internal packet manager to install other packages. We choose that because with ubuntu-based images we had build problems and very low performances

- ```MAINTAINER``` is the developer of the application (and the author of the Dockerfile)

- ``` RUN ``` is a Docker instruction that execute a command given as parameter, any misspelling or wrong parameters will result in a build failure

- ```ADD``` is a simple cut and paste from base directory to given directory

- ```EXPOSE``` is used to declare the ports that will be used from the docker container, which will also be used by OpenShift

- ```ENTRYPOINT``` is the command that docker will execute after the creation of the container.

For an extensive list of Docker commands and references use the following resources:

- [Docker Commands](https://docs.docker.com/v1.8/reference/builder/)
- [Docker Best Practices](https://docs.docker.com/engine/articles/dockerfile_best-practices/)

# Building from Private Repository

The procedure to build from a private repository is similar to the procedure for deploying from a public repository, with just one additional step the user needs to carry out. You will need to send an HTTP request to the PaaS Manager to create a secret. This secret is used to securely clone your repository during the build process.

## Creating a Secret

You can create a secret a secret a REST client via graphical interface or curl using an HTTP POST on the URL  ```http://[URL]/api/v1/nubomedia/paas/secret``` with these json object as body:

```
"projectName":"my-project",
	"privateKey":"-----BEGIN RSA PRIVATE KEY-----\n
	MIIEowIBAAKCAQEAxBSEyhfttjfqesNc8/NNgYp93hDSetbWWi3/v48FhZebU6P/\n
	wx2C5XtrOo5Co8R98loqCh+Dw3OoH6HhPBqSq7JQYqEU3MakaZ4Sh/19rziPPA1g\n
	mKU9GEXhogu415vQz1sD/heXDQFNZdUokcYFNrbfGpfmsBEdkcVzPLBN4GJP1wCu\n
	M7oWzwOhLbqh98ib8WadqDBiRqztal5h2Xa7wWsgulDkaCvtOPlIm7W/TICyUXOd\n
	NH811dJ4EhqmbJQZlOzsuA2js6Vz58HqV8ytYiVdkQnsP5jLzPNM0nkSh3XWeube\n
	0zhuEJmh0wS9BmsipKa9VIXNH2/mnufq28wMoQIDAQABAoIBABUn3ZfscwJpEAyE\n
	zZ+ojaE/bwspp3wHeAMs2V4ysTbTv7eLh0nnAjt+UHh15uzCg5BFeCm1csMA1I/t\n
	KF8SwuZxi8jIdnbHm++lVXyEti3UnWeuTdDKa0gWKh0QxLXGowXsXQbqRqrpjA9D\n
	q2fnBKL9oh69au9uOVGEC0XuA8kEwg7aJ39Dcr543UJj/QfeNoaJtw1LA99dag1/\n
	G4is9dpBRPU4uIQo37CBLf1TbpYTQNL+46WYHhz72p/BIAoIv+kSdgsrRvyV/JeP\n
	cTcazMUJrXSxDZzWmfr32yqtQRu1nU+uckisDwpqgUuhfnoKkoU4BH1uKxW+tzK6\n
	XktqJlECgYEA601pR2HGspgcZcNv7zeWRGxWbMngfzC/W33sCsTlqvLcP+gq6bJX\n
	4PB7ulLCDII2MEnF05+Fh8pM+egUgr7Y+B3AFrOJDg2tIlXu8Ro3Uwoxd28oO61Z\n
	awdvkuCZ9Qeue9mPYVGIjQoY84UbJlbXjfiF+Huylx24VeThOjCYZm0CgYEA1VPk\n
	k42r7I/61Ar+SscHyaT8lUMssMjENa2y7Wh4WvH/ZKirgo2E1q/uPdtrP3YfP154\n
	gf0bbsrl6zm1oPB/y0UOGC0XWc7F6nn23C9ax27ICavo62g06+t/qqW4BcAfp8VN\n
	IxmyNNtvo56CuoI1+PcyMZaVbYwPgecwQHGMboUCgYEApR8PsB33N8DyvJ7nX/Gc\n
	K6vzAiiwt9DXmDbHe88sdEg1M0uTQaf7b0iTKu+EaQ6/RCehAZ7CL8ZROlYYfp+6\n
	1nLaJ5QZq5kBVEUFhoAlLsrKZ8vDag194FO5glLG92JKmXLU4TA8KO1bERjpMoBi\n
	h6hNK1ByxQUAJJaXTyRm7gkCgYBrTbCK+9b/vgh4AjOY33YuWnvmhIyFO+dd7Mo0\n
	mrj3XgSN2D21BIRODN50ZNsUZ9Ed6eIJ2Iuk9hAieru+gVp2n3yQcpXtSZHJ+KFQ\n
	bc1mxXV/T+ZwCtGb3bAw4Pyof9QsapT7U+CMr9f+4Ct3rymA2q53vPvax3nBaM2f\n
	jL4LlQKBgE/VkGclp53bBagi6A+AiM6U2oYqgFSyyNdLYI6N+5zghbERd2zvF9Vu\n
	fjCSbyGgpvyJw2cPTF+MwiYvfuRBGW6JNfk2QhdoFHWIZ0Zis2cGs3o/wHCW5K6/\n
	XSYNlaIoCsgWHXHPUaVgWui2B51nfnu31B5tUYAmjXZc42Miz+ml\n
	-----END RSA PRIVATE KEY-----"
}
```

!!! info

    Private key value must preserve newlines, use ```\n``` to preserve it.

The above is an example of what the private RSA Key looks like. Replace it with your generated private key. For more information on generating RSA private key, check the [GitHub documentation](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).

The GUI (or the Http request) will return the ```secretname``` that has to be used when you want to deploy the application as described in [deploying via PaaS GUI](https://github.com/nubomedia/developer-guidelines/blob/develop/docs/paas/paas-gui.md) or in [deploying via PaaS API](https://github.com/nubomedia/developer-guidelines/blob/develop/docs/paas/paas-api.md).
