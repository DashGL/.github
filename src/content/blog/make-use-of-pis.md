---
title: "Making Use Of Raspberry Pi’s"
description: "Better Utilizing the Random SoC's I have lying around my house for hosting"
pubDate: "December 5, 2021"
heroImage: "/img/20230215_041432.jpg"
author: Kion
---

Right now I have a bunch of Pi’s sitting around my house. And I keep thinking that I can use them for different small projects, but in reality it seems like I get busy with work and family, and then I never get around to actually messing around with the Pi’s. And right now something I do pretty poorly is managing my VPS’s. I have a bunch of random things scattered around a bunch of VPS’s. And now that I have fiber optic internet, it seems like I should try and make better use of local resources on my network.

There is the option of trying to move everything to free hosting on Github. I tried using Gatsby, but it seemed like a lot of work to try and convert a site to use it. Mainly it seemed like a bad investment. Something like NextJS would allow me to set up projects as well as static sites, so going forward that seems like the right tool to use. Converting what I have doesn’t seem like the best use of my resources at the moment.

What kind of things will I be looking at?

– Moving did.shellshop.lol  
– Probably moving shellshop.lol in general  
– Moving this blog  
– Setting up a NextCloud instance  
– Setting up a network share

The main crux is how I want to manage my DNS, and hosting. For the DNS, the most direct option would be to set up my Raspberry Pi Zero W to be a dedicated Pi-hole instance. I tried to use it for taking notes, the problem is that there doesn’t seem to be reliable way of running Nodejs on the Pi Zero, so otherwise it’s only been holding static contents. If the Raspberry Pi Zero W was able to run Nodejs, it seems like it would be a great little development server. This is why I ordered a Raspberry Pi Zero 2W, so we’ll shuffle things around and see how that works.

Mostly I think we’ll be running into https problems around hosting. I have two options. The first is to use a VPS to reserve a static IP and then forward all of the traffic to the Raspberry Pi, or the second option is to use a dynamic DNS. And since I’m trying to reduce the number of VPS’s i have, the dynamic DNS approach seems to be more in the spirit of what I’m going for.

It looks like in terms of practical application. I’m getting a 504 error timeout when I try to access my local network remotely. And the Raspberry Pi’s connected to my LAN keep dying for some reason. I think the easier option might be to simply to rent a cheap VPS that will act as an NGINX server to validate Let’s Encrypt, and then use a VPN to reverse proxy the request back to my local network. In general I’m not in too much hurry to do this. I might start by making a list