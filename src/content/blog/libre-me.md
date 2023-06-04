---
title: "Libre me from Evil"
description: "Managed to get my Nexus 5x working with Lineage"
pubDate: "November 28, 2018"
heroImage: "/img/Official-LineageOS-14.jpeg"
author: Kion
---

Managed to get my Nexus 5x working with Lineage. The problem was me being stupid and not reading directions. I needed to flash Twrp to the recovery, boot to the recovery, install Lineage and then select boot to system. The next issue is going to be getting the APN settings to work, which always seems to be hit and miss. So we’ll see how stupid I am to get or not get that.

So I’ll use this post to save some of my Bless Preferences:

```xml
<preferences>
	<pref name="Session.RememberCursorPosition">True</pref>
	<pref name="Tools.ConversionTable.LEDecoding">False</pref>
	<pref name="Default.EditMode">Overwrite</pref>
	<pref name="Tools.ConversionTable.Show">False</pref>
	<pref name="Tools.Statistics.Show">False</pref>
	<pref name="Session.RememberWindowGeometry">True</pref>
	<pref name="Highlight.PatternMatch">True</pref>	
	<pref name="Undo.KeepAfterSave">Memory</pref>
	<pref name="View.Toolbar.Show">True</pref>
	<pref name="Default.NumberBase">Hexadecimal</pref>
	<pref name="View.Statusbar.Offset">True</pref>
	<pref name="Default.Layout.File"></pref>
	<pref name="View.Statusbar.Show">True</pref>
	<pref name="Undo.Limited">False</pref>
	<pref name="Default.Layout.UseCurrent">False</pref>
	<pref name="Session.LoadPrevious">True</pref>
	<pref name="View.Statusbar.Overwrite">True</pref>
	<pref name="Session.AskBeforeLoading">False</pref>
	<pref name="Undo.Actions">100</pref>
	<pref name="View.Statusbar.Selection">True</pref>
	<pref name="ByteBuffer.TempDir"></pref>
</preferences>
```