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
