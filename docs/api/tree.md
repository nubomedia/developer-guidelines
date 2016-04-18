# NUBOMEDIA Tree API

The NUBOMEDIA Tree API allows developers to build video broadcasting web applications. It is developed using WebRTC technology on the top of Kurento Media Server. This API is composed by a server and two clients (Java and a JavaScript):

* ```tree-server``` is a component of the API designed to be deployed and controlled by clients. This component uses a Kurento Media Server instance to provide WebRTC media handling to clients. For this reason, a Kurento Media Server has to be available and configured in the server configuration file.
* ```tree-client``` implements a Tree Client designed to be used in Java web applications or Android applications. The client contains an implementation of JSON-RPC WebSocket protocol implemented by Tree server. It does not contain any functionality to control video players or video capturing from webcams.
* ```tree-client-js``` implements a Tree Client to be used in Single Page Applications (SPA). Besides the JSON-RPC WebSocket protocol, it uses several libraries to control WebRTC and HTML5 APIs in the browsers. This allows to developer to focus in its application functionality hiding low level details.


The Tree server exposes a SockJS  WebSocket at http://treeserver:port/kurento-tree, where the hostname and port depend on the current setup. The exchanged messages between server and clients are JSON-RPC 2.0 requests and responses. The events are sent from the server to client as notifications (they don’t require a response and they don’t include an identifier).

The Java and JavaScript clients has been designed to be used with browsers (it uses several browser-only APIs). Other clients can be implemented if follow the JSON-RPC over WebSocket protocol


## Feature Overview
The NUBOMEDIA Tree API is based on client-server architecture. The exchanged messages between server and clients are JSON-RPC 2.0 requests and responses. The events are sent from the server to client as notifications (they don’t require a response and they don’t include an identifier). The following table summarizes the possible operations provided by the Tree service: 


| Operation            | Description                                    |
|----------------------|------------------------------------------------|
| Create tree          | Request to create a new tree in Tree server. It is send by clients to server.    |
| Set tree source      | Request to configure the emitter (source) in a broadcast session (tree). It is send by clients to server.    |
| Remove tree source   | Request to remove the current emitter of a tree. It is send by clients to server.|
| Add tree sink        | Request to add a new viewer (sink) to the tree. It is send by clients to server. |
| Remove tree sink     | Request to remove a previously connected sink (viewer). It is send by clients to server.|
| Ice candidate        | Notification sent form server to client when a new Ice candidate is received from Kurento Media Server. It is send by server to clients.|
| Add ice candidate    | Request used to add a new ice candidate generated in the browser. It is send by clients to server.|
| Remove tree          | Request used to remove a tree. It is send by clients to server.|


## Example Use Case
The typical scenario of an application using the NUBOMEDIA Tree API is a web application that allows a user to broadcast his webcam and any other user can see it.

From a technical point of view, this use case can be implemented as a Single Page Application (SPA) as a bunch of HTML, CSS and JS files. These files are served from a SpringBoot web server, but can be served with any http server. The JavaScript code uses Tree JavaScript client to communicate directly with Tree Server.

JavaScript logic communicates with SpringBoot server by means of WebSocket protocol. The SpringBoot server handle WebSocket messages and uses Tree Client to communicate with Tree Server. That is, there is no direct communication between JavaScript code and Tree Server. All communications is through SpringBoot server app.


## Resources
***
