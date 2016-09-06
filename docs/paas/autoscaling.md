# Autoscaling

The NUBOMEDIA autoscaling system provides an advanced functionality for supporting the runtime execution of Multimedia Applications. In particular, this system provides automated scaling in and out (where scaling in means the removal of existing media plane entities while scaling out means adding new ones) of media plane entities based on the existing number of sessions. In order to provide such functionality NUBOMEDIA Applications must use an extended version of the Kurento-media-client library which communicates with the NUBOMEDIA Control layers whenever a session in instantiated or closed. This thight communication between the control layer and the application allows full control of the media plane instances.

