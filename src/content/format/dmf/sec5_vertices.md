---
title: 5.0 Vertices
pubDate: June 22, 2023
author: Kion
slug: dmf/vertices
---

# 5.0 Vertices

## 5.1 Binary

Now we get into the vertex list which starts to demonstrate how the Dash Model Format takes rigged assets and distils them down into something simple. Each vertex in the binary vertex list is given by the following struct.

```c
typedef struct {
	float pos[3];
	int16_t skin_index[4];
	float skin_weight[4];
} DashVertex_t;
```

The position is three floats for the x, y, and z coordinates of each vertex. Each vertex has up to four slots to be influenced by bones in the skeleton. The combined total of the weights should be 1.0f. The skin_index property gived the id of tbe bone in the skeleton to be influenced by. The value is a signed short value, so that -1 can be used to store the bind pose. The following weight value has four float, one for each of the slot values. The most common occurence will likely be a single bone weight suck as the example of skin_index: [4, 0, 0, 0] skin_weight: [1.0f, 0.0f, 0.0f, 0.0f] where 100% of the vertex is weighted to the bone index 4. For multiple influences skin_index: [4, 5, 0, 0] skin_weight: [0.75f, 0.25f, 0.0f, 0.0f], you have 75% of the vertex weight bound to bone index 4 and 25% to bone index 5. If no bones are declared, these values can be ignored, and the values can be filled with 0.

## 5.2 JSON

```json
{
  "pos" : float[3],
  "skin_index" : int[4],
  "skin_weight" : float[4]
}
```

The JSON format of the vertex closely follows the binary format. The key values for each struct are given as the strings "pos", "skin_index", "skin_weight". The values for "pos", and "skin_weight" are float values, and "skin_index" should be whole (non-decimal) number values.

