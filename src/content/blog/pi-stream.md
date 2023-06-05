---
title: "Streaming from Framebuffer"
description: "Streaming from a Raspberry Pi, but I'm not sure why I looked into this"
pubDate: "July 15, 2019"
heroImage: "/img/maxresdefault-555505230.jpeg"
author: Kion
---

https://lists.freedesktop.org/archives/gstreamer-devel/2013-July/041965.html

How to specify 16 bit, 32 bit?  
How to specify width and height?  
How to create new virtual interface?  
How to stream data?

`sudo ffmpeg -f fbdev -r 20 -i /dev/fb0 this\_test.mpg`

https://www.acmesystems.it/ffmpeg  
[Raspberry Pi Security Camera](https://prabg.wordpress.com/2018/03/24/raspberry-pi-3-security-camera/)

Raspberry Pi Case: https://www.tindie.com/products/Ampersand/null-2-kit/  
Jsmpeg: https://github.com/phoboslab/jsmpeg  
Framebuffer Png: https://github.com/AndrewFromMelbourne/raspi2png