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

所有的应用程序都是命令行驱动的，
