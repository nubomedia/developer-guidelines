# Overview

The NUBOMEDIA Repository API has the objective of exposing the repository capabilities to application developers. By means of this repository capabilities, developer are able to **store and recover** multimedia streams (and metadata) in a cloud environment in a scalable, reliable and secure way. This API also provides fully interoperability with the NUBOMEDIA Media API, so that specific Media Elements are able to record/play media in/from the repository.

The NUBOMEDIA Repository API is build on the top of the [Kurento Repository](http://doc-kurento-repository.readthedocs.org/). The *kurento-repository-sever* can be provided in a seamless way by the NUBOMEDIA PaaS (see [PaaS Manager](../pass/pass-gui) page to find out how to do it).

# NUBOMEDIA Repository Client

The NUBOMEDIA Repository Client (NRC) is a very light weight library that provides to access the NUBOMEDIA cloud repository. The NRC library has been distributed via Maven, and therefore it can be found on [Maven central repository](http://search.maven.org/#search%7Cga%7C1%7Cde.fhg). In order to use it, simply include it on your project's *pom.xml* file as describe below. Notice that the original *kurento-repository-client* dependency is also required.

```xml
<dependencies>
   <!-- Kurento client dependency -->
   <dependency>
      <groupId>org.kurento</groupId>
	  <artifactId>kurento-repository-client</artifactId>
   </dependency>

   <!-- Nubomedia client dependency -->
   <dependency>
      <groupId>de.fhg.fokus.nubomedia</groupId>
      <artifactId>nubomedia-repository-client</artifactId>
      <version>1.0</version>
   </dependency>
</dependencies>
```

!!! info

    We are in active development. Please take a look to the [Maven Central Repository](http://search.maven.org/) to find out the latest version of the artifacts.

With these two dependencies included in our Java project, we are able to create instances of [Kurento Repository Client](http://doc-kurento-repository.readthedocs.org/en/latest/repository_client.html), which is the object in charge of handling the repository server. Inside NUBOMEDIA, a single instance of this object should be created by application, example as follows:

```java
    // One RepositoryClient instance per application
    RepositoryClient repositoryClient = RepositoryClientProvider.create();
```

In order to checkout running examples of NUBOMEDIA applications, please take a look to the **tutorials** section within this documentation. The [nubomedia-repository-tutorial](../tutorial/nubomedia-repository.md) has been specifically designed to understand how to use the NUBOMEDIA Repository API.
