树莓派
======

## 如何使用树莓派摄像头软件

<code>raspivid</code> 是一个命令行应用程序，它可以用来捕捉摄像头模块的视频，同时<code>raspistill</code>应用也可以让您
捕捉图像。

<code>-o</code> or <code>–output</code> specifies the output filename and <code>-t</code> or <code>–timeout</code> 
specifies the amount of time that the preview will be displayed in milliseconds. Note that this set to <code>5s</code>
by default and that <code>raspistill</code> will capture the final frame of the preview period.

