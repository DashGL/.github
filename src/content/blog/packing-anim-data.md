---
title: "Methods of Packing Animation Data"
description: "Rethinking Raspberry Pi Storage: Repurposing the Geekworm Case and Hard Drive"
pubDate: "October 3, 2018"
heroImage: "/img/1c5114ac73c9920022e915912c301279.jpeg"
author: Kion
---

To make things simple, I think I can use a fixed struct for the animation data. To put it simply:

```c
typedef struct DashKeyFrame {
    int frame;
    int bone_id;
    int parent_bone_id;
    char use_position;
    char use_rotation;
    char use_scale;
    char nop;
    vec3 position;
    vec3 rotation;
    vec3 scale;
}
```

I think the above struct will allow me to use a fixed struct for each keyframe. Since for every frame, and every bone we need to have a transformation matrix to tell the mesh how to deform. But with the use of interpolation it means there are a lot of values that get automatically get filled in. And we can’t make the complete assumptions about just filling in all of the keyframes, otherwise the data that gets saved into the file and read from the file becomes drastically different. Also in a lot of cases the use of nested arrays is used to manage this information, and even though it’s redundant, just including the frame number, bone id and bone parent id in each keyframe removes the need to use any kind of nested structure in the data.

As for the character boolean values, we have one value that tells the importer to use position or not, rotation or not and scale or not. The effect of this is that there can be alot of data with default zero values are included in the data, but in this case simplicity and a fixed structure take priority, so I won’t lose much sleep if a few extra bytes is included here or there. The one weird part about all of this is the frames per second value, which I figured I might as well include as the fourth byte, otherwise it’s just a no operation byte anyways. So while I’m okay with wasting bytes for the sake of having a fixed struct, we also might as well use bytes that aren’t really being used for anything. I think this makes a change to the animation entry.

```c
typedef struct DashAnimationEntry {
    char animation_name[0x20];
    unsigned int animation_id;
    unsigned int offset;
    unsigned int offset;
    unsigned int number_of_keyframes;
    unsigned int frames_per_second;
    unsigned int number_of_frames;
    unsigned int nop[2];
}
```

So I end up adding two nop values to the end of the animation entry, and then one nop into the keyframe values. But I guess that’s acceptable.