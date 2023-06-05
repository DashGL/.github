---
title: "Getting Jiggy with the Latte Panda"
description: "Throwing all of the smaller single board computers into one single board computer"
pubDate: "August 12, 2019"
heroImage: "/img/Latte-Panda-4183295904.jpeg"
author: Kion
---

Originally I was thinking of using multiple single board computers on a network to perform different tasks. But after buying a 256GB micro-SD card which I had intended to use for a Raspberry Pi, I had a change of heart. The latter panda has 32GB of on board storage which I can use to host the OS, and then with 256GB of extra storage, rather than having a lot of different devices on the network, I can probably install everything to the latter panda.

So what’s the plan? There are a bunch of points I want to hit on this. I guess I should start with the high priority ones and work from there.

### Access point

I want to be able to host a wifi network and have clients, my laptop and cell phone, connect to this. The advantage of doing this is I can set up a DNS to be able to define name for locations on the network. And not have to remember ip addresses of my network clients, or have to manually define a DNS for each client on the network.

### KVM

For the most I don’t need to use windows for common tasks. The one thing I find myself using windows for is disk mounting tools like Daemon tools lite. So I want to host a virtual windows machine that I can use to mount a disk image and extract the files from the disk on the network.

### Samba

This is a pretty simple, samba is a useful utility for hosting files on a network.

### Nextcloud

I tried hosting this on a Pi and it was slow. So I want to try the latte-panda to see if works better with an intel processor and 2GB of RAM.

### Approach:

In terms of approach I want to host the system on the 32 GB of internal storage and then /var on the 256 GB micro-sd card. Next I want to try getting the access point. And from there KVM. For KVM i can probably get away with the NAt interface as I plan on using everything with samba, so I don’t need a bridged network or anything like that. For samba i can host the files on `/var/data`. And for nextcloud I can store the files on `/var/www/apache`, or `/var/www/nextcloud`? I think I want to use apache for nextcloud exclusively and then use nginx as a reverse proxy server.

The latte-panda has a built in antenna for wifi. Unfortunately I throw it away thinking that I wouldn’t need it. In terms of setting up a wifi access point, I think I can do it just fine with a usb wifi dongle, but it definitely adds more lights to something that seems to keep gaining more and more light sources. Having built in wifi and being able to use the onboard micro-sd card would have gone a long way to making a more sleek network host. I guess I’ll just have to keep going.

For making an access point, I have two network devices. The ethernet `enx00e04c367344`, and wifi, wlan0. So I need to install and configure hostapd, install dnsmasq. Set up the wifi interface, and forward the traffic with iptables.