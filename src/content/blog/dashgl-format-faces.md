---
title: "DashGL Format, Support and Faces"
description: "Rethinking Raspberry Pi Integration: Lessons from an Access Point Experiment"
pubDate: "October 1, 2018"
heroImage: "https://www.fxguide.com/wp-content/uploads/2012/07/utah_teapot.jpg"
author: Kion
---

In general I’m pretty happy with the texture and material approach I came up with in one of my earlier posts. By standardizing the structs with respect to textures and materials and keeping the options limited, we can simply our header file. Though this leaves us with a few issues. If it’s possible to simply most of our struct type entries, is there a way to standardize images, faces and animations so that these only have one entry? And then in terms of entries, is it possible force a fixed header, or will the header depend on the type of the content?

In general the goal is to create a binary file format that only supports a fixed number of attributes and is geared towards models that were around in the mid-1990’s. So basically meshes, rigged meshes, and rigged meshes with animation. In terms of faces, we’re looking to normals, vertex colors, and texture cords and whether to use them or not. Which means we could potentially use a face struct similar to the following:

```c
typedef struct Face {
    char materialIndex;
    char useVertexColors;
    char useVertexNormals;
    char useTexCoords;
    unsigned short a, b, c, nop;
    vec4 a, b, c;
    vec3 a, b, c;
    vec2 a, b, c
}
```

So that basically for each face we have a material index, whether the face uses vertex colors, vertex normals, and/texture coordinates. For materials I imagine it would be difficult to use more than 256 materials in a single file, but if that ever becomes an issue we can just increment the file type number. For the boolean values it would probably be simple to just use bit flags, but I think that would be cramming things down too much. Using structs means that we can include the definition of what each value is doing directly into the source code, where as with flags it would have to be describe somewhere else.

Following that we would have the indices for the a, b, and c values for the face. For the indices I think that we would define a vertex as a x,y,z position combined with a weight. So I might force the vertex list to be declared with the weights required, and then just set to all zero if not used. It seems kind of wasteful for files that might not use it, but in comparison I think the saving from using binary over text are going to be sufficient. And it also has the benefit that there are no major structural changes in the file for if a mesh has weights or not (i’m looking at you .dae). Which means we could get the file header down to something that looks like:

```
IMG
TEX
MAT
VERT
FACE
BONE
ANIM
```

And then that would make it easier to just simply require all of the things. For IMG and animation, I think we could declare the number in the header, and then just have the number of bytes before each image is declared and then just read the number of images from there. And then do something similar with animations. What would probably be more “computer-sciency” and cleaner would be to have a look up table at the top of the IMG and ANIM sections to be able to have a global offset to just to to read the information. So the section would basically have it’s own mini-header. That way someone could read the header, and then read the offset to a given image rather than having to loop through all of them. It would also make the file a little more forgiving to edit (not that I can see too many people editing by hand), but it has the added benefit of having more nerd points.

But packing all of the attributes in this way means that I could have a very simple header (type, offset, count) with a fixed number of entries. And then a simple list to determine what kind of mesh the file is. If there are bones, then it’s rigged, no bones, then it’s a mesh. Bones and Animation, then it’s rigged with animation, though the main detail is to look at the bones, to determine yes or no.