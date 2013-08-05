# RaspiCam Documentation

## July 2013
This document describes the use of the three Raspberry Pi camera applications as of July 2013.

这份文档介绍了如何使用树莓派的三个摄像头应用程序。

There are three applications provided: raspistill, raspivid and raspistillyuv. Both raspistill and raspistillyuv
are very similar and are intended for capturing images, while raspivid is for capturing video.

树莓派提供了三个应用程序：<code>raspistill</code>，<code>raspivid</code>，<code>raspistillyuv</code>。其中raspistill和
raspistillyuv是非常类似的，它们都被用于截图，raspivid用于录像。

All the applications are command-line driven, written to take advantage of the mmal API which runs over OpenMAX. The
mmal API provides an easier to use system than that presented by OpenMAX. Note that mmal is a Broadcom specific API used
only on Videocore 4 systems.

The applications use up to four OpenMAX(mmal) components camera, preview, encoder and null_sink. All applications
use the camera component: raspistill uses the Image Encode component, raspivid uses the Video Encode component, 
and raspistillyuv does not use an encoder, and sends its YUV or RGB output direct from camera component to file.

The preview display is optional, but can be used full screen or directed to a specific rectangular area on the display. 
If preview is disabled, the null_sink component is used to 'absorb' the preview frames. It is necessary for the camera 
to produce preview frames even if not required for display, as they are used for calculating exposure and white balance 
settings.

In addition it is possible to omit the filename option, in which case the preview is displayed but no file is written, 
or to redirect all output to stdout. Command line help is available by typing just the application name in on the 
command line.

## Setting up the camera hardware
Please note that camera modules are static-sensitive. Earth yourself prior to handling the PCB: a sink tap/faucet or 
similar should suffice if you don’t have an earthing strap.

The camera board attaches to the Raspberry Pi via a 15-way ribbon cable. There are only two connections to make: 
the ribbon cable need to be attached to the camera PCB and the Raspberry Pi itself. You need to get it the right way 
round, or the camera will not work. On the camera PCB, the blue backing on the cable should be facing away from the PCB, 
and on the Raspberry Pi it should be facing towards the Ethernet connection (or where the Ethernet connector would be 
if you are using a model A).

Although the connectors on the PCB and the Pi are different, they work in a similar way. On the Raspberry Pi, pull up 
the tabs on each end of the connector. It should slide up easily, and be able to pivot around slightly. Fully insert 
the ribbon cable into the slot, ensuring it is straight, then gently press down the tabs to clip it into place. The 
camera PCB itself also requires you to pull the tabs away from the board, gently insert the cable, then push the tabs 
back. The PCB connector is a little more awkward than the one on the Pi itself. You can watch a video showing you how
to attach the connectors at www.raspberrypi.org/archives/3890 (scroll down for the video).

## Setting up the Camera software







