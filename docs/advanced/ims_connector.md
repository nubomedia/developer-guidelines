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

## Setting up IMS Network
For the IMS network, the Open Source IMS Core (OSIMS) which was developed by Fraunhofer Institut FOKUS and now maintained by a spin-off company of Fraunhofer Institut FOKUS called Core Network Dynamics (CDN). OSIMS is an Open Source implementation of IMS Call Session Control Functions (CSCFs) and a lightweight Home Subscriber Server (HSS), which together form the core elements of all IMS/NGN architectures as specified today within 3GPP, 3GPP2, ETSI TISPAN and the PacketCable intiative. The four components are all based upon Open Source software (e.g. the SIP Express Router (SER) or MySQL). 

A comprehensive guide on setting up your own OSIMS network can [here](http://www.openimscore.org/documentation/installation-guide/). This guide, guides you on how to obtain the source, compile it, configure the network environment, configure the IMS core network to map you environment, start the components and configure the subscribers. It provides two default users (Alice and Bob), you can use to test.

### Creating new Subscribers
This applies in the case, you wish to create new subscribers. All configurations are carried out through the FHoSS (FOKUS Home Subscriber Server) web interface. 
The IMS subscriber account is composed of three correlated parts:
*	Subscription (IMSU) – identifies the user on the level of contract between subscriber and network,
*	Private Identity (IMPI) – used by the user for authorization and authentication within the home network,
*	Public User Identity (IMPU) – addressable identity of the user.

Any Service Profile is always prescribed to particular IMPU. This part describes how to create such account from the scratch. In order to create a user account, first all the above described steps need to be executed. To create IMSU from web console navigate through: User Identities → IMS Subscription → Create and specify the following values:
*	Name – any reasonable unique name
*	Capabilities Set – optional parameter specifying S-CSCF selection preferences for Interrogating-CSCF (I-CSCF),
*	Preferred S-CSCF – optional preassigned S-CSCF.

To create IMPI navigate through: User Identities → Private Identity → Create and specify the following values:
*	Identity – in the form username@domain i.e. carlo@nubomedia-ims.test
*	Secret key – password,
*	Authentication Schemes – whichever is required, I usually use all,
*	Default – for instance Digest-MD5.

Hereafter, the IMPI needs to be assigned to the previously created IMSU.
To create IMPU navigate through: User Identities → Public User Identity → Create and specify the following values:
*	Identity – in the form of SIP URI  i.e. sip:carlo@nubomedia-ims.test
*	Service Profile – any existing profile, there is always an empty profile created (no IFCs attached), which can be assigned by default.
At the end add a list of allowed visited networks, at least the home network and associate IMPU with previously created IMPI.

Last step in the provisioning process is to assign a service profile to the User subscriber account. A Service Profile is activated for a particular user by assigning it to one of his IMPU’s. From the HSS web console, navigate to: User Identities → Public User Identity → Search.

## Testing and Validation
To test and validate the IMS connector on your IMS network, a sample tutorial can be found [here]("../tutorial/nubomedia-ims.md") that explains how the IMS connector is used within an application and how the application connects can connect to your IMS network.
