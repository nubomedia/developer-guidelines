### APIs
In order to deploy your application on the NUBOMEDIA Cloud Platform, there are there main libraries to include as dependency to your projects.
These libraries extend the Kurento libraries with functionalities on how to obtain network resources for example the IP address of the Kurento Media Server.
The next sections below provide an overview of each library, how to obtain the library and how to include it into your project.

### NUBOMEDIA Media Client (NMC) Library
The NMC library provides the base functionality to compliment the Kurento Client for auto discovery of the Kurento Media Server (KMS) IP.

#### Getting Started
This section explains where to obtian the NMC and how to include it on your project. The assumption here is you are a maven Guru. If not, there are plenty of tutorials online to get you started.

The NMC is distributed via Maven can be found on [Maven central repository](http://search.maven.org/#search%7Cga%7C1%7Cde.fhg).
Simply include it on your project's pom.xml file as describe below, then run the command ```mvn install```.

```
<dependencies>
...
<dependency>
<!-- Kurento client dependency -->
<groupId>org.kurento</groupId>
	<artifactId>kurento-client</artifactId>
</dependency>
<dependency>
  <!-- Nubomedia client dependency -->
		<groupId>de.fhg.fokus.nubomedia</groupId>
		<artifactId>nubomedia-media-client</artifactId>
		<version>1.0</version>
	</dependency>
</dependencies>
```
*NOTE: At the time of writing the release version is 1.0. This might change as development evolves, so make sure you have the right version (latest) and replace the version number accordingly.*

#### KMS Auto Discovery Process
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

```
String reserveKms(String id) throws NotEnoughResourcesException;
String reserveKms(String id, int loadPoints) throws NotEnoughResourcesException;
void releaseKms(String id);
```
The method ```reserveKms()``` will be invoked and its value returned. If ```NotEnoughResourcesException``` exception is thrown, it will be thrown in KurentoClient.create() method.


#### REST Interface to NUBOMEDIA Virtual Network Function (VNF)
In conjunction to implementing the ```org.kurento.client.internal.KmsProvider``` this library uses a simple REST client to interact with VNF. This section is not necessary important for developing your application, but so you know how everything glues together, here are a few technical explanations.

During deploying on the NUBOMEDIA PaaS, some envirnoment variables are set by the NUBOMEDIA PaaS Manager on the application container with the address on which the VNF can be reached and also a Virtual Network Function Identifier for your application is created. The ```VNFRService``` interface provides the following API:
```
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
