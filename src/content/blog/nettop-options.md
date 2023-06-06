---
title: "Playing around with Nettops"
description: "Looking for slim and light desktop computers that can run multiple monitors"
pubDate: "March 29, 2022"
heroImage: "/img/article-1280x720.jpeg"
author: Kion
---

It looks like I’ve switched from an obsession to netbooks to an obsession with nettops. Since it makes more sense to be able to swap the screen, keyboard and mouse depending on what functionality I want the computer to provide. I think there is functionality that is provided by laptops but it’s much more of an investment.

A laptop is essentially a computer that comes bundled with a fixed screen, fixed keyboard, and fixed mouse (in the form of a trackpad). And while you may have the option to upgrade the screen, RAM or Hard disk. There isn’t much you can do to change anything outside of those few factors, expect by completely buying a new laptop.

There’s also the issue that laptops are somewhat of an oxymoron when it comes to specs. Better specs will increase what you can do on a laptop, but it comes at the cost of more heat, more weight, and less battery life which are aspects that are not desire-able with when it comes working with a mobile computer.

Right now I have four nettops and it allows me to test different configurations with respect to computer hardware. And I’m only buying and storing the nettop which helps save on both space and costs for testing different configurations. The four configurations I have are as follows.

1. Windows 11 modern  
2. Windows XP classic  
3. Ubuntu 21.10 Ultra-wide  
4. Ubuntu 21.10 Dual Screen

On the Linux-side of things, I’ve found Ubuntu to be the clear winner for a distribution. It’s the most common one to work with, and the easiest one to find information for. It’s also the easiest to make the assertion that anything that works for you on a certain release of Ubuntu, should work the same for other people with the same distribution. What also helps is that the Gnome experience is getting really polished. I’m really looking forward to a lot of the UI updates coming in Ubuntu 22.04, and aside from the login and tiling, I have very little issuing with the overall desktop experience.

As far as ultrawide versus multi-monitor, i find multi-monitor to be a vastly superior experience. The first issue with a lack of default tiling on Gnome makes it much easier to use a multi-monitor setup to snap Windows to the left or right of the screen. I haven’t tried something like PaperWM, but for my workflow I often use Gimp and the File Manager to cascade windows ontop of each other. If I had a console-only workflow then I would probably be using i3wm/sway already.

Where multi-monitor really shines is full screen. If I run a game or video on my ultrawide screen, then a large portion of the screen ends up being used for black bars. Where as with a multi-monitor setup I can run something in full screen, and then still have another screen for space. Combined with the quick window snapping, multi-monitor is a much better experience.

Where things take an interesting turn is Windows. I can’t say I’ve used Windows very much for the last ten years. The main reason for having a Windows computer is that I used Noesis, I don’t like using Wine, and I prefer to avoid hosting a virtual version of another operating system. So my thinking was to use Windows as a Remote Desktop Server. That way I could use Linux as my main driver, and then tap into Windows remotely as needed.

Since I’ve taken the time to set up a windows computer, I wanted to take the opportunity to see what games could be played natively since I avoid using Wine. In general it looks like games like Typing of the Dead Overkill, Resident Evil Revelations, and Starcraft 2 just work. I would also probably install Phantasy Star Universe all without having to deal with Wine.

In the case of older games, I wanted something that could be run on period accurate hardware. Which is why I went to the extent to find an nettop for running Windows XP on. From my testing it looks like broken games are just broken and they work on Windows 11 just as well as they would have on Windows XP. Mostly I bought it to tinker with and do something for fun. I mostly want to play around with hardware that I didn’t have a chance to play around with when it was new.

Once we’ve done testing with Windows, we can find out what approach works best. Remote desktop, virtual host, or bite the bullet and attempt to work with Wine. But in cases like Noesis being able to see and work with the Windows file system is pretty important so having a working Windows desktop is pretty important when it comes to installing, and testing games. So at this point I would mostly lean towards the emulation approach. And virtual box allows me to seemlessly run a Windows desktop on a different monitor.

Notes: [GIGABYTE BRIX PRO GB-BSi5-1135G7-BWUS Mini](https://www.newegg.com/gigabyte-gb-bsi5-1135g7-bwus-brix-pro/p/56-164-160)