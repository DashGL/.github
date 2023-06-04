---
title: "Planning out Dash Model Format Version 2"
description: "More planning when I should probably be implementing something"
pubDate: "October 4, 2018"
heroImage: "/img/dash_v2_outline.png"
author: Kion
---

So back of the napkin style, file layout planning. At the top of the file will be the magic number, version number, offset to the table of contents, and length of file. Though for the length, it’s the length of the file from the start of the table of contents, until the end of the file, not including the magic number and copyright, meta information. The meta information is located in-between the offset declared after the magic number. In general, this is not needed, so it could be located right after the first line in the file, but at the very least the file definition and export tool should be listed in this area. And in the case of copyright, the creator should declare what rights they intend for their creation in the file, to avoid confusion.

After that comes the table of contents with the number, offset and length. So the first offset from the images should come right after the table of contents (if exists). Otherwise the offset from the first material will come right after the table of contents.

Which means I need to start planning out the exporter for a Threejs plugin. So The first step will be to create a constructor for the exporter. The next step will be to create a parse function that accepts parameters for the author and copyright. The next step will be to break down the images, textures and materials into structs and then write the images, textures and structs. Since I have the base from the first version of the Dash Model Exporter, so I can take notes from my original exporter. And the last thing is in the case of Threejs, I should probably use Buffer Geometry and not normal geometry, because it’s easy to convert normal geometry to buffer geometry, and then break down the attributes into the specific lists.