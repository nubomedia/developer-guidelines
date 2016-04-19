# WebRtcPeer API

The NUBOMEDIA WebRtcPeer API abstracts the client RTC media capabilities, so that its media capture and communication capabilities are exposed to the developer in a simple, seamless and unified way. It is specifically concentrated on WebRTC client capabilities. Following W3C WebRTC specifications WebRTC APIs are split into two: the Media Capture API (i.e. getUserMedia) and the PeerConnection API. The former exposes client capabilities for accessing webcam and microphone while the latter enables media communications through an SDP negotiation mechanism. The WebRtcPeer unifies both under a common abstraction.

## Features overview
Ths WebRtcPeer API offers a WebRtcPeer object, which is a wrapper of the browser’s RTCPeerConnection API. Peer connections can be unidirectional (send or receive only) or bidirectional (send and receive). The following snippet shows how to create the latter in JavaScript, i.e. a WebRtcPeer object to send and receive media (audio and video). This code assumes that there are two different video tags in the web page that loads the script. These tags are used to show the video as captured by the browser and the media received from other peer. The constructor receives a property bag that holds all the information needed for the configuration

```
var videoInput = document.getElementById('videoInput');
var videoOutput = document.getElementById('videoOutput');

 var constraints = {
     audio: true,
     video: {
       width: 640,
       framerate: 15
     }
 };

 var options = {
   localVideo: videoInput,
   remoteVideo: videoOutput,
   onicecandidate : onIceCandidate,
   mediaConstraints: constraints
 };

var webRtcPeer = kurentoUtils.WebRtcPeer.WebRtcPeerSendrecv(options, function(error) {
      if(error) return onError(error)

      this.generateOffer(onOffer)
   });

```

After executing this code, an RTCPeerConnection object is created and then the method getUserMedia is invoked. The constraints are used in the invocation, and in this case both microphone and webcam are used. However, this does not create the connection between peers. This is only achieved after completing the SDP negotiation between peers. This process implies exchanging SDPs offer and answer and, since Trickle ICE is the mechanism carried out by the API. Therefore, a number of candidates describing the capabilities of each peer should be exchanged.

In the previous piece of code, when the webRtcPeer object gets created, the SDP offer is generated with this.generateOffer(onOffer). The only argument passed is a function, that will be invoked one the browser’s peer connection has generated that offer. The onOffer callback method is responsible for sending this offer to the other peer.

Assuming that the SDP offer has been received by the remote peer, it must have generated an SDP answer that should be received in return. This answer must be processed by the webRtcEndpoint, in order to fulfill the negotiation. This could be done in the implementation of the onOffer callback function.

```
function onOffer(error, sdpOffer) {
  if (error) return onError(error);

  // We've made this function up
  sendOfferToRemotePeer(sdpOffer, function(sdpAnswer) {
    webRtcPeer.processAnswer(sdpAnswer);
  });
}
```
As introduced before, the library assumes the use of Trickle ICE to complete the connection between both peers. In the configuration of the WebRtcPeer object, there is a reference to an onIceCandidate callback function. The library will use this function to send ICE candidates to the remote peer. In turn, the client application must be able to receive ICE candidates from the remote peer. Assuming the signaling takes care of receiving those candidates, it is enough to invoke the following method in the webRtcPeer to gather the ICE candidate:

```
webRtcPeer.addIceCandidate(candidate);
```

## Examples and Use Cases
There are several ways to use the NUBOMEDIA WebRtcPeer API:

1.	By means of the minified JavaScript file. This library can be directly downloaded from the following URL:

http://builds.kurento.org/release/6.2.0/js/kurento-client.min.js

1.	By means of NPM (package manager for Node.js):

npm install kurento-utils

1.	By means of Bower (package manager for browser JavaScript):

bower install kurento-utils

There are complete tutorials that show how to use this API in WebRTC applications developed on Java, Node and JavaScript. These tutorials are hosted on GitHub:
* [Java](https://github.com/Kurento/kurento-tutorial-java)
* [Node] (https://github.com/Kurento/kurento-tutorial-node)
* [Javascript]( https://github.com/Kurento/kurento-tutorial-js)

## API Availability
The NUBOMEDIA Repository API is currently available in the following languages:
* JavaScript for WebRTC browsers (i.e. Chrome and Firefox on all versions since beginning 2015)
* Java for Android 4.0 and later
* Objective C for iOS on versions 7.0 and later.

These implementations are described in sections below in this document. These implementations are just a wrapper of the getUserMedia and RTCPeerConnection APIs provided by the WebRTC Chrome stack. Hence, further programming languages might be supported by creating the appropriate wrappers on those APIs.

## Resources
Full Documentation
* [Browser WebRtcPeer](http://doc-kurento.readthedocs.org/en/stable/mastering/kurento_utils_js.html)
* [Android WebRtcPeer](http://webrtcpeer-android.readthedocs.org/)
* [iOS WebRtcPeer](https://github.com/nubomediaTI/Kurento-iOS)

API Source code
* [Browser WebRtcPeer](https://github.com/Kurento/kurento-utils-js)
* [Android WebRtcPeer](http://webrtcpeer-android.readthedocs.org/)
* [iOS WebRtcPeer](http://kurento-ios.readthedocs.org/en/latest/index.html)


