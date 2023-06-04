---
title: "Planning out the next Dash Model Format"
description: "Trying to consolidate SoC devices using the Geekworm x830 case and mass storage"
pubDate: "September 29, 2018"
heroImage: "https://cdn2.sculpteo.com/blog/wp-content/uploads/2018/10/Blog-headline-2-11.jpg"
author: Kion
---

In the last post I examined gltf and I think it has too many issues to get involved with. That being said, I definitely won’t complain if there is some ground swell of support and devs can magically make the file format viable, but until then I’m not holding my breath. I think it would be a better use of my time to address the issues with the Dash Model Format and amend them so that I can start working on a Noesis plugin or something.

One thing that became clear after looking back over the gltf definition is that it does make sense to split the image and the texture, which is something I was having trouble with. As the attributes for textures can be a set struct, but the image is a variable length file. Granted I can have the image merged into the texture, but it makes the file format really ugly. So what I’m thinking of now, is having the images listed individually, with the count being the number of bytes. And then textures and materials can be structs with the count representing the number of structs in the array.

|Type | Offset | Count |
|-----|--------|-------| 
| IMG | 0x100 | 0x123 |
| IMG | 0x130 | 0x395 |
| TEX | 0x400 | 0x2 |
| MAT | 0x420 | 0x2 |

Which I think makes the image, texture and material management really clean looking. The only other consideration is the option to add names into the file. Something I’m tempted to leave out completely. But if I do put it in, I can allocate 0x20 bytes to a name string and the beginning of the texture and material structs respectively to define a name. Since Images are already using the byte length for the count, I don’t think it’s a good idea to add in names, so if it’s needed it can be included in the texture if needed.