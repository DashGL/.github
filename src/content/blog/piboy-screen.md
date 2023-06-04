---
title: "Piboy Screen"
description: "Looking at the waveshare screen for the Raspberry Pi"
pubDate: "November 1, 2018"
heroImage: "/img/P_20181030_093303.jpg"
author: Kion
---


So after making pages about different hardware projects, it looks like the Piboy Advance is the easiest place so start. I ended up using this driver (https://github.com/juj/fbcp-ili9341), which ended up being a lot easier than I thought it would be. Rather than installing some blob with god-knows-what in it, you clone the code, compile it and then execute the binary. And it only took me a couple of tries with trial and error to get something that worked.

```
$ cmake -DWAVESHARE35B_ILI9486=ON -DSPI_BUS_CLOCK_DIVISOR=30 -DDISPLAY_ROTATE_180_DEGREES=ON ..
```

There’s still the matter of tuning the display. Which seems to be kind of hit and miss. The framerate is displayed in the upper lefthand corner. Except the pi needs to be running some kind of task to actually display this number. So Open Tyrian seems like a good candiate for testing. I’ve found several install instructions for Tyrian on the internet, but I’ve never had any luck with it. I decided to poke around in the RetroPi github repository and finally found something that works, mostly by shear stupid luck. Though unfortunately it doesn’t look like my bash history was saved for some reason. So if I come across it later. I’ll have to write a post just for that.

![](/img/IMG_20181101_024316.jpg)

We have a completely different and more fundamental problem that this screen is completely garbage. It’s true that I haven’t done any tuning to make the screen have the right refresh rate. But more fundamentally the backlight bleed on this screen is so bad it’s hard to make out anything. I’m going to try and track down an adafruit screen and see if that makes an improvement.

Edit:

After looking around at screen hats, it seems like it would pretty much be easier to use the official screen. Which I’m not a fan of, because it blocks access to the sd card slot. I think it might be easier to set the usb boot bit, and then use a [usb device](https://www.amazon.co.jp/dp/B00C5K8E1A/ref=sspa_dk_detail_0?pd_rd_i=B00C5K8D5W&pf_rd_m=AN1VRQENFRJN5&pf_rd_p=35261a28-eed5-46a8-9369-308fa0c478f8&pf_rd_r=AHTCZBHZ7S2Q009A8BBP&pd_rd_wg=py9Ai&pf_rd_s=desktop-dp-sims&pf_rd_t=40701&pd_rd_w=McpjD&pf_rd_i=desktop-dp-sims&pd_rd_r=46bef607-dde1-11e8-afc8-19cdea3ec1a8&th=1) for booting. That way I can switch out usb’s without having to take the whole thing apart each time.