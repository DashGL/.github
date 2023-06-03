---
title: "Getting Reorganized"
description: "Trying to consolidate SoC devices using the Gekkworm x830 case and mass storage"
pubDate: "September 25, 2018"
heroImage: "https://img.dxcdn.com/productimages/sku_569673_5.jpg"
author: Kion
---
 

In order to consolidate my Raspberry Pi’s I bought the Gekkworm x830 case, and already I’ve having second thoughts on the purchase. The case itself does what it’s advertised to do, which is basically house a Pi with a 2.5″ hard drive. The downside is I bought a 5400RPM 2TB hard drive for it and I’ve forgotten about how stupid physical medium is, so I might have to open it up and replace the hard drive with my left over 256GB 2.5″ SSD drive.

After that the plan was to set up the Pi as the access point for all of my devices. So I’ll need to set up the wifi so that my other devices use it to connect to. Also one thing I’m not sure about with the aluminum case is the resulting on board wifi, so it’s probably a good idea to use a usb adapter to get better signal strength. One thing I’m not sure about is the wifi drivers for most of the usb devices, so I’ll have to look into which devices have which drivers, and then order a new usb dongle for that.

After setting up the ssd and the wifi dongle. The next step will be to set up nginx to host any projects I might have on it. Specifically that will mean editing the dns settings so that I can add dns settings for the devices I connect to it. And specifically for the case of nextcloud, have a cloud server to manage validating a certificate with let’s encrypt and then syncing that with my home router.
