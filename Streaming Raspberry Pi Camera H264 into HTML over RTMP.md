### Streaming Raspberry Pi Camera H264 into HTML over RTMP

#### SPD Raspberry Pi Camera setup instructions for Raspbian Wheezy

Requirements: Raspbian Wheezy with hard float enabled. I assume you have avahi, wifi, and have updated to the latest 
firmware and enabled the camera. 并且在树莓派上配置了一个Web服务器。

安装crtmpserver流媒体服务器：

    sudo aptitude install crtmpserver

<b>注：</b>aptitude 是 Debian GNU/Linux 系统中, 非常神奇的的软件包管理器,基于大名鼎鼎的 APT 机制, 整合了 dselect 和 apt-get的
所有功能, 并提供的更多特性,特别是在依赖关系处理上。

添加crtmpserver流媒体服务器日志目录

    sudo mkdir /var/log/crtmpserver

改变<code>/etc/crtmpserver/applications/flvplayback.lua</code>目录下的flvplayback.lua文件的某些值。

    validateHandshake=false,//验证握手
    keyframeSeek=false,//关键帧寻找
    seekGranularity=0.1
    clientSideBuffer=30//客户端缓存区

重启crtmpserver服务器

    sudo /etc/init.d/crtmpserver restart

源码方式安装ffmpeg，这一步骤是非常重要的。
Install ffmpeg from source. This step is very important. It won't work with the Raspbian version of ffmpeg because the 
Debian version of libavcodec doesn't contain the H264 libraries needed for the flash streaming protocol.

    sudo aptitude remove ffmpeg
    cd /usr/src
    sudo mkdir ffmpeg
    sudo chown `whoami`:users ffmpeg
    git clone git://source.ffmpeg.org/ffmpeg.git ffmpeg
    cd ffmpeg
    ./configure
    make
    sudo make install

Stream from raspberry pi camera to crtmpserver and wrap the raw video in flv metadata

    raspivid -t 0 -w 960 -h 540 -fps 25 -b 500000 -vf -o - | ffmpeg -i - -vcodec copy -an -f flv -metadata 
    streamName=myStream tcp://0.0.0.0:6666

Check the crtmpserver logs. You should see the incoming connection from ffmpeg show up like this:

    <MAP name="" isArray="false">
        <STR name="ip">0.0.0.0</STR>
        <UINT16 name="port">6666</UINT16>
        <STR name="protocol">inboundLiveFlv</STR>
        <STR name="sslCert"></STR>
        <STR name="sslKey"></STR>
        <BOOL name="waitForMetadata">true</BOOL>
    </MAP>

Download jwplayer from http://www.longtailvideo.com/, unzip it and place the jwplayer folder in a directory of your 
web server. I got jwplayer version 3359, YMMV. Then, place the following in an html file in the same directory of 
your web server:
    
> index.html

    <html>
    <head>
        <title>Raspbi Camera RTMP stream test</title>
    	<link rel="stylesheet" type="text/css" href="/css/index.css">
    </head>
    <body>
    	<div id="video-jwplayer_wrapper">
    		<object type="application/x-shockwave-flash" data="/jwplayer/jwplayer.flash.swf" width="100%" height="100%" bgcolor="#000000" id="video-jwplayer" name="video-jwplayer" tabindex="0">
    			<param name="allowfullscreen" value="true">
    			<param name="allowscriptaccess" value="always">
    			<param name="seamlesstabbing" value="true">
    			<param name="wmode" value="opaque">
    		</object>
    		<div id="video-jwplayer_aspect"></div>
    		<div id="video-jwplayer_jwpsrv"></div>
    	</div>
    	<script src="/jwplayer/jwplayer.js"></script>
    	<script type="text/javascript" src="/js/app.js"></script>
    </body>
    </html>
    
> index.css

    * {
        margin: 0;
    	padding: 0;
    }
    #video-jwplayer_wrapper {
    	position: relative;
    	display: block;
    	width: 960px;
    	height: 540px;
    }
    #video-jwplayer_aspect {
    	display: none;
    }
    #video-jwplayer_jwpsrv {
    	position: absolute;
    	top: 0px;
    	z-index: 10;
    }
    
> app.js

    jwplayer('video-jwplayer').setup({
        flashplayer: "../jwplayer/jwplayer.flash.swf",
    	file: "rtmp://192.168.1.124/flvplayback/flv:myStream.flv",
    	autoStart: true,
    	rtmp: {
    		bufferlength: 0.1
    	},
    	deliveryType: "streaming",
    	width: 960,
    	height: 540,
    	player: {
    		modes: {
    			linear: {
    				controls: {
    					stream: {
    						manage: false,
    						enabled: false
    					}
    				}
    			}
    		}
    	},
    	shows: {
    		streamTimer: {
    			enabled: true,
    			tickRate: 100
    		}
    	}
    });

You're done! Visit your html page and after a moment's buffering you should see video.

<b>NOTES</b>

I found I can have a consistent 25 fps at a bitrate of 500000 over a USB Netgear 8192CU Wi-Fi dongle with a delay of about 1s at best.
Other types of h264 stream players, e.g. flowplayer, are available.







