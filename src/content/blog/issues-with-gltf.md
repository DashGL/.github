---
title: "My Issues with GLTF"
description: "Some random rants about getting familiar with GLTF"
pubDate: "September 28, 2018"
heroImage: "http://www.geeks3d.com/public/jegx/2018q2/geexlab-madview3d-gltf-3d-model-01.jpg"
author: Kion
---
 

I have more than my fair share of issues with the .dae or Collada file type. Generally the fact that it’s an XML file format makes it a non-starter and generally very difficult to work with. The use of tags and id’s makes ti very messy to export to as a file format. And even if you try to use json as a base format to work with, you’ll never really know what you’ll end up with until you actually try exporting the file, which generally means you’re going to spend most of your time messing with strings. But in addition to that, the example models don’t provide enough examples, and there are functionalities, such as the ability to include textures as base64 images, that are just not supported.

In general it looks like .dae came out, and a lot of people at the time put a lot of effort in to make it work, but no one has supported the file format beyond that because it sucks.

So when .gltf was announced as a json format, I was cautiously optimistic that there could be an accessible standard format for 3d models. After looking at the file spec, my optimism quickly faded. Not that I’ll complain if the file format gains wide spread adoption and is supported by a wide number of applications to the point where I don’t have to worry about it. But there are several aspects of the file definition that I find baffling and may serve as a road-block to anyone supporting it.

### Scenes
```json
    "scene": 0,
    "scenes": [
        {
            "nodes": [
                0
            ]
        }
],
```

Not a huge deal. In general scenes are hard to get a grasp on when you’re starting to build tools by exporting cubes. I’m not sure I’ll need to use scenes anytime soon, but I can understand why they are there. What confuses me about the scenes though is the way they are implemented. If you go the other way, you can make a scene with a list of models and lights. Though I guess that’s what’s going on here, you define a list of objects or “nodes” and then just define by index, which objects or “nodes” are in any given scene.

### Everything is a Node
```json
    "nodes": [
        {
            "children": [
                4,
                1
            ],
            "matrix": [
                1.0,
                0.0,
                0.0,
                0.0,
                0.0,
                0.0,
                -1.0,
                0.0,
                0.0,
                1.0,
                0.0,
                0.0,
                0.0,
                0.0,
                0.0,
                1.0
            ]
        },
        {
            "mesh": 0,
            "skin": 0
        },
        {
            "children": [
                3
            ],
            "translation": [
                0.0,
                -3.156060017772689e-7,
                -4.1803297996521
            ],
            "rotation": [
                -0.7047404050827026,
                -0.0,
                -0.0,
                -0.7094652056694031
            ],
            "scale": [
                1.0,
                0.9999998807907105,
                0.9999998807907105
            ]
},
```

This is one that I find confusing but everything is a node. Models are nodes, bones are nodes, lights are lights. I can kind of understand as in the context of a 3d library, the actual content of any given 3d object doesn’t really matter. From the library everything is just a transformation matrix that gets pushed onto the matrix stack. In the case of a list of bones, I can probably make the root bone the first “node” followed by the list of bones following that. And then have the mesh and skin declared at the end. The mesh that gets declared at the end is probably going to be the index that gets declared in the scene array.

Though this works for one model as you get a close to being internal with the bone numbering. I would be scared to have more than one skinned mesh in a single gltf file, and if that would require the exporter to offset the bone list of a given character by the index in the array. So if I have a character, a weapon and a pet. I could see how you could add the weapon as a node in the characters hand. But a pet that has it’s own skeleton and animations, it would make a lot more sense to just declare a new gltf file. Which is also kind of annoying as there’s no difference between individual assets or assets included as a scene. A gltf file will generally have to be seen as the root of a given scene by an importer. Which is why it would kind of be nice is the specification had the option to leave out the scene definition entirely to specify a single asset.

### Meshes are Stupid

```json
    "meshes": [
        {
            "primitives": [
                {
                    "attributes": {
                        "JOINTS_0": 1,
                        "NORMAL": 2,
                        "POSITION": 3,
                        "WEIGHTS_0": 4
                    },
                    "indices": 0,
                    "mode": 4,
                    "material": 0
                }
            ],
            "name": "Cylinder"
        }
],
```

I can understand the way meshes are declared. They basically have meshes divided up into draw calls. So if I have a character with a body and a face with two materials, then I’ll have to declare two primitives. Not bad right? Well it doesn’t really look that simple. The way I try to declare models is with one list of unique vertices, and a list of materials. And then primitives can just be a material number with a list of face indices. In this case the position, normals, joints and weights are all included in the primitive. Does this mean I need to include the same global list of vertices twice? Or does it mean I need to break up the part of the mesh so that each part is effectively it’s only model? So that in this example the body is vertices 0-900 and then the head is 0-100, rather than just being one long list of vertices for the geometry, and then split into two draw calls for the faces. I’m not really sure, it’s confusing.

### Skins, the Confusion Continues

```json
    "skins": [
        {
            "inverseBindMatrices": 13,
            "skeleton": 2,
            "joints": [
                2,
                3
            ],
            "name": "Armature"
        }
],
```

Aside from “joints” I think I’m familiar with what’s going on here. The inverseBindMatrices is a list of matrices with the inverse transpose of the parent bone so that rotations from the animation can be calculated relative to the child bone for the t-pose. The skeleton is the list of bones. But I’m never really sure what “joints” mean, or where these indices are referring too. This was a similar problem to .dae where you had to manage a massive number of cross referenced id’s and names that should have been implicitly understood within the context of the file anyways. GLTF mostly solves this problem with indices, but the problem is they have it broken down into so many components that it becomes a problem of managing what each index is actually pointing to.

### Accessors and Buffer Views are Stupid
```json
    "accessors": [
        {
            "bufferView": 0,
            "byteOffset": 0,
            "componentType": 5123,
            "count": 564,
            "max": [
                95
            ],
            "min": [
                0
            ],
            "type": "SCALAR"
        },
        {
            "bufferView": 1,
            "byteOffset": 0,
            "componentType": 5123,
            "count": 96,
            "max": [
                1,
                1,
                0,
                0
            ],
            "min": [
                0,
                0,
                0,
                0
            ],
            "type": "VEC4"
},
``` 

In general the way gltf manages buffers is actually pretty good. I wish there were only two ways to manage buffers, 1. either as base64 string, or 2. as a binary .glb file. I don’t completely miss the point of being able add buffers as separate binary file, but that just increases the number of files to be managed. Which kind of represents the problem with GLTF in general is that it tries to please everyone. When really if you needed to manage a scene you could use a .blend file. If you need a lot of rigging and complex camera movement you could use an .mmd file. There’s really no need to reinvent the wheel for either of these. The weakest link when it comes to 3d files is that you either have files like .OBJ or .PLY that can’t use bones, or you have to go up to a .DAE file which requires you define a freaking scene. What’s needed is a simple _asset_ format. The Khonos group even specifies this in their introduction to GLTF:

The GL Transmission Format (glTF) is an API-neutral runtime asset delivery format. glTF bridges the gap between 3D content creation tools and modern graphics applications by providing an efficient, extensible, interoperable format for the transmission and loading of 3D content.

_Asset Delivery Format_, I couldn’t tell with all these freaking nodes. But back to the main problem with accessors and buffer views, is they basically do the same thing. You have a buffer that contains all of the data required to define all of the vertices and faces in the file. And then from the mesh you point to an index for an accessor that gives the range, format, min and max, and which buffer view to use and then the buffer view says which buffer, from where to where and how long the buffer is. You’re basically adding two _needless_ layers for something that can just be done from the mesh. “Here’s the position, it’s in this buffer, start at this offset, use this stride, this component type and this min and max”. At best I could understand just having buffer views with this information unique to each buffer view, but accessors and then buffer views? No, it’s stupid and redundant.

### Materials are Stupid
```json
    "materials": [
        {
            "pbrMetallicRoughness": {
                "baseColorTexture": {
                    "index": 0
                },
                "metallicFactor": 0.0
            },
            "emissiveFactor": [
                0.0,
                0.0,
                0.0
            ],
            "name": "blinn3-fx"
        }
    ],
    "textures": [
        {
            "sampler": 0,
            "source": 0
        }
    ],
    "images": [
        {
            "uri": "DuckCM.png"
        }
],
```

Not sure why all materials need to have "pbrMetallicRoughness" definition, but you do and it's stupid. Also I'm not sure why "baseColorTexture" needs to be declared inside the metallic definition. Materials would make a lot more sense if this were not needed, or if you could just declare phong shading, or your own custom shaders somewhere. I'm not sure why everything needs to have a metal definition, or that's the trend the industry is taking and everything needs to be shiny and reflective. Could be and I could just be an idiot, but I would like to be able to declare some simple models without all of this non-sense. Images and textures seems redundant, but I guess not that bad. You can declare images as a list of images, and then you can declare textures with different sample, clamp or mirror settings.

### Asset Definition
```json
{
	"type" : "mesh",
	"position" : {
		"buffer" : 0,
		"offset" : 0x1200,
		"stride" : 0x12,
		"count" : 100
	},
	"normal" : {
		"buffer" : 0,
		"offset" : 0x1200,
		"stride" : 0x12,
		"count" : 100
	},
	"bones" : {
		"buffer" : 0,
		"offset" : 0x100,
		"count" : 5
	},
	"weights" : {
		"buffer" : 0,
		"offset" : 0x2000,
		"count" : 100
	},
	"calls" : [
		{
			"material" : 0,
			"indices" : {
				"buffer" : 0,
				"offset" : 0x3000,
				"count" : 30
			}
		},
		{
			"material" : 1,
			"indices" : {
				"buffer" : 0,
				"offset" : 0x3400,
				"count" : 23
			}
		}
	],
	"animations" : [
		{
			"buffer" : 0,
			"offset" : 0x4000,
			"name" : "run"
		}
	]
}
```

So what's the practical application of all my ranting? Have all of the definitions inside the "mesh" node. If you define the type as a mesh, then you can make a lot of definitions implicit. Do I need to tell the accessor that I'm looking for a set of three floats for each position. No because it's the freaking position, that should be implicit. In what situation are you going to have any other data between a single set of x, y, z coords? The answer should be never. If you make definitions for bones and animation, then you can throw those in the buffer as well, since that not something that you really need to edit. Like if you have a mesh as a child of another mesh, then I could understand why you might want to adjust the position by hand. But for complication meshes with hundreds of bones, is that something that should be taking up space as text in the file? Definitely not.

For calls you can either do things one of two ways. You can either have a buffer where you have everything ready to be copied to the graphics memory for performance, or the style above that keeps positions separate to be loaded into a texture editor. And you can assume that the indices are going to be unsigned shorts in pairs of 3 for triangle faces and again, don't need to define the type, because it should be implicit. So rather than having a ton of different data types and indexes cross referencing and redefining the same redundant information, you can keep everything in one short view to be easily read and exported by different applications, rather than defining a file type that no one can support.