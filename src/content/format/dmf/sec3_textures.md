---
title: 3.0 Textures
pubDate: June 21, 2023
author: Kion
slug: dmf/textures
---

## 3.0 Textures

### 3.1 Binary

For the binary version, the texture structure is defined as follows. 

```c
typedef struct {
	char name[0x20];
	int16_t tex_id;
	uint16_t flip_y;
	uint32_t img_offset;
	uint32_t img_length;
	uint16_t img_width;
	uint16_t img_height;
	uint16_t wrap_s;
	uint16_t wrap_t;
	uint8_t nop[0x0c];
} DashTexture_t;
```

The struct starts with a name for the texture which is limited to 32 characters. Following that is the texture id, which should start at 0 for the first item in the list and increment by one after that. This attribute is generally for show as the position in the array takes priority over this value. As materials with a texture id reference will reference the position in the array, this attribute is included to make the binary file easier to trace through. Following that is binary true/false value for Flip Y. I can never keep track of which one is which so I'll probably have to make a table for which application does what to keep these consistent. 

The image offset is the offset inside the file where the image data can be found. And the image length is the length of the binary for the internal image file. Image files are always in PNG. It might be possible to use allow for different image formats such as DDS, JPEG, or TGA, but the image will need to be reliably parsed or written to from a vareity of applications. So for now it's thought that PNG offers the widest standard support across multiple lanaguages and multiple applications, and offers vaious coding and file compression. The image width and image height are also included. Followed by the settings for wrap S and T overfow settings, specifically clamp, repeat or mirror. 

The last 0x0c bytes of the file are used for padding to keep the struct length at 0x40. This could be used for filter settings, center, offset, rotation or mipmaps. But these are not currently supported and have been left out intentionally to avoid over-complication. They could be added into the file format definition if deemed neccessary or useful. 

### 3.2 JSON

```json
{
  "id" : int,
  "name" : string,
  "flip_y" : bool,
  "img" : "data:image/png;base64,...",
  "width" : int,
  "height" : int,
  "wrap_s" : enum("clamp","mirror","repeat"),
  "wrap_t" : enum("clamp","mirror","repeat")
}
```

For the JSON version, the id is a whole number, the name is a string in quotes (suggested limited to 32 characters), flip_y is a 0 or 1 boolean value. The image data is included as a base64 data string. The width and height are included as numbers, and wrap_s and wrap_t are enums, meaning they should have a specific string value, or an int that matches the index of the intended value.


