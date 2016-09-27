# PaaS Manager APIs

This documentation describes how to use the PaaS Manager via the exposed REST APIs. We'll use a command line HTTP client (curl) for showing the different REST API calls.

## Get a token

In order to request a token, you need to execute the following request: 

```bash
curl -s -u openbatonOSClient:secret -X POST http://localhost:8081/oauth/token 
-H "Accept:application/json" 
-d "username=admin&password=passowrd&grant_type=password“ 
```

Response: 
```json
{
    "additionalInformation": {},
    "expiration": "Sep 28, 2016 6:49:35 AM",
    "refreshToken": {
        "expiration": "Oct 27, 2016 6:49:35 PM",
        "value": "7b7040a2-85cb-4791-b4e4-427f2a2a0afb"
    },
    "scope": [
        "read",
        "write"
    ],
    "tokenType": "bearer",
    "value": "0e8f1ca3-2605-4eae-b229-4f886228b993"
}
```


## Create an Application

In order to create in order words deploy your application on NUBOMEDIA PaaS, it is important that you followed the previous steps provided in the [Guidelines on Building Application](https://github.com/nubomedia/developer-guidelines/blob/develop/docs/paas/paas-introduction.md) in order to understand how to create an application for NUBOMEDIA. If you made all the steps required, you should get at the end a URL of a Git repository where your Dockerfile is contained. See [example](https://github.com/fhg-fokus-nubomedia/nubomedia-magic-mirror). 

Once you have all files assembled, the *Create App* View on the PaaS Manager GUI provides you two options to create your application: a *Form* view and *JSON* view. Here the *JSON* view is what you wish to use. Explanation on using the *Form* view is found [here](paas-gui.md).

For the *JSON* view, you should see a mask similar to the image below.

![PaaS Manager Gui - Start View](../img/paas_manager_gui_create_app_json.png)
*PaaS Manager - Create App - JSON View*

For instantiating this application on the NUBOMEDIA PaaS you should create a JSON object like the following: 

```json
{
	"gitURL": "https://github.com/fhg-fokus-nubomedia/nubomedia-magic-mirror.git",
	"name": "nubo-magic-mirror",
	"cloudRepository": true,
	"cdnConnector": true,
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
	"turnServerUsername": "username",
	"turnServerPassword": "password",
	"scaleInOut": 1,
	"scale_out_threshold": 50,
	"qualityOfService": "GOLD"
}
```

where: 

- ```gitURL```: the Git repository URL that contains your project (source or jar), Dockerfile and other files that are necessary to run your application. If the repository is public the link has to be the HTTPS version, if is private has to be the SSH version

- ```name```: is the name of your application as you would want it to appear on the PaaS. This name is used for creating the DNS entry for your application.

- ```cloudRepository```: Boolean value of ```true``` or ```false``` indicating if your application will be needing the cloud repository (the cloud repository is a running instance of the [Kurento repository server](http://doc-kurento-repository.readthedocs.org/en/latest/server.html) application)

- ```cdnConnector```: Boolean value of ```true``` or ```false``` indicating if your application will be needing the CDN Connector (the CDN Connector is a running instance of the [NUBOMEDIA CDN Connector](https://github.com/nubomedia/nubomedia-cdn-connector), more info at [tutorial CDN Connetor](../tutorial/nubomedia-cdn.md))
 
- ```flavor```: This defines the size of the KMS instances. With MEDIUM flavor, you get 2 VCPU and with LARGE flavor you have 4VCPU. The capacity is defined as 100 points for VCPU.

- ```ports```: Indicate the transport protocol and ports exposed for your application. The port is the port exposed by the container on which  your application will be running, and the target port is the external port on which your application is reachable for the outside. So there is a mapping coming on within the PaaS for the port and target port. In principle, you can leave the port and target port the same, unless your application has special requirements.

- ```replicasNumber```: is a numeric value indicating the number of containers to be created for your application by the PaaS. This value is used for load balancing.

- ```turnServerActivate```: Boolean value of ```true``` or ```false``` indicating if you want a TURN server to be set on the path of your application

- ```stunServerActivate```: Boolean value of ```true``` or ```false``` indicating if you want a STUN server to be set on the path of your application

- ```stunServerIp```: The IP address of the STUN server you wish to use

- ```stunServerPort```: The port of the SUN server you wish to use

- ```turnServerUrl``` - The IP address and port of the TURN server you wish to use

- ```turnServerUsername```:  The username to be used as credentials to access the TURN server

- ```turnServerPassword```:  The password to be used as credential to access the TURN server

- autoscaling properties (please refer to [this page](autoscaling.md) for more information about autoscaling): 

	- ```scaleInOut```: Scale in or out indicates the MAX number of media servers (KMS) instances that will be instantiates at runtime, by the auto scaling system.

	- ```scale_out_threshold```: This is the threshold (in terms of averaged number of points) which will be used for the policy of the autoscaling system. Check which flavor you are going to use before defining this threshold. 

>!!! info
    Do not put a value lower than the total capacity of your flavor!

- ```qualityOfService```: If enabled it provides dedicated bandwidth levels between media server instances (optional). Possible values are:

	- ```BRONZE```
	- ```SILVER```
	- ```GOLD```

When all values have been entered, click on the *Create App* button below. Your request will be forwarded to the PaaS Manager and you will be directed the a view similar to the image below, which shows you the status to the creation process. ```Status``` gives you the status of the process. You might want to reload this page in case you see no update to the status field. The *Load log* button shows you the real time process status from NUBOMEDIA PaaS.

## Get Application status

In order to check the application status you should send the following request: 

```bash
curl -X GET -H "Authorization: Bearer token-id" 
-H "project-id: your-project-ID" 
-H "Accept:application/json" 
http://localhost:8081/api/v1/nubomedia/paas/app/d4fe1d69-fbe1-413c-835a-b03b36e7a21e
```
where: 
- ```token-id``` can be taken from the request get token executed as before

- ```your-project-ID``` can taken from the dashboard

- ```url``` should be the following http://localhost:8081/api/v1/nubomedia/paas/app/app-id

Example response: 
```json
{
    "cdnConnector": false,
    "cloudRepository": false,
    "createdAt": "Sep 27, 2016 6:51:36 PM",
    "createdBy": "admin",
    "flavor": "MEDIUM",
    "gitURL": "https://github.com/fhg-fokus-nubomedia/nubomedia-magic-mirror-jar.git",
    "id": "d4fe1d69-fbe1-413c-835a-b03b36e7a21e",
    "mediaServerGroup": {
        "floatingIPs": [
            "80.96.122.75"
        ],
        "hostnames": [
            "media-server-vnf-v631072ec-444"
        ],
        "id": "165c9700-a211-4abe-8bc4-59142f172a2b",
        "nsdID": "52b34f8d-15e9-448a-9841-a11266ba63f4",
        "nsrID": "2219e596-95b0-4115-9739-2a4af330b975"
    },
    "name": "magic-mirror-mp",
    "osName": "v631072ec",
    "podList": [],
    "ports": [
        8443,
        443
    ],
    "projectId": "170b4875-81ba-4736-95a1-619a725643e1",
    "projectName": "nubomedia",
    "protocols": [
        "TCP",
        "TCP"
    ],
    "replicasNumber": 1,
    "resourceOK": true,
    "route": "v631072ec.paas.nubomedia.eu",
    "scaleInOut": 3,
    "scaleOutThreshold": 150.0,
    "status": "RUNNING",
    "stunServerActivate": false,
    "targetPorts": [
        8443,
        443
    ],
    "turnServerActivate": false
}

```

## Get Application build debug log

In order to get the application build debug log you should send the following request: 

```bash
curl -X GET -H "Authorization: Bearer token-id" 
-H "project-id: project-id" 
http://localhost:8081/api/v1/nubomedia/paas/app/{app-id}/buildlogs
```

## Get Application logs

In order to get the application log you should send the following request: 

```bash
curl -X GET -H "Authorization: Bearer token-id" 
-H "project-id: project-id" 
http://localhost:8081/api/v1/nubomedia/paas/app/{id}/logs/{podName}
```

## Delete an Application

In order to delete an application you should send the following request: 

```bash
curl -X DELETE -H "Authorization: Bearer token-id" 
-H "project-id: project-id" 
http://localhost:8081//api/v1/nubomedia/paas/app/{id}
```



