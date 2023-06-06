---
title: "Comparing Different MML2 Versions"
description: "Comparison of the different releases of PC, PSX, PSP and JP vs EN"
pubDate: "November 13, 2022"
heroImage: "/img/Rockman-Dash-22-1647619519.jpeg"
author: Kion
---

There are four released version of Megaman Legends 2. These are:

1. PC Japanese Version
2. PSX Japanese Version
3. PSX English Version
4. PSP Japanese Version

The English and Japanese PSX versions are effectively the same as far as file structure and formats go. With the exception for translated text and some minor differences in textures. 

The PC version version takes quite a different approach as far as file structure goes. It takes a different approach for archiving files into dat files which don't have any padding inbetween them compared to the Playstation version which seems to add bytes inbetween each file to align to the CD boundaries. 

The files themselves should be similar if not the same, so extracting and comparing the buffers is another comparison that I might come back and check. 

What I'm most interested in right now is the differences or similarities between the PSX and PSP Japanese versions. My hypothesis is that these are effectively the same games, and the only change was made to the executable to change the game from 4:3 to 16:9 and probably made some slight changes to draw distance for what could be loaded in and out of the game's culling field. 

I would be extremely happy if the PSP version fixed the issue I have with the game reusing the same framebuffer coordinates for each scene because of the Playstation's limited 1MB of video memory. I was thinking this could be fixed in the PSP, but now that I check, the PSP only has 2MB of video RAM!!!??

More importantly I would be surprised if the dev's broke something that wasn't broken. The game already loads in textures and palettes as needed. So unless they wanted to save a few more milliseconds on a few room loads, I don't think there would be any need to remap the game's textures. But having them overlap less would make things easier for me to match them up. Which is what we're checking for.

We're going to start with the `COMMON` folder. Which contains the following files.

- DEMO.BIN
- GAME.BIN
- G_OVER00.BIN
- G_OVER01.BIN
- G_OVER02.BIN
- INIT.BIN
- LOGO.BIN
- PL00P000.BIN
- PL00P001.BIN
- PL00P002.BIN
- PL00P003.BIN
- PL00P004.BIN
- PL00P005.BIN
- PL00P010.BIN
- PL00P011.BIN
- PL00P012.BIN
- PL00P013.BIN
- PL00P014.BIN
- PL00P015.BIN
- PL00R01.BIN
- PL00R02.BIN
- PL00R03.BIN
- PL00R04.BIN
- PL00R05.BIN
- PL00R06.BIN
- PL00R07.BIN
- PL00R08.BIN
- PL00R09.BIN
- PL00R0A.BIN
- PL00R0B.BIN
- PL00R0C.BIN
- PL00R0D.BIN
- PL00R0E.BIN
- PL00R0F.BIN
- PL00R10.BIN
- PL00T.BIN
- PL00T2.BIN
- PL01P000.BIN
- PL01T.BIN
- PL02P000.BIN
- PL02T.BIN
- PL03P000.BIN
- PL03T.BIN
- PL04P000.BIN
- PL04T.BIN
- PL05P000.BIN
- PL05T.BIN
- PL06P000.BIN
- PL06T.BIN
- PL07P000.BIN
- PL07T.BIN
- SELECT.BIN
- STAFF.BIN
- SUBMAP.BIN
- SUBSCN.BIN
- TITLE.BIN

Of these only the following four files are different:

- DEMO.BIN
- GAME.BIN
- INIT.BIN
- SUBSCN.BIN
- TITLE.BIN

Which makes sense since these are files related to the title screen. So the files for Megaman's model that gets loaded in during the game are the same. 

From there we can shift our attention to the `DAT` folder. Which doesn't contain exactly the same files. The PSP contains the following exclusive files.

- CAPCOM.TIM
- NOW_LOADING.TIM
- STFF.BIN
- STFFT.BIN
- joydata1.bin
- joydata2.bin
- joydata3.bin

And we have a similar story to the `COMMON` folder, where the main difference is cosmetic with respect to the platform changes between PSX and PSP. But otherwise most of the core gameplay content we can expect to be the same. Or at least that's what I thought. Looking at the check sums in the `DAT` folder, the following files where different.

- ST01.BIN
- ST01T.BIN
- ST04.BIN
- ST04T.BIN
- ST05T.BIN
- ST06T.BIN
- ST08.BIN
- ST08T.BIN
- ST0B.BIN
- ST0BT.BIN
- ST10.BIN
- ST10T.BIN
- ST14.BIN
- ST14T.BIN
- ST15T.BIN
- ST17.BIN
- ST1703.BIN
- ST1703T.BIN
- ST17T.BIN
- ST19.BIN
- ST1902.BIN
- ST1902T.BIN
- ST19T.BIN
- ST1B.BIN
- ST1BT.BIN
- ST1D.BIN
- ST1DT.BIN
- ST1E.BIN
- ST1ET.BIN
- ST1F.BIN
- ST1F01.BIN
- ST1F01T.BIN
- ST1F02.BIN
- ST1F02T.BIN
- ST1FT.BIN
- ST23.BIN
- ST23T.BIN
- ST24.BIN
- ST24T.BIN
- ST2502.BIN
- ST2502T.BIN
- ST25T.BIN
- ST26.BIN
- ST2601.BIN
- ST26T.BIN
- ST28.BIN
- ST28T.BIN
- ST29.BIN
- ST2901.BIN
- ST2901T.BIN
- ST29T.BIN
- ST2A.BIN
- ST2AT.BIN
- ST2D.BIN
- ST2DT.BIN
- ST2E.BIN
- ST2ET.BIN
- ST2F.BIN
- ST2FT.BIN
- ST30.BIN
- ST3001.BIN
- ST3001T.BIN
- ST30T.BIN
- ST32.BIN
- ST32T.BIN
- ST33.BIN
- ST33T.BIN
- ST35.BIN
- ST3501.BIN
- ST3501T.BIN
- ST3502.BIN
- ST35T.BIN
- ST36.BIN
- ST36T.BIN
- ST37.BIN
- ST37T.BIN
- ST38.BIN
- ST38T.BIN
- ST3C.BIN
- ST3C01.BIN
- ST3C01T.BIN
- ST3CT.BIN
- ST3E.BIN
- ST3ET.BIN
- ST3F.BIN
- ST3FT.BIN
- ST40.BIN
- ST40T.BIN
- ST48.BIN
- ST48T.BIN
- ST4A.BIN
- ST4AT.BIN
- ST4B.BIN
- ST4BT.BIN
- ST4C.BIN
- ST4CT.BIN
- ST4D.BIN
- ST54T.BIN
- ST56.BIN
- ST56T.BIN
- ST57T.BIN

These look like the stage and corresponding stage texture files. So maybe they did take advantage of the extra Megabyte of VRAM data? I guess my next blog post will be looking into specfically what the differences are with these two versions. 

