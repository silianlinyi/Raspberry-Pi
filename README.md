树莓派
======

## 如何使用树莓派摄像头软件

<code>raspivid</code> 是一个命令行应用程序，它可以用来捕捉摄像头模块的视频，同时<code>raspistill</code>应用也可以让您
捕捉图像。

<code>-o</code> or <code>–output</code> specifies the output filename and <code>-t</code> or <code>–timeout</code> 
specifies the amount of time that the preview will be displayed in milliseconds. Note that this set to <code>5s</code>
by default and that <code>raspistill</code> will capture the final frame of the preview period.

<code>-d</code> or <code>–demo</code> runs the demo mode that will cycle through the various image effects that are 
available.

## 实例命令

截图以jpeg格式
> raspistill -o image.jpg

获取5秒H264格式的视频
> raspivid -o video.h264

获取一个10秒的视频
> raspivid -o video.h264 -t 10000

在演示模式下获取10秒视频
> raspivid -o video.h264 -t 10000 -d

To see a list of possible options for running raspivid or raspistill, you can run:
> raspivid | less

> raspistill | less

Use the arrow keys to scroll and type q to exit.

> Note that we recommend that you change SSH password if you are using a camera, in order to prevent unwanted access.

## 如何通过网络查看树莓派摄像头的视频
> Linux:

在终端运行下列命令来安装依赖：
> sudo apt-get install mplayer netcat

Find your IP address by running ifconfig. (Your IP address will be listed in the console output and will probably 
be of the form 192.168.1.XXX).






