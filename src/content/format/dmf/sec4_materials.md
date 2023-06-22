---
title: 4.0 Materials
pubDate: June 22, 2023
author: Kion
slug: dmf/materials
---

# 4.0 Materials

## 4.1 Binary

The Dash Model format approach to materials is to define everything. This is in contrast to a lot of other binary formats that I've dealt with that will often use bitflags at the start of the struct in order to indicate which properties either are, or aren't defined in the struct, and have a variable struct length. So to get around this, the Dash Model Format used a fixed struct where all of the properties for a material are defined, and default values are used, or properties that aren't needed can be ignored.

```c
typedef struct {
	char name[0x20];
	int16_t mat_id;
	int16_t tex_id;
	uint16_t use_blending;
	uint16_t blend_equation;
	uint16_t blend_src;
	uint16_t blend_dst;
	uint16_t skinning;
	uint16_t shader_type;
	uint16_t face_side;
	uint16_t shadow_side;
	uint16_t ignore_lights;
	uint16_t vertex_color;
	uint16_t use_alpha;
	uint16_t skip_render;
	float alpha_test;
	float diffuse[4];
	float emissive[4];
	float specular[4];
} DashMaterial_t;
```

The first 32 bytes of the struct is provided for the material name. I'm not sure how often materials are actually named in practice, but a name like "material_000", or "material_001", is suggested if none is already defined as it futher helps with tracing through the file. Following the name is the id, this is a 2 byte short value that is expected to be the index of the material. Note that similar to textures, the array position in the file takes priority, so in a situation where the id is out of order, the material_id property in the face struct will use the index and not the id. Following the material id is the texture id. This will be the diffuse mapping for the material. If there is a texture it should use an id from 0-n depending on the number, and if no texture is mapped to the material then the index should be set as -1 to indicate.

The next 8 bytes are 4 short values for blending. The first propery use_blending indicates if alpha-blending should be used or not. So 0 for "don't use blending", and 1 for "use blending". In the case that 1 is set, then the following three properties for the equation, src alpha, and dest alpha will be evaluated, otherwise these values will be ignored. So if blending is not desired all of these values can be defined as 0. The blend constants are defined as follows.

```c
// Blend Equations

#define DASH_MATERIAL_NORMAL        0
#define DASH_MATERIAL_ADDITIVE      1
#define DASH_MATERIAL_SUBTRACT      2
#define DASH_MATERIAL_MULTIPLY      3

// Blend Contansts

#define DASH_MATERIAL_ZERO                  0
#define DASH_MATERIAL_ONE                   1
#define DASH_MATERIAL_SRC_COLOR             2
#define DASH_MATERIAL_ONE_MINUS_SRC_COLOR   3
#define DASH_MATERIAL_SRC_ALPHA             4
#define DASH_MATERIAL_ONE_MINUS_SRC_ALPHA   5
#define DASH_MATERIAL_DST_ALPHA             6
#define DASH_MATERIAL_ONE_MINUS_DST_ALPHA   7
#define DASH_MATERIAL_DST_COLOR             8
#define DASH_MATERIAL_ONE_MINUS_DST_COLOR   9
#define DASH_MATERIAL_SRC_ALPHA_SATURATE    10
```

The next value is skinning. In general most meshes in the Dash Model Format are assumed to be be rigged meshes. But some tools have a flag for if a specific model should acted as a skinned model or not, so this attribute has been included. In general for the Dash Fomat, if bones are declared, the assets is assumed to be skinned mesh, otherwise if the bone list has a length of 0, it's assumed to be a non-rigged mesh. But a non-rigged mesh is ecentially a rigged mesh to a single bone at the origin. For the shader type, the possible shader types are "basic", "lambert" and "phong". For "basic", only the diffuse color is used, for "lambert" emissive and diffuse color are evaluated. And for the last possible value "phong", diffuse, emmissive and specular are all evaluated. And if you're wondering why the options are limited and why there isn't metalic, the idea of the format is to define a simple format that can be viably supported by a wide range of applications. So more options could be added depending on necessity or support availability for a wide range of applications. 

```c
// Shader Types

#define DASH_MATERIAL_BASIC        0
#define DASH_MATERIAL_LAMBERT      1
#define DASH_MATERIAL_PHONG        2
```

The next two properties are face side and shadow side. For face side I think the perferable option is to define the faces to be on the desired side in the first place depending on the winding order. I think this is mostly a flag for double-sided, but as long as it's there, we might as well add in the option for backside. And these same values can be used for the shadow side as well. The property for ignore lights is a boolean value, so 1 for "please ignore lights", otherwise 0 for "please calculate" light source. 

```c
// Render Side

#define DASH_MATERIAL_FRONTSIDE     0
#define DASH_MATERIAL_BACKSIDE      1
#define DASH_MATERIAL_DOUBLESIDE    2
```

The next propery is vertex color, which indicates the material uses vertex color. The Dash Model Format approach is that everything should have vertex colors, as vertex colors are always defined in every face. If vertex colors are not needed, then vertex colors should be defined as white. This option has been included as it's an option found in some 3d applications, but in general this value should always should be assumed to be 1 for the Dash Model format approach, but you have the option to turn it off. For use alpha, this is a pretty common option, set 1 to indicate the material uses the alpha channel and 0 to indicate alpha should not be used. And the last proprty before the colors is the skip_render boolean. I'm not actually sure when or where this attribute is implemented. It seems to mostly be used in a case where a character holds multiple weapons and the weapon is to be skipped drawing or not. But really that should be done as a separate mesh, but this has been included since it seems like a pretty common attribute in several applications, as well as being implemented in several formats. For the alpha test property, this is a float from 0.0f to 1.0f if alpha test is desired. If it's not needed, it should be set at 0.0f.

Lastly we come to the different color channels. Diffuse is defined as four floats for red, green, blue and alpha. For emmissive, four floats are defined for red, green blue and intensity. How intense is the intensity? I really have no idea, it seems to depend on the application. And last we have specular which has four floats for red, green, blue and the specular coefficient or how [shiny](https://www.youtube.com/watch?v=93lrosBEW-Q) something is. Though this value seems to depend on the application, so we'll probably have to write some kind of scalar depending on the application. I think 30.0f should be considdered "standard" or something like that.

4.2 JSON

```json
{
  "id" : int,
  "name" : string,
  "texture" : int,
  "use_blending" : bool,
  "blend_equation" : enum("norm", "add", "sub", "mult"),
  "blend_src" : enum("zero", "one", "src", "one_minus_src", "src_alpha", "one_minus_src_alpha", "dst_alpha", "one_minus_dst_alpha", "dst", "one_minus_dst"),
  "blend_dst" : enum("zero", "one", "src", "one_minus_src", "src_alpha", "one_minus_src_alpha", "dst_alpha", "one_minus_dst_alpha", "dst", "one_minus_dst"),
  "skinning" : bool,
  "shader_type" : enum("basic", "lambert", "phong")
  "face_side" : enum("front", "back", "double"),
  "shadow_side" : enum("front", "back", "double"),
  "ignore_lights" : bool,
  "vertex_color" : bool,
  "use_alpha" : bool,
  "skip_render" : bool,
  "alpha_test" : float,
  "diffuse" : float[4],
  "emissive" : float[4],
  "specular" : float[4]
}
```

The JSON format closely follows the properties defined by the binary format, with the main expection that the enum values can and probably should be defined as a specific string value from the given possibility of string values.

