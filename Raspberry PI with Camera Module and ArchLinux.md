# Raspberry PI with Camera Module and ArchLinux

I reccently bought a Raspberry PI. I am new to ArchLinux so I thought I'd make some notes of the initial setup. 
This is the hardware used to build the camera:

* Raspberry Pi Model B 512MB RAM
* Camera Module for Raspberry Pi
* WiFi USB Nano
* OpenBox Sky Case
* 16GB SD

## 1. SD卡分区

I added the ArchLinux image to a 16GB card. The images creates a 2GB parition so you need to either extend it before 
booting or create a new partition after boot. I choose to create a new partition after boot, and use it for /home .

    fdisk /dev/disk/by-id/mmc-*
    n add partition
    w save and exit

创建文件系统

    mkfs.ext4 /dev/mmcblk0p3

Remove everything in /home . I am assuming you have nothing important here yet.

    rm -rf /home/*

Mount the device at boot.

    /dev/mmcblk0p3 /home ext4 defaults 0 2

## 2. Package manager

I had never heard of Pacman package manager before. But its just as easy. To search for packages do this.

    pacman -Ss partOfPackageName

To install a package do this.

    pacman -S packageName

Upgrade all packages.

    pacman -Syu

Initiallly I had some problems when installing packages. I got this.

    "from mirror.archlinuxarm.org : The requested URL returned error: 404 Not Found"

Just perform a full uppgrade and it should be resolved.

## 3. 网络设置

To list available devices do.

    ip link

To get the wireless network running I installed these.

    pacman -S wpa_supplicant
    pacman -S wpa_actiond
    pacman -S ifplugd
    pacman -S dhclient
    pacman -S openntpd

There is a very nice setup wizard, just do this.

    wifi-menu -o

Network configuration exists in profile files. I had some problems with the DHCP client not setting IP:s after 
network loss. I added DHPClient to the profiles to make it reconnect.

    #/etc/netctl/ethernet-dhcp
    Description='A basic dhcp ethernet connection'
    Interface=eth0
    Connection=ethernet
    IP=dhcp
    DHCPClient='dhclient'
    TimeoutDHCP=30
    ExecUpPost="ntpd -s &"


    #/etc/netctl/wlan0-tallefjantlinksys
    Description='Automatically generated profile by wifi-menu'
    Interface=wlan0
    Connection=wireless
    Security=wpa
    ESSID=MYNETWORKNAME
    IP=dhcp
    Key=12312312312313123132
    DHCPClient='dhclient'
    TimeoutDHCP=30
    ExecUpPost="ntpd -s &"

To make WLAN connect when available, and Ethernet connect when plugged, do this.

    systemctl enable netctl-auto@wlan0.service
    systemctl enable netctl-ifplugd@eth00.service
    
To make WLAN and Ethernet use DHCP at boot, do this.

    systemctl enable dhcpcd@eth0
    systemctl enable dhcpcd@wlan0

I noted that the network setup was not working after the first initual upgrade with Pacman. To solve it I just 
disabled the enabled services and enabled them again.

    systemctl status --failed
    systemctl disable FAILEDSERVICE

I had problems with DHCP timeouts so I added the TimeoutDHCP parameter and set it to 30. Default is 10 seconds. 
Also the ExecUpPost will set time from NTP when connected.

## 4. 时间和日期

In my case, Sweden.

    rm /etc/timezone
    ln -s /usr/share/zoneinfo/Europe/Stockholm /etc/timezone

Also, the Raspberry Pi has no battery. The time will be 1970 on every reboot. You can use NTP to set the date from 
a time server on startup.

    pacman -S openntpd

To start ntpd when a network interface is connected, add ExecUpPost to your interface profile. Here is an example of 
my eth0.

    Description='A basic dhcp ethernet connection'
    Interface=eth0
    Connection=ethernet
    IP=dhcp
    ExecUpPost="ntpd -s &"

## 5. Raspberry Camera

In ~/.bashrc I added.

    export PATH=$PATH:/opt/vc/bin

In /boot/config.txt I added.

    gpu_mem=128
    start_file=start_x.elf
    fixup_file=fixup_x.dat

The camera may seem slow. There is a default delay of 5 seconds before it takes the picture. You can change this 
with "-t 0".

I've noticed the device hangs when storing larger videos. It's a good idea to record video directly to RAM, that 
works much better for me. By default /tmp is mounted as tmpfs.

    /opt/vc/bin/raspistill -t 0 -o /tmp/test.png
    /opt/vc/bin/raspivid -o /tmp/out.h264 -t 5000
    /opt/vc/bin/raspivid -o /tmp/out.h264 -t 20000
    /opt/vc/bin/raspivid -o /tmp/out.h264 -t 60000
