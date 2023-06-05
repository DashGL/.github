---
title: "Hardware Selection for handheld concept"
description: "Looking into a screen hat for the raspberry pi to use as reference hardware"
pubDate: "July 23, 2019"
heroImage: "/img/61n2TiuseIL_SX522_.jpg"
author: Kion
---

For the screen we can use: [WINGONEER Raspberry Pi 3.5inch 800×480 60fps](https://www.amazon.co.jp/dp/B076P947BM/?coliid=I10C3Q1QRNXTV&colid=1Q0YAGH44KJ4F&psc=1&ref_=lv_ov_lig_dp_it), which uses the GPIO. I think the documentation for this can be found in the raspi-wiki on [this page](http://www.raspberrypiwiki.com/index.php/3.5_inch_TFT_800x480@60fps). Assuming that this is the right display, then we need to write the following into the `/boot/config.txt` file.

```
#for raspberry pi 3b+/3b/2b+/b+
dtoverlay=dpi18
overscan_left=0
overscan_right=0
overscan_top=0
overscan_bottom=0
framebuffer_width=800
framebuffer_height=480
enable_dpi_lcd=1
display_default_lcd=1
dpi_group=2
dpi_mode=87
dpi_output_format=0x6f005
hdmi_timings=480 0 16 16 24 800 0 4 2 2 0 0 0 60 0 32000000 6
display_rotate=3
```

![](/img/61a9ycBOUkL_SX522_.jpg)

For the Battery I think we can use this [UPS hat](https://www.amazon.co.jp/dp/B07K7J9JSR/?coliid=I3B1HT8VIOI3BZ&colid=1Q0YAGH44KJ4F&psc=1&ref_=lv_ov_lig_dp_it). Since it stacks, I think that means we can use a Raspberry Pi 3 (and underclock it down to match the Pi Zero), to get wifi and bluetooth. I don’t think we need the Raspberry Pi 3B+ or 4, since we generally don’t need the heat or extra power, since we’re generally going for 2d at 800×480.