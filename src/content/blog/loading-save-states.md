---
title: "Load Save State issue"
description: "Troubleshooting Save State Issues in PCSX-Reloaded"
pubDate: "May 6, 2020"
heroImage: "/img/hacking-save-state-fig1-3722335346.png"
author: Kion
---

After trying to set the memory I ran into several problems. At 1st I thought it was because I tried to change too many things to fast. And then after walking back several changes The reason seemed to be because Save States were not loading at all. The main reason I can think of for this is because the version I ForkedHad an issue with it.

I think there are several ways to do with This. One would be to look for a different version to fork. And another will be the right tools for looking at save states directly. As far as looking at save States directly I mean is you is looking into the Decompression. No in general would make more sense to use a version that actually works. So I think I’m going to look into the apt-get version and find out where that repository is.

[https://github.com/pcsxr/PCSX-Reloaded](https://github.com/pcsxr/PCSX-Reloaded)

After a few tests I found that trying to move around the order so that the system memory comes at the start of the save state has the side effect that save states don’t seem to be listed when loading. So there could be some check that happens ahead of time to throw these away. Not sure if I want to track down this process or not.

And then I also find that the 10th save state tends to be pretty annoying. The 10th state seems to a backup state that is automatically created when the emulation had stopped to be able to restore the state again. So I might try and check the last three characters of the save state, and if it’s ‘010’, then skip and don’t write a state.