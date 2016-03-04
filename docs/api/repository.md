# Overview
This is a very light weight library that provides the following functionality to NUBOMEDIA applications:
* CloudRepositoryProfile that contains the information to access the NUBOMEDIA cloud repository
* Implementation of Kurento repository RepositoryUrlProvider interface to obtain the IP address of the server 


# Getting Started
This section explains where to obtian the NMC and how to include it on your project. The assumption here is you are a maven Guru. If not, there are plenty of tutorials online to get you started.

The NUBOMEDIA Repository Client (NRC) is distributed via Maven can be found on [Maven central repository](http://search.maven.org/#search%7Cga%7C1%7Cde.fhg).
Simply include it on your project's pom.xml file as describe below, then run the command ```mvn install```.

```
<dependencies>
...
<!-- kurento-repository -->
	<dependency>
		<groupId>org.kurento</groupId>
		<artifactId>kurento-repository-client</artifactId>
		<version>6.2.1</version>
</dependency>
<dependency>
  <!-- Nubomedia repository client dependency -->
		<groupId>de.fhg.fokus.nubomedia</groupId>
	  <artifactId>nubomedia-repository-client</artifactId>
	  <version>1.0</version>
	</dependency>
</dependencies>
```
*NOTE: At the time of writing the release version is 1.0. This might change as development evolves, so make sure you have the right version (latest) and replace the version number accordingly.*

# Prerequisite
Make sure to include the line below in your config.properties file found in [user.home]/.kurento/. If this file or folder doesn't exist, go ahead and create it.

```
kms.url.provider=de.fhg.fokus.nubomedia.kmc.KmsUrlProvider
````
This interface specifies a single function ```getRepositoryUrl()``` which returns the address on which the cloud Kurento client can be reached.
