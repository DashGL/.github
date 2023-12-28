---
title: Updates for September 2023
description: Status Report on Trying not to be a low-life loser
pubDate: Sept. 10, 2023
heroImage: /img/sept-2023-blob.png
author: Kion
---


Hey guys kion here and I haven't posted anything since updating the site with AstroJS. After updating I wanted to improve on blog content, but I didn't have any practices in place that would insure that. What I've started to do is create tasks in ClickUp. That allows me to draft an article there, and then I can copy and place that directly into GitHub. Another reason in general for delays is that I've been living in Japan for the last 15 years and want to move back to the states for a better work-life balance. The problem is that my wife's green card application has been in the works for the last two years. And on top of adding stress, it takes a lot of time away from productivity to do paperwork. I'm hoping things end soon, but a lot of that depends on the government's willingness to force people to take experimental medical treatments against their will. 

## DashGL Site Updates

One thing I have done is spend a lot of time improving the look and feel of the DashGL site. This is using AstroJS for the framework and Tailwind and Flowbite for the CSS. What I like about AstroJS is that it makes it really easy to have a list of markdown files. Change or add one and it redeploys. This is in contrast to something like GatsbyJS where you content can live elsewhere and gets pulled in before being packaged and deployed. 

The way the site is structured now is that the landing page exposes content from the rest of the site. So as I add things, these parts should be automatically updated with their respective sections.

I've ported over all four of the sad tutorials I have. The inspriration for tutorials was that as I was reverse engineering Phantasy Star assets, one thing that kept bothering me is, "what does the program look like to render this"? And that was the motivation for DashGL, to be able to get to the point of being able to render playstation-level 3d graphics on something like a Raspberry Pi. 

The problem with tutorials is that they're very timing consuming with respect to trial and error. I have a few other sample applications I've made with OpenGL 1.0 that I could port here. But I think I'll go over tutorials in more detail in the next monthly update. 

Then there's the blog. For the blog I ported over everything from my old wordpress site, which is why most of the blogs are small notes that I started writing and then never really completed a thought. I've updated my process where I'm using ClickUp as a way to track and draft blog posts. And then I can copy and paste them over.

And the last part is resources. One part about doing everything open source is compatibility. There are things like SketchFab or the Unity Store. But their license generally doesn't allow for the free use of models. One thing I'm trying to do here is provide assets that are free to work with for open source programs. Where you're not having to worry about a license problems.

The other functionality that this section provides is links and descriptions of useful tools or Github projects. And I was going to use it to highlight cool fan projects. Now that I think about it. I'm probably overloading this section and I might try to move more of that to the blog. And be better about adding tags to articles to be able to filter them. 

And last is the Dash Model Format. To be honest up front the need for this had been entirely replaced by glTF. But I'm keeping it around for two reasons. The first reason is that it's definitely helpful to me to have data format to map to. And then I can use that as a way to debug or work with other formats or programs by being able to have something to compare to.

The second reason that I want to keep it around is because there's a huge gap between something like obj, and gltf/fbx in terms of complexity and functionality. If you want to get into this stuff, I specifically designed the format to be very simple but be able to handle bones and animations. So if you're writing your own software, this would make it easy to be able to understand what's going on inside, and not be dependent on other people for support. 

## Commissions

![vitascky](https://github.com/DashGL/dashgl.github.io/assets/25621780/cacb61bf-186e-428d-8f1e-987e251fc6c7)

I want to highlight some awesome resources. First we have the Chibi-Linux mascot pack. This is from Vic. These characters look super cute. And they are completely free and open to use. These are by [vitascky](https://sketchfab.com/vitascky). I'll need to update the resource page with downloads for these. 

![Dashie and friends](https://github.com/DashGL/dashgl.github.io/assets/25621780/7856e0b0-ac4e-437a-a043-de3c5cf12003)

And second is this amazing scene from [GeckoBoyAdvance](https://twitter.com/geckoboyadvance). He has some amazingly detailed images in his portfolio. I love all of the tiny little details that come out in this image.

And last is this spicy picture of Dashie (not shown) by [DBP](https://twitter.com/demonbloodpal). This is going to be an easter egg for the 404 page. Hey, I need to have fun somehow. I need to create the default 404 page. But once I do, i might be easy to accidentally on purpose load a missing page. 

## Megaman Legends

I'm inching my way through Megaman Legends 2. I'll get a small window to make progress on this before real life comes up and hits me with a baseball bat. I traced through the offset to get the Megaman character. The next step will be will be to look into the animations for Megaman, and while I'm doing that I'll also be looking at the high poly characters that are included in the game files.

As a side note, I'm tempted to come back and update `mml.dashgl.com`. Now that I have a blog, I'll probably want to move this content over there. And when I do that I'll do a domain redirect so that it points to the blog and filters on the tag. 
