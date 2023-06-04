---
title: "Dash Model Format Version 2"
description: "Making notes on the layout for the next version of the Dash Model Format"
pubDate: "October 2, 2018"
heroImage: "/img/th-4015604522.jpeg"
author: Kion
---


I think I have the header defined for the next version of the dash model format

| Offset | 0x00 | 0x04 | 0x08 | 0x0c |
|--------|------|------|------|------|
|0x0000 |DASH| 2 |offset |length|
|0x0010 |IMG| num |offset |length|
|0x0020 |TEX| num |offset |length|
|0x0030 |MAT| num |offset |length|
|0x0040 |VERT| num |offset |length|
|0x0050 |FACE| num |offset |length|
|0x0060 |BONE| num |offset |length|
|0x0070 |ANIM| num |offset |length|

### IMG

Images and animations will be in archive. For image, i think the archive struct will be:

```c
typedef struct DashImage {
	char image_name[0x20];
	unsigned int image_id;
	unsigned int offset;
	unsigned int length;
	char image_extension[0x04];
}
```

### TEX

For textures, since we don’t need much I think we can use the following:

```c
typedef struct DashTexture {
	char texture_name[0x20];
	unsigned int texture_id;
	unsigned int image;
	unsigned int wrapS;
	unsigned int wrapT;
	unsigned int flipY;
	float rotation;
	vec2 center;
}
```

Generally which image to use, how to wrap S and T, whether to flip Y or not, the rotation (in radians) and the center. Probably won’t use all of these, but it should allow for these values to be utilized if needed, even if I never use them.

### MAT

For materials, the main issue I think is how to manage blending.

```c
typedef DashMaterial {
	char material_name[0x20];
	unsigned int material_id;
	int texture;
	float alpha_test;
	float opacity;
	char vertex_colors;
	char blending;
	char blend_src;
	char blend_dst;
	char side;
	char transparent;
	char visible;
	char skinning;
	DashColor diffuse;
	DashColor ambient;
	DashColor specular;
}
```

For materials, generally the aim is to go for something between lambert and phong. We only need one texture coordinate, since bump, environment and lighting maps are beyond the scope of what the goals of this file format are. Values that are not defined can be ignored, like blending should be 0 is no blending is defined, ambient and specular can have 0 for the fourth bit to indicate not to use them. And as for vertex colors, if the face defines them, then i’ll set the value for the material if needed in the importer, but I don’t think the value is needed in the struct, though I might add it just the be safe.

### VERT

Vertices should be pretty easy.

```c
typedef DashVertex {
	vec3 position;
	unsigned short indices[4];
	vec4 weight;
}
```

A vertex is defined as a position combined with a weight and index. If there is no weight (or index), then just fill in zero.

### FACE

The face we already defined in a previous post. I don’t really see any need to change anything. The only part that annoys me is the use of nop to fill in space between four byte boundaries. Though I could just make the indices unsigned ints, but that would be kind of stupid when passing to the graphics buffer. So I might take another look and move some crap around.

```c
typedef DashFace {
	char materialIndex;
	char useVertexColors;
	char useVertexNormals;
	char useTexCoords;
	unsigned short indices[3];
	unsigned short nop;
	vec4 vertex_colors[3];
	vec3 vertex_normals[3];
	vec2 tex_coords[3];
}
```

### BONE

For the bones I kind of want to throw everything in (including the kitchen sink). Calculating values for bones can be annoying, so if the program provides these values it would probably be best to use them, or at the very least be required to know how to calculate them, so they can be verified.

```c
typedef DashBone {
	char bone_name[0x20];
	int parent_bone_id;
	int bone_id;
	vec3 position;
	vec3 rotation;
	vec3 scale;
	mat4 matrix;
	mat4 inverse_bind_matrix;
}
```

So pretty much include everything. Local position, local rotation, local scale, local matrix, and inverse bind matrix. If an importer works better with one more than the others it can just be used, otherwise, these values can be calculated as needed.

### ANIM

```c
typedef DashAnimationEntry {
	char animation_name[0x20];
	unsigned int animation_id;
	unsigned int offset;
	unsigned int length;
	unsigned int frames_per_second;
}
```

Animation entry is just a carbon copy of image entry, since it’s basically an internal archive. For the actual animation data, there are three options, fcurves, so a value for a given axis for a given time, or a collection of key frames for specific frames, or lastly a key frame for every bone, for every frame in the animation. I’m really tempted to go with the last one because it makes reading and writing stupidly easy, and these values can be optimized by the importer if needed.