---
title: 8.0 Animations
pubDate: June 22, 2023
author: Kion
slug: dmf/animation
---

# 8.0 Animations

## 8.1 Binary

Animations are defined by two structs. One struct which defines the animation, it's name and how long it is. And another struct that defines a specific key frame for a specific bone, which this document will refer to as a "track". The reason for this is to define the animation with only fixed length structs. So the first struct will provide definitions for all of the animations with a fixed length of structs, and then each animation is defined by an array of fixed length tracks. It makes the process very simple to break down and parse. The struct for the animation definion is given as follows.

```c
typedef struct {
	char name[0x20];
	int16_t anim_id;
	uint16_t fps;
	uint32_t track_offset;
	uint32_t track_count;
	float duration;
} DashAnimation_t;
```

The first 32 bytes of the struct are for the animation name. Ideally this should be "run" or "walk", but similar to the bones, this value should be defined and should be unique. The anim_id property is pretty much entirely for show as there is nothing dependend on this index. It is included to make the file format as friendly as possible for reverse engineering to make the function of something easy for someone to understand when looking at the hexidecimal represention of the file format.

The next property is the "frames per second" or "fps", in most situations this value will likely be 30 or 60. The track_offset is the offset in the file to the list of animation tracks that define animation, and the track_count property is the length of the track list. The last property in the struct is a float value for the duration of the animation in seconds.

```c
typedef struct {
	int16_t bone_id;
	int16_t prs_flags;
	float time;
	float pos[3];
	float rot[4];
	float scl[3];
} DashAnimTrack_t;
```

The animation track is a small innovation for the Dash Model Format. With most of the binary file formats I have experience with, the animation formats will often have a pointer to a list of bones, for which each bone value, there is a pointer if there are position values, rotation values, or scale values and pointers to where those lists can be found, and how many values there are. 

For the Dash Model Format, we use a direct list of fixed length structs to accomplish this. And the approach that we use is the first property in the struct is the bone_id to define the key frame for. The next value is a bitflag value for the "position", "rotation", and "scale" flags. In all cases places in the struct are reserved for each of these transformation, but whether or not they are evaluted depends on this bitflag. If the first bit is set, then position will be evaluated, if the second bit is set rotation will be evaluated, and if the third bit is set then scale will be evaluated. Note that the bit flag must have a bit set, so possible values are a single transformation, any combination of two transformation types or all transformation types. 

Following the bitflag is a float for the time value for the time of the keyframe in the animation. Following the time are the three reserved floats for the position, which are in x, y, z order. The rotation values are stored as quaternions in x, y, z, w order. And scale values are stored in x, y, z order.

## 8.2 JSON

```json
{
  id : int,
  name : string, 
  fps : int,
  duration : float, 
  tracks : [
    {
      bone_id : int,
      time : float,
      pos : float[3], (if exists)
      rot : float[4], (if exists)
      scl : float[3] (if exists)
    },
    ...
  ]
}
```

The JSON representation of this struct largely reflects the contents of the binary struct. Like with other structs, rather than providing and offset and length for the tracks, the tracks are included as an array of objects directly. Otherwise the id, name, fps, and duration are as defined in the bianry section. For the animation track, the same rules hold true for the bone id and time. The difference for the transformations is that the transformations are only defined if they exist. And similar to the binary format, at least one transformation type should be defined, but the combination of any two, or all three can also be defined.

