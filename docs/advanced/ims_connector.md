# IMS Connector
The IMS Connector enables developers to create feature-rich communications applications with open Telecomunication  standards - 3GPP IMS and Open Mobile Alliance (OMA) service enablers for Presence, XML Document Management (XDM), SIMPLE IM-based instant messaging.  The key benefits and advantages provided by this module is the shortened development time by providing a high abstraction API layer above the underlying protocol complexity. Key features provided by the connector include:
* Event Notification Framework for Presence 
* Publication, Subscription and Notification
* OMA Instant Messaging and Conferencing Multimedia Telephony with Audio and Video
* Chat with MSRP (Message Session Relay Protocol)
* Multimedia Telephony (3GPP MMTel)

## Use of IMS Connector
This section covers the usage scope of the IMS Connector. What can be expected from the module and what developers should keep in mind on using the module.

1. *Existing IMS Network*: This module does not provide a backend IMS network. It only provides functionality as an IMS User Agent (UA) to connect to an existing IMS network which the user is subscribed to.
2. *Signaling Path*: provides only signaling functionalities to establish a session with other IMS UA, the media path will need to be integrated by the developers. In the case of using this connector with the NUBOMEDIA platform, developers will need to use the NUBOMEDIA media client to set up the media path between two endpoints.
3. *User Agents*: It is assumed that the user agents are from same technology stack, i.e. User Agents with same session protocols negotiation capabilities.  

## Service Architecture
The IMS Connnector constitutes four building blocks namely the Core Services, IMS API and the Sip Stack.

### Core Services
This module provides interfaces for developers to registers and instantiate an instance of the core service object in order to consume the exposed IMS APIs. Applications connect to the IMS connector using the ```Connector.open```call with arguments ```<scheme>:<target>[<params>]```where:
* *scheme* is a supported IMS scheme. In this specification the imscore is defined in the CoreService interface 
* *target* is an AppId. An AppId should follow the form of fully qualified Java class names or any naming syntax that provides a natural way to differentiate between modules. 
* *params* are optional semi-colon separated parameters particular to the IMS scheme used
Calling ```Connector.open```returns an instance of the core service object.

```
myCoreService = (CoreService) Connector.open(“imscore://org.nubomedia.CallApp”); 
myCoreService.setListener(this); 
```

With the ```CoreService``` instance, the application can create the following services:
* CreateCapabilities
* CreatePageMessage
* CreatePublication
* CreateReference
* CreateSession
* CreateSubscription

The ```CoreService Listener``` interface notifies the application on the following notifications
* pageMessageRecieved
* referenceReceived
* sessionInvitationReceived
* UnsolicitatedNotifyReceived


To terminate execution and un-register the application from the IMS network, the application need to call ```myCoreService.close``` method.

### IMS API 
The IMSAPI features a high level of abstraction, hiding the technology and protocol details, but can still prove to be useful and powerful enough for non-experts to create new SIP based IMS applications. It maintains the fundamental distinction between the signalling and the media plane in the API. It manages the SIP transactions and dialogs according to the standards, formats SIP messages, and creates proper SDP payloads for sessions and media streams. SIP methods, with the transactions and dialogs they imply, have abstractions in the API. The core service object relates to IMS registration and the SIP REGISTER message.
The service methods correspond to non-REGISTER SIP messages and, where applicable, the session oriented SDP lines, sent to remote endpoints, including the standardized transactions and dialogs that the standards imply.

* Session Service Interface  - ```INVITE, UPDATE, BYE, CANCEL```
* Reference Service Interface - ```REFER and NOTIFY```
* Subscription Service Interface - ```SUBSCRIBE and NOTIFY```
* Publication Service Interface - ```PUBLISH and NOTIFY```
* Capabilities Service Interface- ```OPTIONS```
* PageMessage Service Interface - ```MESSAGE```
The ```SIP INFO``` method has no representation in this API

Here is a simple example on using the PageMessage Service Interface.
*Sending a simple PageMessage* 
```
try { 
  PageMessage pageMessage; 
  pageMessage = coreservice.createPageMessage(null, "sip:alice@nubomedia.org"); 
  pageMessage.setListener(this); 
  pageMessage.send("Hi, I'm Bob!".getBytes(),"text/plain");
} 
catch (Exception e){ 
  //handle Exceptions 
}
```
*Receiving a PageMessage*
The callback method pageMessageReceived is found in the CoreServiceListener interface. 
```
public void pageMessageReceived(CoreService service, PageMessage pageMessage) { 
  pageMessage.getContent(); 
  ... // process content 
}
```

### Sip Stack 
The Sip Stack used on the IMS connector is the JAIN-SIP reference implementation.
The Java APIs for Integrated Networks (JAIN) is a Java Community Process (JCP) work group managing telecommunication standards. JAIN SIP API is a standard and powerful SIP stack for telecommunications. The reference implementation is open source, very stable, and very widely used for SIP application development. The sip stack provides three base transport protocols, TCP, UDP and recently also WS.
