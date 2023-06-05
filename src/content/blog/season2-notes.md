---
title: "DashGL Season 2 Notes"
description: "Looking into EGL for writing tutorials"
pubDate: "October 24, 2019"
heroImage: "/img/IMG_20191018_010745.jpg"
author: Kion
---

A quick note on hardware. Bought a PsVita, it is a really impressive device. For laptops pretty much want a 14″ 1440p ips, ryzen with 8gb of ddr4 ram and 256gb of nvme storage. So it basically doesn’t exist, so I’ll have to keep watching for when something of this description fits. Preferably a Thinkpad.

For DashGL the next season of tutorials took an unexpected turn. Originally I had planned to use SDL, but it turns out that emulation station doesn’t use the X11 environment. So rather than launching X11 specifically to run one game, I decided to look into egl and running games from the command line directly, and it’s an approach that I’m pretty excited about.

Though the drawback of using egl is that there is not a lot of documentation. So I’m kind of flying blind on this one. I’m thinking that the approach for this set of tutorials isn’t that I’m some kind of guru, but more to present some examples to act as a place to get started, and maybe people more familiar with the subject can fill in on the details.

Right now we’ve managed to get the first triangle down which took several hours of cleaning out the program included with the pi to get it down to the “hello world triangle”. Next step is to include the shader install part of the application as a library. So I’ll probably be writing about my issues with that in the next post.