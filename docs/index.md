# Introduction

NUBOMEDIA is an open source cloud **Platform as a Service (PaaS)** which makes
possible to integrate **Real Time Communications (RTC)** and multimedia through
advanced media processing capabilities. The aim of NUBOMEDIA is to democratize
multimedia technologies helping all developers to include advanced multimedia
capabilities into their Web and smartphone applications in a simple, direct and
fast manner. To accomplish that objective, NUBOMEDIA provides a **set of APIs**
that try to abstract all the low level details of service deployment,
management, and exploitation allowing applications to transparently scale and
adapt to the required load while preserving QoS guarantees.

# Table of contents

This documentation provides developers guidelines on how to use the develop
applications targeted to run on the NUBOMEDIA cloud platform. Here you will
find guidelines on application development, deployment, using the elastic and
scalability functionalities of the platform. The contents of this documentation
is divided into the following sections:

- An introduction to the NUBOMEDIA APIs and architecture is depicted in the
  [Getting started](./getting-started.md) section.

- The detailed description of the different NUBOMEDIA APIs can be found in the
  following sections: [Media API](./api/media.md), [Repository
  API](./api/repository.md), [WebRtcPeer API](./api/webrtcpeer.md), [Signaling
  API](./api/signaling.md), [Room API](./api/room.md), and [Tree
  API](./api/tree.md).

- The tutorials section contains demo applications showing how to use
  NUBOMEDIA build different types of multimedia applications. The tutorials are
  [magic-mirror](./tutorial/nubomedia-magic-mirror.md) (i.e. WebRTC with
  computer vision and augmented reality techniques),
  [repository](./tutorial/nubomedia-repository.md) (i.e. media record and
  playback on the NUBOMEDIA repository), and
  [room](./tutorial/nubomedia-room.md) (i.e. WebRTC multiconference application
  using the NUBOMEDIA Room API).

- The [PaaS Manager](./paas/paas-introduction.md) is the part of the system
  enabling developers to deploy and manage NUBOMEDIA applications. The
  capabilities of this component can be accessed by means of the the [PaaS
  API](./paas/paas-api.md) and the [PaaS GUI](./paas/paas-api.md).

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

# Contact us

If you have any doubt, comment, or problem related with NUBOMEDIA, please ask
for support in the [NUBOMEDIA development mailing
list](https://groups.google.com/forum/#!forum/nubomedia-dev).
