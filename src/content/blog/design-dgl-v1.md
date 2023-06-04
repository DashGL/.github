---
title: "Designing the Dash Model Format"
description: "More rants and ramblings about how to bring the format together"
pubDate: "March 11, 2019"
heroImage: "/img/Best-3D-Model-File-Format-for-your-AR-Applications-1-1200x635-1055366701.jpeg"
author: Kion
---


Since we have the FBX SDK working, then the next step is to start working on the next piece, which is designing the Dash Model Format. Specifically I’m aware that GLTF is a thing, but it’s an over complicated format, and not something that I really want to work with myself. And in a lot of cases I’m finding that I’m going in the opposite direction of everyone else that seems to view threejs as a deployment target at the end of production, where as I’m using threejs for reading models and trying to get them back into a usable format.

So I’ll start with the pieces that I’m pretty confident about and then try to fill in-between or mark out areas that are unclear. So the first part that’s easy is the magic number at the beginning of the file. To be able to check if a file is a Dash Model file or not, the first four bytes of the file should start with “DASH”, and then be followed by the version number. In general my plans for the Dash Model format is a very specific set of models to specifically be able to accurately describe models that were created in the 1990’s to 2000’s, so there could be a lot of modern techniques that I’m missing out on. I don’t want to make anything that tried to support everything and gets over-bloated, but I do want to leave myself some flexibility to allow the format to be expanded or adjusted without breaking compatibility if possible. So following DASH, my plan is to put the version number, which can either be a uint32 value of either “1, 2, 3, ect”, or maybe even a string value of “v1.0”, “v2.1”, ect. And I’m leaning toward the string version as it seems more descriptive.

The next step is to start working on the values that I want, or need to record. The general areas are pretty straightforward.

– Images  
– Textures  
– Materials
– Vertices (skin weights, and skin indexes)  
– Faces (indices, texture coords, face vertex colors, material index, face normals)
– Bones  
– Animations

So there’s really only one “easy” design decision in here, and that’s to have the face handle vertex colors and face normals as opposed to storing them with the vertices, and it’s easier to think of a unique vertex as a unique point with a unique weight. Though honestly, even that is debate-able. From there I have a few general questions to contend with:

### Questions

1. Do I separate between Meshes and Skinned meshes?  
2. For weights do I allow for setting the number of weights, or do I assume a maximum of 4?  
3. How do I set the format for faces?  
4. Do I support shaders?  
5. Do I support more than 1 uv channel?

### Thoughts

And right now my thoughts are:

**1** I don’t think I need to separate between skinned and non-skinned meshes and just assume everything is technically a skinned mesh, except when it’s not. So models that don’t have bones will just have zero bones, zero animations, and zero weight.

**2** For weights, one the idea is to save space on file size using binary, so I think I can afford to use weights. And second most of what this file format is intended for is working with characters. But at the same time it would be pretty stupid to export stages (which have a large number of vertices) with vertex weights. But at the same time, since these won’t have animations, the file size should balance out. So a skinned or not flag could be beneficial, but if I want to make the format as static as possible, then I would just include weights in all cases.

**3** For face format, this raises several questions. Do I allow for different face types to be defined (ie by defining a byte with bit flags), or do I define a create a flag that is set for a group of faces. And if I do that would I would have to allow for multiple face groups, or would I force one type for the entire model? Again this goes with vertices, but if I want to make the file format as static as possible, then inserting default values when these are not set would make things simple, as that’s generally want the editor or viewer is doing in the background anyway.

**4** For now, I think no. Even if I define shaders, it’s not something that is supported by a lot of editors. So I think that sticking with a standard format makes more sense.

**5** Again, no. I don’t have enough experience with multiple uv channels, and all of the models I’ve worked with, and that I’m targeting for this format only have one uv, channel, so I’m not going to try to support something I’m not familiar with. So for vertices, and faces I have the following format:

### Vertices

```
[ x (4) float ] [ y (4) float ] [ z (4) float ]  
[ indicex (2) ushort ] [ indicey (2) ushort ] [ indicez (2) ushort ] [ indicew (2) ushort ]  
[ weightx (4) float ] [ weighty (4) float ] [ weightz (4) float ] [ weightw (4) float ]
```

### Faces 

```
[ material index (2) ushort ]  
[ a (2) ushort ] [ b (2) ushort ] [ c (2) ushort ]  
[ aColor (4) uchar[4] ] [ bColor (4) uchar[4] ] [ cColor (4) uchar[4] ]  
[ aNormal (12) float[3] ] [ bNormal (12) float[3] ] [ cNormal (12) float[3] ]  
[ aTexture (8) float[2] ] [ bTexture (8) float[2] ] [ cTexture (8) float[2] ]
```

The next structure to start working on is bones. For bones, there is a lot of information that we have the option of either including or not including. A quick sketch of what I have in mind currently is the following:

```
[ Bone Name (0x20) char[0x20] ]  
[ Bone Id (2) ushort ] [ Parent Bone Id (2) ushort ]  
[ Position (12) float[3] ]  
[ Rotation (16) float[4] ]  
[ Scale (12) float[3] ]  
[ Matrix (0x40) float[16] ]  
[ Inverse Matrix (0x40) float[16] ]
```

Though I have several issues with this. For starters having the bone name floating about the struct looks kind of dumb, but I think in this format, we have images and animations that will have names as well. So I’m not sure if I want to have the names floating, or declare the id before the name. Declaring the id first is going in look better in general, even if it makes the string length 0x28 instead of 0x20, which shouldn’t really be an issue.

For rotation we have the option of using either quaternions or euler angles. I don’t think we have to worry about gimbol lock with bones, so normal euler angles are probably going to be better to work with.

From there we have the Matrix and inverse matrix. Looking at it, the matrix looks pretty stupid. Originally I would just want the matrix (and potentially inverse matrix), but I know some importers want the individual position rotation and scale, so I can probably take this out.

And for the inverse matrix, i’m not sure if this is the best to place to put it. The values provided could be wrong, so calculating them on import might be the easiest way, since I’ll probably have to figure out how to calculate them sometime eventually. But basically we can trust the transformations, and that someone could write these, but probably not the inverse matrix. So what we end up with is:

```
[ Bone Id (2) ushort ] [ Parent Bone Id (2) ushort ]  
[ Bone Name (0x20) char[0x20] ]  
[ Position (12) float[3] ]  
[ Rotation (12) float[3] ]  
[ Scale (12) float[3] ]
```

Which looks stupid, because it looks I’ll have NOPS everywhere to make the spacing align the struct to increments of 0x10 to make it easier to work with. This is why I originally just wanted matrix and inverse matrix, but generating matrices is easier than extracting them. Also I should go ahead and check to see which editors generate (and require) inverse matrices, because if editors have the values when exporting then might as well pop them in there.

Though I could reorganize the struct to

```
[ Bone Id (2) ushort ] [ Parent Bone Id (2) ushort ]  
[ Position (12) float[3] ]  
[ Rotation (12) float[3] ]  
[ Scale (12) float[3] ]  
[ Bone Name (0x20) char[0x28] ]
```

To align the struct to fit sizes of 0x10. Though I don’t think I did this with vertices or faces, so I can probably ignore this constraint and implement what’s easiest, but for bones, it seems like a good idea to make them easy to read (visually). Though I think a lot of that has to do with the string being declared inside the struct, if I declare the string in the header (like an archive), then I can just declare position, rotation, scale in the body of the file (though matrix and inverse matrix would be more elegant). Easiest option seems to be to throw bone names under the bus.

Which means right now my file header looks something like this:  

![](/img/dmf_header.png)

Which I’m actually liking declaring the names in the header, so doing the same with materials and textures might make it look more clean. And since the size of the header isn’t fixed, i should add offset and length in the first line of the file. Which makes the face list and vertex list the odd members here since everything else has a name. I’m tempted to use the space to declare the format for the vertices and faces. Also the extended letters for the labels look stupid, i should keep those to a magic number.