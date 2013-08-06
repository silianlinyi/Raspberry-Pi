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

## 通用命令行选项

### 预览窗口（Preview Window）
> --preview, -p 预览窗口设置 <'x,y,w,h'>

Allows the user to define the size and location on the screen that the preview window will be placed. Note this will be
superimposed over the top of any other windows/graphics.

> --fullscreen, -f 全屏预览模式

Forces the preview window to use the whole screen. Note that the aspect ratio of the incoming image will be retained, 
so there may be bars on some edges.

> --nopreview, -n, 禁用预览窗口

完全禁用预览窗口。请注意：尽管预览窗口被禁用，摄像头还是会产生帧，所以也会耗电。

> --opacity, -op 设置预览窗口透明度

设置预览窗口透明度。0 = 透明，255 = 完全不透明

### 摄像头控制选项（Camera Control Options）

> --sharpness, -sh 设置图像清晰度(-100 to 100)

设置图像清晰度，默认是0.

> --contrast, -co 设置图像对比度(-100 to 100)

设置图像对比度，默认是0.

> --brightness, -br 设置图像亮度(0 to 100)

设置图像亮度，默认是50.0是黑的，100是白的。

> --saturation, -sa 设置图像色彩饱和度(-100 to 100)

设置图像色彩饱和度，默认是0.

> --ISO, -ISO Set capture ISO

Sets the ISO to be used for captures. Range is 100 to 800.

> --vstab, -vs 打开视频防抖

打开视频防抖，只用于视频模式下。

> --ev, -ev 设置曝光补偿

设置图像曝光补偿。范围是-10到+10，默认是0.

> --exposure, -ex 设置曝光模式

可选项包括：

* <code>auto</code>           自动曝光模式
* <code>night</code>          夜间拍摄模式
* <code>nightpreview</code>
* <code>backlight</code>      背光
* <code>spotlight</code>
* <code>sports</code>         运动Select setting for sports (fast shutter etc.)
* <code>snow</code>           Select setting optimized for snowy scenery
* <code>beach</code>          Select setting optimized for beach
* <code>verylong</code>       Select setting for long exposures
* <code>fixedfps</code>       Constrain fps to a fixed value
* <code>antishake</code>      Antishake mode
* <code>fireworks</code>      Select setting optimized for fireworks

> 请注意并非所有这些设置都已实现，依赖于camera tuning

> --awb, -awb 设置自动白平衡(AWB)

* <code>off</code>            Turn off white balance calculation
* <code>auto</code>           自动模式（默认）
* <code>sun</code>            Sunny mode
* <code>cloud shade</code>    Cloudy mode
* <code>tungsten</code>       Tungsten lighting mode
* <code>fluorescent</code>    Fluorescent lighting mode
* <code>incandescent</code>   Incandescent lighting mode
* <code>flash</code>          Flash mode
* <code>horizon</code>        Horizon mode

> --imxfx, -ifx 设置图像特效

* <code>none</code>           无
* <code>negative</code>       负片
* <code>solarise</code>       过度曝光的图像
* <code>whiteboard</code>     Whiteboard effect
* <code>blackboard</code>     Blackboard effect
* <code>sketch</code>         素描特效
* <code>denoise</code>        去噪图像
* <code>emboss</code>         浮雕特效
* <code>oilpaint</code>       油画特效
* <code>hatch</code>          Cross-hatch sketch style
* <code>gpen</code>           石墨素描风格
* <code>pastel</code>         柔光特效
* <code>watercolour</code>    水彩效果
* <code>film</code>           Grainy film effect
* <code>blur</code>           模糊效果
* <code>saturation</code>     Colour-saturate the image
* <code>colourswap</code>     没有完全实现
* <code>washedout</code>      没有完全实现
* <code>posterise</code>      没有完全实现
* <code>colourpoint</code>    没有完全实现
* <code>colourbalance</code>  没有完全实现
* <code>cartoon</code>        没有完全实现

> --colfx, -cfx 设置色彩效果<U:V>

The supplied U and V parameters (range 0 to 255) are applied to the U and Y channels of the image. For example, --colfx 
128:128 should result in a monochrome image.

> --metering, -mm 设置测光模式

指定测光模式用于预览和截图。
* <code>average</code>        Average the whole frame for metering
* <code>spot</code>           Spot metering
* <code>backlit</code>        Assume a backlit image
* <code>matrix</code>         Matrix metering

> --rotation, -rot Set image rotation (0-359)

Sets the rotation of the image in viewfinder and resulting image. This can take any value from 0 upwards, but due to 
hardware constraints only 0, 90, 180 and 270-degree rotations are supported.

> --hflip, -hf Set horizontal flip

Flips the preview and saved image horizontally.

> --vflip, -vf Set vertical flip

Flips the preview and saved image vertically.

> --roi, -roi Set sensor region of interest

Allows the specification of the area of the sensor to be used as the source for the preview and capture. This is defined 
as x,y for the top left corner, and a width and height, all values in normalised coordinates (0.0-1.0). So to set a ROI 
at half way across and down the sensor, and an width and height of a quarter of the sensor use :-roi 0.5,0.5,0.25,0.25

## 特定应用设置（Application-­‐specific settings）

### raspistill

    --width, -w Set image width <size>
    --height, -h Set image height <size>
    --quality, -q Set jpeg quality <0 to 100>
    
Quality 100几乎没有压缩，75是一个不错的选择。

    --raw, -r 添加raw Bayer数据到jpeg metadata中

This option inserts the raw Bayer data from the camera in to the JPEG metadata.

    --output -o 输出文件名字<filename>

指定输出文件名字。如果没有指定，文件将不会保存。如果文件名字是'-'，那么输出将会被发送到<code>stdout</code>。

    --verbose, -v 运行过程中输出详细信息。

在运行过程期间输出调试/信息消息。

    --timeout, -t Time before capture and shut down

The program will run for this length of time, then take the capture (if output is specified). If not specified, 
this is set to 5 seconds.

    --timelapse, -tl 间隔拍摄模式

The specific value is the time between shots in milliseconds. Note you should specify %04d at the point in the filename 
where you want a frame count number to appear. For example:

    -t 30000 -tl 2000 -o image%04d.jpg

上面的代码意思是，在30秒时间内，每两秒截一张图，依次命名为image1.jpg, image0002.jpg...image0015.jpg. Note that the %04d 
indicates a four-digit number with leading zeros added to pad to the required number of digits. So, for example, 
%08d would result in an eight-digit number.

    --thumb, -th Set thumbnail parameters (x:y:quality)











