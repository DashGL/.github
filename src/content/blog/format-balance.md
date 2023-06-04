---
title: "Balancing the Model Layout"
description: "Exploring File Structure Options in the Dash Model Format"
pubDate: "March 11, 2019"
heroImage: "/img/4293843_orig-2303414837.png"
author: Kion
---

So one thing that I can’t get my brain around is around is how to balance the Dash Model format. Specifically with respect from the file body to the file header. I have a couple of options in front of me. By far the easiest option would be to implement the file type in blocks. And in that situation you would create an Image block, that image block would contain all of the information for images, and then you would move on to the texture block, which would do the same for the textures. In general one of the designs that I’m working around is that I want the top of the file to act as a summary of the tile. So if someone read the top of the file they should get an idea of how many vertices, faces, animations and images are in the file.

But then what happens is that you have to manage how much information to put in the header versus the body. You can create a super tiny header and include the rest of the information in the body, or you can put some of the information (like names) in the header and have the body be nothing but structs, or you can simply have nothing but blocks where all of the data is defined in a list one right after the other. Though right now I think it might be easier to define the file format as blocks, include all of the information that I think is required to implement each block, and then there I can think about to balance the information between the header and blocks.

Quick Summary of the Properties:

### Meta
- Author  
- License  
- Created on  
- Exported By

### Image
- Image Id  
- Image Name  
- Image Data

### Texture  

- Texture Id  
- Texture Name  
- Image Id  
- Wrap S/T  
- flipY

### Materials
- Material Id  
- Material Name  
- Texture Id (-1 for none)  
- Diffuse Color  
- Skinning?

Mostly Minified Version

```bash
DASH v1  OFS LEN
AUTH            LEN
[                ]
COPY            LEN
[                ]
DATE            LEN
[                ]
TOOL            LEN
[                ]
IMG  NUM  OFS  SIZE
[ id png ofs len ]
[ id png ofs len ]
[ id png ofs len ]
TEX  NUM  OFS  SIZE
MAT  NUM  OFS  SIZE
VERT NUM  OFS  SIZE
FACE NUM  OFS  SIZE
BONE NUM  OFS  SIZE
ANIM NUM  OFS  SIZE
[ id time ofs len ]
[ id time ofs len ]
[ id time ofs len ]
[ id time ofs len ]
```

Fully Minified Version

```bash
DASH VER  OFS  SIZE
AUTH NUM  OFS  SIZE
COPY NUM  OFS  SIZE
DATE NUM  OFS  SIZE
TOOL NUM  OFS  SIZE
IMG  NUM  OFS  SIZE
TEX  NUM  OFS  SIZE
MAT  NUM  OFS  SIZE
VERT NUM  OFS  SIZE
FACE NUM  OFS  SIZE
BONE NUM  OFS  SIZE
ANIM NUM  OFS  SIZE
```