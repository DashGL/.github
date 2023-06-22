---
title: 6.0 Faces
pubDate: June 22, 2023
author: Kion
slug: dmf/faces
---

# 6.0 Faces

## 6.1 Binary

The Dash Model format is designed as an interchange format. So one of the design goals was to define vertices as single point with a specific weight. This was a model could be converted or imported into an editor, and allow the points to be manipulated. This is in contrast to a delivery format where all of the vertices are in a buffer that is already in a format that can be drawn directly into the GPU. In this format the positions and other other attributes are copied as many times as required in the order to draw the mesh. So in order to maintain the format as an interchange format, the following balance was decided on that a given vertex is a unique position with a unique weight, and then normals, uv's and vertex colors would all be defined per face indice. This allows for a very simple but powerful approach to model definitions that provides a lot of flexibility while using fixed-length structs. The struct for the face is given below.

```c
typedef struct {
	uint16_t mat_id;
	uint16_t nop;
	uint32_t a;
	uint32_t b;
	uint32_t c;
	uint8_t aClr[4];
	uint8_t bClr[4];
	uint8_t cClr[4];
	float aNrm[3];
	float bNrm[3];
	float cNrm[3];
	float aUv[2];
	float bUv[2];
	float cUv[2];
} DashFace_t;
```

In the struct the first property is the material id. Textures are defined, materials are linked to textures, and faces reference a material. The following two bytes are not used in the interest of keeping to 4 byte boundaries. The next three properties are the a, b, c. These are indices that refer to indexes in the vertex list. They are 32 byte byte values as it was determined that 16 byte values didn't provide enough indices. So while the other id's for textures, materials and bones are all signed 16 byte values (to allow for -1), in this case there is no case where a vertex is not referenced for a triangle, and thus is unsigned. Each of the indices is specifically labeled 'a', 'b', and 'c' instead of an int array. This is is specically to demonstrate the relationship bewteen each index and the vertex color, normal and diffuse uv coordinate associated with each index.

For vertex color, colors are defined as four 8 bytes for RGBA for each indice respectively. This was used instead of four float values as it was deemed that using floats would cause too much unnecessary padding to the face list. Following that are normals for each indice. These are a vector float value with a combined sum of 1.0f to show the direction of the face for each index in the face. And finally is the diffuse uv coordinate for each of the face indices. If no diffuse texture is mapped to the material being used for this face, then these values can be left as 0.0f. 

## 6.2 JSON

```json
{
  "material" : int,
  "indices" : [a, b, c],
  "verex_color" : [
    "rgba('255, 255, 255, 1.0)", 
    "rgba('255, 255, 255, 1.0)" , 
    "rgba('255, 255, 255, 1.0)"
  ],
  "normals" : [ float[3], float[3], float[3] ],
  "diffuse_uv": [ float[2], float[2], float[2] ]
}
```

The content of the JSON format of this struct is identical to the binary, but the provided strings are different from the binary struct. The material id is given by an id. The indices are provided in an array. And then each property for each indice are provided in their own arrays. Vertex color is provided as a CSS style, "rgba(255, 255, 255, 1.0)" string as alpha is expected in the binary format. The normals are an array of three float arrays for the x, y and z normal vector for each one of the faces. And the diffuse_uv values is a list of three float arrays with two float values for S and T respectively.


