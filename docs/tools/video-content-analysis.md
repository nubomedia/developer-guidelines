# Video Content Analysis (VCA)

This framework is in charge of the computer vision technology of the NUBOMEDIA project, through wihch the developers can apply different computer vision algorithms to their filters. In this sense, you can find different VCA filters such as face or motion detector. Please, for further information visit the [VCA documentation site](http://nubomedia-vca.readthedocs.org/en/latest/index.html).


## ARModule

ARModule contains marker detector filter utilizing ALVAR augmented reality library ie ALVAR markers can be detected from the video image. ARModule can render 2D and 3D augmentation with OpenCV and Irrlicht rendering engine above the detected ALVAR marker. ARModule can also and send events about markers to the Kurento Client. Find more information about this module in the [ARModule documentation site](http://nubomedia-vtt-ar.readthedocs.org/).

## MultisensoryDataFilter

MultisensoryDataFilter (MsData) is a Kurento Filter that implements multi-domain AR filter providing 2D graphics visualisation service to other modules through data pads. MsData Interface enables sending of different kinds of content based on the identification and variable arguments thus the format of the data is quite flexible. Find more information about this module in the [MsData documentation site](http://nubomedia-vtt-msdata.readthedocs.org/).
