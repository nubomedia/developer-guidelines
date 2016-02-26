#### Overview
This is a very light weight library that provides the following functionality to NUBOMEDIA applications:
* CloudRepositoryProfile that contains the information to access the NUBOMEDIA cloud repository
* Implementation of Kurento repository RepositoryUrlProvider interface to obtain the IP address of the server 

#### Prerequisite
Make sure to include the line below in your config.properties file found in [user.home]/.kurento/. If this file or folder doesn't exist, go ahead and create it.

```
kms.url.provider=de.fhg.fokus.nubomedia.kmc.KmsUrlProvider
````
