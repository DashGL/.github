---
title: 2.0 Concepts
pubDate: June 21, 2023
author: Kion
slug: dmf/concept
---

# 2.0 Concepts

## 2.1 Binary

The binary version of the Dash Model Format is intended as the starndard form. The reason for this is to provide a format that can be easily parsed by various 3D applications, tools and engines. The start of the binary file is given by the following 0x10 byte struct.

```c
typedef struct {
	uint32_t magic;
	uint16_t major_version;
	uint16_t minor_version;
	uint32_t header_offset;
	uint32_t header_count;
} DashMagic_t;
```

The first four bytes of the file are defined by the magic number 'DASH' or 0x48534144 in little endian. Following that is a 2 byte short value for the major version, and then a 2 byte short value for the minor version. As stated in the version section of this document, minor version changes are intended for development, with full version changes intended for stable releases.  

The next 4 bytes in the header is the offset to the attribute table which defines the geometry of the asset. In the current version this will always be at offset 0x10. Originally it was intended for space to be allowed for meta data to be inserted into the top of the file if needed, but a META data attribute will likely be included in future definitions. The last four bytes in the header are for the count of attributes in the attribute table. This is redundant and should always be 6 for the current 1.5 version, and will likely be increased to 7 when the meta attribute is introduces.  

## 2.2 JSON

For the JSON version, a single file defines a single asset, so the file format is defined by a single JSON object.

```json
{
  "format" : "DASH",
  "version" : "1.5"
  ...
}
```

In place of the magic number, there should be a key "format" with the string "DASH" in the top level of the object to define that the JSON object is intended to be interpretted as a DASH JSON file. A key "version" should also be included so that the importer can accurately recognize the struct values that need to be defined for each attribute.

## 2.3 Binary

The main contents of the geometry are all defined by the attribute table at the top of the file. This is included to act as a table of contents to try and provide as much information to the developer as possible. The binary file format should be well documented, but steps should taken to make the file format as intuitive and easy as possible to reverse engineer to make it as simple and flexible as possible to work with.

| Offset | 0x00 | 0x04 | 0x08 | 0x0c |
| ------ | ------ | ------ | ------ | ------ |
| 0x0000 | DASH | major(1) / minor(5) version | Attr Table Offset (0x10) |  Attr Count (6) | 
| 0x0010 | TEX | flags(0) | Texture List Offset |  Texture Count | 
| 0x0020 | MAT | flags(0) | Material List Offset |  Material Count | 
| 0x0030 | VERT | flags(0) | Vertex List Offset |  Vertex Count | 
| 0x0040 | FACE | flags(0) | Face List Offset |  Face Count | 
| 0x0050 | BONE | flags(0) | Bone List Offset |  Bone Count | 
| 0x0060 | ANIM | flags(0) | Anim List Offset |  Anim Count | 

The table above shows the header for the binary version of the file format. The first 0x10 bytes show the Magic number structure that is defined in the previous section of this document. Following that is an attribute table for the following attributes, "Textures", "Materials", "Vertices", "Faces", "Bones" and "Animations". For the table the first four bytes is a magic number table to make it easier to read and trace from looking at the file. The four bytes after that are reserved for flags, however these are likely not required and act as padding so that each attribute struct to have a length of 0x10 bytes. The next four bytes are the offset to the defined attribute list, and the last four bytes is the length of the structs defined in the list.

```c
#define DASH_LABEL_TEX 	0x00786574
#define DASH_LABEL_MAT 	0x0074616D
#define DASH_LABEL_VERT 0x74726576
#define DASH_LABEL_FACE 0x65636166
#define DASH_LABEL_BONE 0x656E6F62
#define DASH_LABEL_ANIM 0x6D696E61

typedef struct {
	uint32_t label;
	uint32_t flags;
	uint32_t offset;
	uint32_t count;
} DashAttribute_t;

typedef struct {
	DashAttribute_t tex;
	DashAttribute_t mat;
	DashAttribute_t vert;
	DashAttribute_t face;
	DashAttribute_t bone;
	DashAttribute_t anim;
} DashAttrTable_t;
```

## 2.4 JSON

```json
{
  "format" : "DASH",
  "version" : "1.5"
  "textures" : [ ... ],
  "materials" : [ ... ],
  "verterices" :
  "faces" : [ ... ],
  "skeleton" : [ ... ],
  "animations" : [ ... ]
}
```


