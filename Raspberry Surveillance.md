# 树莓派监控（Raspberry Surveillance）

I reccently bought a Raspberry PI. The main reason was that I had an idea of making some video surveillance software. 
I have made 2 previous blog posts regarding the Raspberry Pi and both of them are sort of related this one. One where 
I wrote some notes on the initial setup of the hardware and basic software. One where I wrote about how to setup 
Owncloud. This post is about https://github.com/tomasbjerre/RaspberrySurveillance. It is project where I've developed, 
mainly, a web interface that can:

* Show snapshot of camera
* Setup motion triggering
* Start and stop motion triggering
* Save captured videos to disk
* Save captured videos to Webdav (Supported by Owncloud)

This means you can start and stop motion triggering from a web interface. Whenever a video is captured you can have 
it uploaded to Owncloud and have it automatically synced to you Windows PC.

<img src="http://files.bjurr.se/blog/rbs.png">

When camera triggers it will store pictures and/or vieos, it is configurable. Here is a very small example of a 
captured event.

* <a href="http://files.bjurr.se/blog/00003-image.jpg" target="_blank">Picture that detected change</a>
* <a href="http://files.bjurr.se/blog/00003-2013-08-06_10-03-30-video.h264" target="_blank">Video recorded on trigger</a>
* <a href="http://files.bjurr.se/blog/00003-image-diff.jpg" target="_blank">Picture showing what was changed in picture</a>

### 原文地址：http://bjurr.se/raspberry+surveillance
