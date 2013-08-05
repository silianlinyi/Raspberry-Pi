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
### To view the feed on Linux:

在终端运行下列命令来安装依赖：
> sudo apt-get install mplayer netcat

Find your IP address by running ifconfig. (Your IP address will be listed in the console output and will probably 
be of the form 192.168.1.XXX).

Run the following command in a terminal to view the feed using MPlayer:
> nc -l -p 5001 | mplayer -fps 31 -cache 1024 -

### To view the feed on Windows

Install and run Linux instead.

Find your IP address by running ipconfig. (Your IP address will be listed in the console output and will probably 
be of the form 192.168.1.XXX).

Note that your browser may complain that these files are malicious, as they are unsigned executables.

Press the Windows key and the ‘r’ key simultaneously to bring up the “Run” dialog. Enter cmd.exe into the dialog 
and press enter/return to open a DOS prompt.

Enter the following command at the prompt to view the feed using MPlayer:
> [Path to nc.exe]\nc.exe -L -p 5001 | [Path to mplayer.exe]\mplayer.exe -fps 31 -cache 1024 -

### To view the feed on OS X

Alternatively, you can download mplayer using Brew, which we recommend.

Find your IP address by running ifconfig. (Your IP address will be listed in the console output and will probably 
be of the form 192.168.1.XXX).

Run the following command in Terminal to view the feed using MPlayer:
> nc -l -p 5001 | mplayer -fps 31 -cache 1024 -

To view the feed on a Raspberry Pi:

Find your IP address by running ifconfig. (Your IP address will be listed in the console output and will probably 
be of the form 192.168.1.XXX).

Run the following commands in a terminal on the receiving Pi:
> mkfifo buffer
> nc -p 5001 -l > buffer | /opt/vc/src/hello_pi/hello_video/hello_video.bin buffer

### To transmit the feed from the Pi with camera module attached








