# Introduction

NUBOMEDIA is the first **open source elastic cloud PaaS** (Platform as a
Service) specifically designed for
**real-time interactive multimedia services**, which exposes its capabilities
through simple **APIs**.

For understanding how a real PaaS works we just need to think on services such
as Heroku, the Google App Engine or Pivotal PCF. All of them expose to
developers the ability of uploading, deploying and executing applications that
leverage the PaaS capabilities through custom SDKs. By using them, developers
do not need to worry about aspects such as the provisioning, the scalability,
the resilience or the security of the services they consume as they are
provided off-the-shelf by the PaaS. This model is quite convenient as it lets
developers to concentrate on creating their application logic while all the
complex aspects of deploying, scaling and securing them are assumed by the PaaS.

NUBOMEDIA is a real PaaS as it makes possible to upload, deploy and execute
developers’ applications written in the **Java programming language**. At the
same time, NUBOMEDIA is a WebRTC platform as it provides the ability of
accessing scalable, secure and reliable WebRTC capabilities. Thanks to this,
NUBOMEDIA combines the simplicity and ease of development of WebRTC API PaaS
services with the flexibility of real PaaS infrastructures. Hence, as NUBOMEDIA
is a Java PaaS, developers can leverage all the capabilities of the Java
platform for creating their applications. The only difference with other PaaS
services it that NUBOMEDIA makes available WebRTC capabilities through a
specific API. Hence, WebRTC just becomes another of the SDKs that can be used
while programming. Once an application is completed, developers just need to
deploy it into NUBOMEDIA and it will scale in a secure and reliable way with
full transparency.

# Table of contents

This documentation provides developers guidelines on how to use the develop
applications targeted to run on the NUBOMEDIA cloud platform. Here you will
find guidelines on application development, deployment, using the elastic and
scalability functionalities of the platform. The contents of this documentation
is divided into the following sections:

- The guide to understand how to NUBOMEDIA works in a nutshell is depicted in
  the [Getting started](./getting-started.md) section.

- The detailed description of the different NUBOMEDIA APIs can be found in the
  following sections: [Media](./api/media.md),
  [Repository](./api/repository.md), [WebRtcPeer](./api/webrtcpeer.md),
  [Signaling](./api/signaling.md), [Room](./api/room.md), and [Tree
  ](./api/tree.md) API.

- A brief introduction of the NUBOEMDIA SDKs APIs can be found in the
  following sections: [Android](./sdk/android.md) and [iOS](./sdk/ios.md) SDK.

- The tutorials section contains demo applications showing how to use
  NUBOMEDIA by means of different types of multimedia applications. These
  tutorials are [magic-mirror](./tutorial/nubomedia-magic-mirror.md) (i.e.
  WebRTC with computer vision and augmented reality techniques),
  [repository](./tutorial/nubomedia-repository.md) (i.e. media record and
  playback on the NUBOMEDIA repository), and
  [room](./tutorial/nubomedia-room.md) (i.e. WebRTC multiconference application
  using the NUBOMEDIA Room API).

- The [PaaS Manager](./paas/paas-introduction.md) is the part of the system
  enabling developers to deploy and manage NUBOMEDIA applications. The
  capabilities of this component can be accessed by means of the the [PaaS
  API](./paas/paas-api.md) and the [PaaS GUI](./paas/paas-gui.md).

- NUBOMEDIA provide seamless integration and access to advanced media
  processing capabilities such as [Video Content Analysis
  (VCA)](./filter/video-content-analysis.md) also known as Computer Vision, and
  [Augmented Reality (AR)](./filter/augmented_reality.md).

- NUBOMEDIA provides different framework tools aimed to simplify the creation
  of complex multimedia applications, for instance the [Visual Development
  Tool](./tools/visual-development-tools.md), [monitoring
  tools](./tools/monitoring-tools.md) for the applications and media servers
  that were started by NUBOMEDIA PaaS (these tools are also monitoring the
  hardware stats), and [NUBOMEDIA Autonomous
  Installer](./tools/autonomous-installer.md) (NAI), which is be able to
  install the NUBOMEDIA platform into an IaaS environment.

- Some documentation for advanced users can be found this documentation: for
  instance [NUBOMEDIA Architecture](./advanced/nubomedia_architecture.md) and
  [Media Server Discovery
  Process](./advanced/media_server_discovery_process.md).

# Contact

If you have any doubt, comment, or problem related with NUBOMEDIA, please ask
for support in the [NUBOMEDIA development mailing
list](https://groups.google.com/forum/#!forum/nubomedia-dev).
