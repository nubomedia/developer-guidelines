
# Content Distribution Network Connector
A Content Distribution Network (CDN) which can also be called Content Delivery Networks is a distributed infrastructure of proxy servers deployed in multiple data centers with the goal of serving end user content with high availability. A large amount of content (text, graphics, scripts, media files, software, documents, etc.) on the Internet is served by CDNs. There are three main types of CDNs, namely:
* *General purpose CDN*: performs web acceleration by using multiple servers on many locations, ideally close to large connection points between Internet Service Providers (ISPs) or also within the same data center (e.g. gaming data centers). Its main role is to cache and store content that is frequently requested by a high number of users
* *On Demand Video CDN*: performs the same role as a general purpose CDN but with the focus on just video content. These CDNs also provides a streaming server for video delivery.
* *Live video CDN: provides mechanism for live video content delivery

NUBOMEDIA focuses only on *On Demand Video* CDNs. For this purpose an SDK - CDN Connector is provided to developers which offers an data APIs to access the user's data on the CDN. This can be used to upload files from the NUBOMEDIA repository to the user's CDN for publishing. 

![NUBOMEDIA CDN](../img/cdn-architecture.png)

*NUBOMEDIA CDN*


## Service Architecture
