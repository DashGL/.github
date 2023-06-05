---
title: "Hardware Plans"
description: "Throwing random thoughts for making a desktop setup that runs on USB"
pubDate: "July 31, 2019"
heroImage: "/img/pi-desktop-1024x626-1843586342.png"
author: Kion
---

### Router Pi 

A pi that acts as a router, dns and wifi access point for the rest of the network. To avoid overloading it, not much should be included for the micro-SD card. Nginx can be used for hosting static content for dashgl. (8GB)

### Latte Panda

Set this up with CentOS 7, to be used with the FBX SDK, Amazon Kindle SDK, and to host KVM guests on the network. 256GB of micro-SD card should also be included for storing roms and rom files on the network via samba. Nginx can also be used to host static files.

### Libreboard

Set this up with Ubuntu or Debian (which ever one works), and set up apache, php and mariadb to host a next cloud server on it, and also host images and media over the network with samba.

### PSP

psx and psp. 64GB or 256?

### Pi test bench

Raspberry pi 3 with 5″ 800×600 monitor running retro pi for running and testing ports, also Use nes romhacks, classic snes games, gba

### Pi screen test 1

Raspberry pi 3 running 2.4″ 320×240 spi interface

### Pi screen test 2

Raspberry pi 3 running 3.5″ 320×240 hdmi interface

### Pi Screen test 3

Raspberry pi 3 running 3.5″ 800×600 spi interface

### Pi Desktop

Raspberry pi 3B+ with i3wm desktop 64GB

Pretty happy with everything except for the redundant devices hosting services on the network. The latte panda is the one device that I definitely need to use because of the KVM host. I wonder if I can install to the 32GB of included eMMC for kvm (hosting windows xp for daemon tools), fbx sdk and kindle. And then I could host the network with a usb wifi adapter (for just my computer and cell phone). And then use a combination of storage for the applications (nextcloud -> micro-sd, samba-> usb), and then use nginx to host multiple domains locally.