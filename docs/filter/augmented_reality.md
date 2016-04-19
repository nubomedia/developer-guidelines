# Augmented Reality (AR)

## ARModule

ARModule contains marker detector filter utilizing ALVAR augmented reality library ie ALVAR markers can be detected from the video image. ARModule can render 2D and 3D augmentation with OpenCV and Irrlicht rendering engine above the detected ALVAR marker. ARModule can also and send events about markers to the Kurento Client. Find more information about this module in the [ARModule documentation site](http://nubomedia-vtt-ar.readthedocs.org/).

## MultisensoryDataFilter

MultisensoryDataFilter (MsData) is a Kurento Filter that implements multi-domain AR filter providing 2D graphics visualisation service to other modules through data pads. MsData Interface enables sending of different kinds of content based on the identification and variable arguments thus the format of the data is quite flexible. Find more information about this module in the [MsData documentation site](http://nubomedia-vtt-msdata.readthedocs.org/).