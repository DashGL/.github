---
title: "Pi Failure"
description: "Rethinking Raspberry Pi Integration: Lessons from an Access Point Experiment"
pubDate: "October 1, 2018"
heroImage: "/img/P_20180930_082011.jpg"
author: Kion
---

I think this experiment might go into the “failed” bin. At home I’ve been increasingly using Raspberry Pi’s to replace any computing needs I might need on a network. Why raspberry Pi’s? Because their ubiquitous, it’s easy to find documentation for them, they only require a micro-usb to power them, generally that amount of computing power is more than enough for a web server, and most importantly it’s a good learning experience to create your own devices and match them together rather than buying something complete.

Specifically my goal for this case is to have an access point for my house. This goes hand in hand with my Android to Lineage experiments. But basically I want to have LineageOS on my cell phone. Then I want to host NextCloud at home. And then to use NextCloud on my cellphone, my NextCloud instance needs a valid https certificate. Which means that my plan was to use my super-cheap cloud server to verify my certificate, have a Raspberry Pi act as an access point, and then use nGinx to forward the traffic back to my NextCloud instance.

Though as I’m writing this I’m realizing a pretty big mistake that I was making. My intention was to make a single device that would perform all of these functions. But In reality I think it would make a lot more sense to simply have a barebones access-point with minimal functionality on it, (just the access point and a DNS server), and then start adding Raspberry Pi’s into the network with fixed ip’s and DNS entries and just have them all forwarded from the central access-point-pi.