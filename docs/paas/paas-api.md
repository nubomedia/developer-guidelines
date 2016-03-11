# PaaS Manager

This documentation describes how to use the PaaS Manager using the REST APIs which it exposes

# Deploy an Application

In order to deploy your application it is important that you followed the previous steps provided in section [add section here] in order to understand how to create an application for NUBOMEDIA. If you made all the steps required, you should get at the end a URL of a GIT repository where your Dockerfile is contained (for example: https://github.com/fhg-fokus-nubomedia/nubomedia-magic-mirror). 

For instantiating this application on the NUBOMEDIA PaaS you should create a JSON object like the following: 

```
{
	"gitURL": "https://github.com/fhg-fokus-nubomedia/nubomedia-magic-mirror.git",
	"appName": "nubo-magic-mirror",
	"projectName": "nubomedia",
	"cloudRepository": true,
	"flavor": "MEDIUM",
	"ports": [{
		"port": 8443,
		"targetPort": 8443,
		"protocol": "TCP"
	}, {
		"port": 443,
		"targetPort": 443,
		"protocol": "TCP"
	}],
	"replicasNumber": 2,
	"turnServerActivate": true,
	"stunServerActivate": true,
	"stunServerIp": "192.168.1.1",
	"stunServerPort": "8080",
	"turnServerUrl": "localhost:8080",
	"turnServerUsername": "nubomedia-prod",
	"turnServerPassword": "test",
	"scaleInOut": 1,
	"scale_out_threshold": 50,
	"qualityOfService": "GOLD"
}
```

where: 





