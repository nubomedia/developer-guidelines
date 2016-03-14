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

- [Getting started](./getting-started.md): Introduction to the NUBOMEDIA APIs
  and architecture.

- NUBOMEDIA APIs: Detailed description of the different NUBOMEDIA APIs, i.e
  the [Media API](./api/media.md), [Repository API](./api/repository.md),
  [WebRtcPeer API](./api/webrtcpeer.md), [Signaling API](./api/signaling.md),
  [Room API](./api/room.md), and [Tree API](./api/tree.md).

- NUBOMEDIA tutorials: Step-to-step information about demos applications
  created with NUBOMEDIA, namely
  [nubomedia-magic-mirror](./tutorial/nubomedia-magic-mirror.md) (example of an
  application using the Media API),
  [nubomedia-room](./tutorial/nubomedia-room.md) (example of an application
  using the Room API),
  [nubomedia-repository](./tutorial/nubomedia-repository.md) (example of an
  application using the Repository API).

- The [PaaS Manager](./paas/paas-introduction.md) is the part of the system
  enabling developers to deploy and manage their server-side applications. The
  capabilities of this component can be accessed by means of the the [PaaS
  API](./paas/paas-api.md) and the [PaaS GUI](./paas/paas-api.md).

- NUBOMEDIA provides different framework tools aimed to simplify the creation
  of complex multimedia applications. Several tools are provided out of the
  box, for instance the [Visual Development
  Tool](./tools/visual-development-tools.md), and [Video Content Analysis
  (VCA)](./tools/video-content-analysis.md).

!!! info

    This documentation is under development. If you do not find the
    information you are looking for here, please ask for support in the
    [NUBOMEDIA development mailing
