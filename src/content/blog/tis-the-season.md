---
title: "Tis the season to tinker"
description: "Laptops, desktops and servers, oh my"
pubDate: "January 2, 2019"
heroImage: "/img/th-748851066.jpeg"
author: Kion
---

Not sure what it is about late December, and early January that makes me want to tinker with tech. Maybe it’s just having a little bit of freetime to want to put something together. So this year I’ve been thinking about how to manage my hardware, and there don’t seem to be too many promising options. Since everything I do in my workload is already covered by the A-series APU mini-itx I put together last year (right before the release of the Ryzen APU’s). The only issue is that I’m using that as a server right now, so I’m going to need to find a replacement server and move everything over if I want to be able to use it as a desktop again. Right now there seems to be some new compelling options in the next year or so, and we should see some more anouncements at CES this year. And I’m really looking to start adopting ARM and RISCV options as they become available over the x86 architexture.

### Laptops

I don’t even remember when I bought by Zenbook. I think it’s one of the earlier models, but even with 13.3″ FHD, 4GB of RAM, 128GB of SSD storage, and Intel integrated graphics, there hasn’t been any device compelling enough to replace it (for a reasonable price). As while the CPU’s have gotten faster, and generally come with 8 or 16GB of RAM and atleast 256GB of storage, screen resolutions on laptops seems to be eternally frozen at FHD. If I do buy another laptop, I’d want something like the LG gram 15, which is able to act as a desktop replacement while still being thin and light. The problem is that this class of laptops generally doesn’t exist, and when it does is stupidly expensive. So in terms of laptops, we’ll have to see if anything comes out either with an ARM cpu and Linux (super not likely), or if anything cheap enough comes with a screen 1440p or higher.

Benchmark: [HP 15-db0000](http://jp.ext.hp.com/notebooks/personal/hp_15_db0000/kakaku.html?jumpid=st_cn_p_af_kkc&sisearchengine=130&siproduct=kakaku) Comes with a Ryzen 3 2200U, 4GB of RAM, 1TB 5400RPM HDD, and a 15.4″ Full HD screen for around five hundred dollars. RAM can be upgraded, and the hard drive can probably be swapped out.

### Desktop

For desktops the main issue I have is with the mini-itx form factor. The inwin chopin seems to be the smallest case widely available and even that seems like it could be slimmed down a bit. With m.2 form factor ssd’s having slots right on the mother board, it seems like you could make a smaller case with a low profile fan and an APU that would be pretty small. I’m also not happy with the power supply that the Chopin comes with. I’d be really tempted to make a micro-atx ‘sleeper’ build using the Fractal Design Core 1100. Though one difference from not living in the states is that PC parts aren’t very cheap, and it often ends up being a better deal to buy a complete computer.

Benchmark: [ThinkCentre M715q](https://www.lenovo.com/jp/ja/kakaku/desktops/thinkcentre/m-series-tiny/ThinkCentre-M715q-Tiny/p/11TC1MT715Q?cid=jp%3Aaffiliate%3Ag2ospo) Ryzen 3 200GE, 4GB of RAM, 128GB of nvme SSD with two display ports for around four hundred dollars. RAM can be upgraded, and storage can probably be added or swapped out.

### Server

While acting as a server shouldn’t be something that has a particulary high requirement, as it’s something that just sits around. At work we have a lot of computers, so it’s pretty easy to forget what role different servers are playing. So the server should have some kind of novalty or be simple to manage. At this point, I think the Node 202 could be the most ‘server-like’ mini-itx case, as it’s a flat case, that holds a normal power supply, has room for storage, and then has room for a pci-express card, if it’s utilized or not. So buying that case could be a possibility. And then that would free up my AMD APU computer, or maybe I could just reinstall Linux on the server that died because of the XFS file system, as I would basically put its corpse inside of there. Either way we have a couple of options, and that would free up my linux box. At home I have a lot of Raspberry Pi’s, so I should probably continue to keep utilizing those to make the most out of what I have, or use my local machine with Gitlab and my cloud server to reduce the use entirely.