# Overview

The NUBOMEDIA Media API has the objective of exposing NUBOMEDIA Media Capabilities through a pipelining mechanism. The NUBOMEDIA Media API is based on the concept of **Media Element**. A Media Element holds a specific media capability. For example, the media element called *WebRtcEndpoint* holds the capability of sending and receiving WebRTC media streams, the media element called *RecorderEndpoint* has the capability of recording into the file system any media streams it receives.

From the application developer perspective, Media Elements are like Lego pieces: you just need to take the elements needed for an application and connect them following the desired topology. In Kurento jargon, a graph of connected media elements is called a **Media Pipeline**. Hence, when creating a pipeline, developers need to determine the capabilities they want to use (the media elements) and the topology determining which media elements provide media to which other media elements (the connectivity).

NUBOMEDIA inherits the rich toolbox provided by Kurento. For further information please visit the [Kurento documentation](http://doc-kurento.readthedocs.org/en/stable/introducing_kurento.html).

# NUBOMEDIA Media Client (NMC)

In order to deploy your application on the NUBOMEDIA Cloud Platform, there a specific NUBOMEDIA library which should be include as dependency to your projects. This library is called **NUBOMEDIA Media Client (NMC)**, and provides the base functionality to compliment the Kurento Client for auto discovery of the Kurento Media Server inside the NUBOMEDIA PaaS. NMC extend the Kurento Client library  with functionalities on how to obtain network resources (for example the IP address of the Kurento Media Server). 

In order to obtain the NMC, it can be done by means of [Maven](https://maven.apache.org/) in a Java project. The NMC is distributed can be found on [Maven central repository](http://search.maven.org/#search%7Cga%7C1%7Cde.fhg). Simply include it on your project's *pom.xml* file as describe below. Notice that the original Kurento Client library is also required:

```xml
<dependencies>
   <!-- Kurento client dependency -->
   <dependency>
      <groupId>org.kurento</groupId>
	  <artifactId>kurento-client</artifactId>
   </dependency>

   <!-- Nubomedia client dependency -->
   <dependency>
      <groupId>de.fhg.fokus.nubomedia</groupId>
      <artifactId>nubomedia-media-client</artifactId>
      <version>1.0.2</version>
   </dependency>
</dependencies>
```

<br>

!!! info

    We are in active development. Please take a look to the [Maven Central Repository](http://search.maven.org/) to find out the latest version of the artifacts.

With these two dependencies included in our Java project, we are able to create instances of [Kurento Client](http://doc-kurento.readthedocs.org/en/stable/introducing_kurento.html#kurento-api-clients-and-protocol), which is the object in charge of creating Media Pipelines and Media Elements. Inside NUBOMEDIA, the instances of KMSs are elastically managed by the platform, scaling in and out depending on the number of Kurento Client instances. All in all, for each media session of a NUBOMEDIA application, an instance of Kurento Client should be created, as follows:

```java
    // One KurentoClient instance per session
    KurentoClient kurentoClient = KurentoClient.create();
```
For a running example of the NUBOMEDIA applications, please take a look to the **tutorials** within this documentation (the [nubomedia-magic-mirror](../tutorial/nubomedia-magic-mirror.md) is a good start to understand how to use the NUBOMEDIA Media API).

## KMS Auto Discovery Process

Kurento Client discovers KMS with the following procedure:

1. If there are a system property with the value “kms.url”, its value will be returned
1. If the file ```~/.kurento/config.properties``` doesn’t exist, the default value ```ws://127.0.0.1:8888/kurento``` will be returned
1. If the file ```“~/.kurento/config.properties”``` exists:
  1. If the property “kms.url” exists in the file, its value will be returned. For example, if the file has the following content:
  ```
  kms.url: ws://4.4.4.4:9999/kurento
  ```
  The value “ws://4.4.4.4:9999/kurento” will be returned.
  
  2. If the property “kms.url.provider” exists in the file, it should contain the name of a class that will be used to obtain the KMS url. 
  In this case:
 
  ```
  kms.url.provider:de.fhg.fokus.nubomedia.kmc.KmsUrlProvider
  ```
  The class ```de.fhg.fokus.nubomedia.kmc.KmsUrlProvider``` will be instantiated with its default constructor. 

This class is a class in the NMC library which implements the interface ```org.kurento.client.internal.KmsProvider```. 
This interface provides the following methods:

```java
String reserveKms(String id) throws NotEnoughResourcesException;
String reserveKms(String id, int loadPoints) throws NotEnoughResourcesException;
void releaseKms(String id);
```

The method ```reserveKms()``` will be invoked and its value returned. If ```NotEnoughResourcesException``` exception is thrown, it will be thrown in ```KurentoClient.create()``` method.


## REST Interface to NUBOMEDIA Virtual Network Function (VNF)

In conjunction to implementing the ```org.kurento.client.internal.KmsProvider``` this library uses a simple REST client to interact with VNF. This section is not necessary important for developing your application, but so you know how everything glues together, here are a few technical explanations.

During deploying on the NUBOMEDIA PaaS, some environment variables are set by the NUBOMEDIA PaaS Manager on the application container with the address on which the VNF can be reached and also a Virtual Network Function Identifier for your application is created. The ```VNFRService``` interface provides the following API:

```java

	/**
	 * Returns a list of VNFRs managed by the Elastic Media Manager
	 * @return a list of all virtual network function records
	 */
	public List<VirtualNetworkFunctionRecord> getListRegisteredVNFR();
	
	/**
	 * Returns a list with detailed information about all apps registered to the VNFR with this identifier
	 * @param vnfrId - the virtual network function record identifier
	 * @return list - the list of application records
	 */
	public List<ApplicationRecord> getListRegisteredApplications(String vnfrId);
	
	/**
	 * Registers a new App to the VNFR with a specific VNFR ID
	 * @param loadPoints - capacity
	 * @param externalAppId - application identifier
	 * @return ApplicationRecord - the application's record
	 */
	public ApplicationRecord registerApplication(String externalAppId, int loadPoints) 
		throws NotEnoughResourcesException;
	
	/**
	 * Unregisters an App to the VNFR with the specific internal application identify
	 * @param internalAppId - identifier of the registered application on the VNFR
	 */
	public void unregisterApplication(String internalAppId) throws NotEnoughResourcesException;
	
	/**
	 * Sends a heart beat to the elastic media manager as a keep alive mechanism for registered sessions
	 * @param internalAppId - the internal application identifier
	 * @param internalAppId
	 */
	public void sendHeartBeat(String internalAppId);
```

And here is the connection:

```kurentoClient.Create().reserveKms()``` -> get instance of ```de.fhg.fokus.nubomedia.kmc.KmsUrlProvider```-> ```VNFRService.registerApplication()``` return KMSURL or throws ```NotEnoughResourcesException```

If return KMSURL = TRUE then ```VNFRService.sendHeartBeat``` every 1 minute to VNF for the KMS.

Subsequently:

```kurentoClient.Create().releaseKms()``` -> get instance of ```de.fhg.fokus.nubomedia.kmc.KmsUrlProvider```-> ```VNFRService.unregisterApplication()``` return void or throws ```NotEnoughResourcesException``` -> ```stopHeartBeatTask();```
