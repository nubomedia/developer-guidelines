# NUBOMEDIA Room API

## Overview

The NUBOMEDIA Room API enables application developers functionalities to create group communication applications adapted to real social interactions. It has been designed for the development of real time conferencing applications basin on room models. In these, each group of participants share a virtual space known as “room” where different resources (e.g. media streams, chat messages, etc.) are shared among the members but isolated to members of other group. The room API makes possible to manage rooms and participants as well as the
communication resources they require in a Kurento Media Server instance. The architecture of an application based on the NUBOMEDIA Room API is illustrated in the Figure below:



## Prerequisite
The NUBOMEDIA Room API can be used safely as long as the following requirements are satisfied:
* The API assumes that all clients participating in a room are based on WebRTC transports. The API does not provide support for other types of RTC transports off-the-shelf.
* The API is based on the NUBOMEDIA Signaling Protocol (i.e. a custom protocol based on JSON-RPC over WebSocket). Hence, ***the API cannot interoperate with other types of signaling mechanisms such as SIP or XMPP***.
