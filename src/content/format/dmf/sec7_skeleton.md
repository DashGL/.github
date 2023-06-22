---
title: 7.0 Skeleton
pubDate: June 22, 2023
author: Kion
slug: dmf/skeleton
---

# 7.0 Skeleton

## 7.1 Binary

The primary intended use for the Dash Model Format is rigged models. So the skeleton for the model is assumed to be rigged to the mesh defined by the vertices and faces in the prior sections. The skeleton is made up of a list of bone structs with the following defintion.

```c
typedef struct {
	char name[0x20];
	int16_t bone_id;
	int16_t parent_id;
	uint8_t nop[0x0c];
	float matrix[0x10];
} DashBone_t;
```

The first property in the struct is the bone name. Ideally this should be something like, "arm" or "leg" when possible, otherwise a name such as "bone_001", should be used. Specifically these names should not be empty and these names should be unique for each bone. As often the bone name will be used when creating animation curves for a given bone. 
The following property is the bone id, and similar to other id's this value is dictated by the bone's position in the array. So this number should be sequential starting with 0 for the root bone, and incrementing from there. 

The parent_id is the index for the parent bone. This value should be -1 for the root bone and be an index in the array after the root bone. The order of the bones should be structured so that parent bones are always defined before child bones. This seems obvious, but it is technically possible to create an out of order bone list that depends on indices. So when writing to a .dmf file it is always important to make sure this condition is set, so that bones can be parsed on a single loop for the respective import scripts.

The is 0x0c of padding for alignment to 0x10 boundaries, followed by the local transformation matrix. Admittedly this definition is a toss up between the local transformation matrix, and the individual transformation values for position, rotations and scale as vector3 values for each. In terms of approach for 3D applications, the local transformation matrix seems to be the most common, and if really required the original transformation values can be decomposed from the local transform matrix. It was also considdered to have both options included in the same bone, but that could present and issue where one value or the other wasn't calculated correctly and could result in compatibility issues between different importers and exporters. So it was chosen that only the transformation matrix be used. Also note that the alignment of the matrix is column-centric fit with the definition of most 3d applications based off OpenGL.

## 7.2 JSON

```json
{
  name : string,
  id : int,
  parent_id : int,
  matrix : float[16]
}
```

In the JSON representation, the struct is nearly identical to the binary struct, without the need for binary padding.



