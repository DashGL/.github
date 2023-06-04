---
title: "Static Dash Format"
description: "Creating a bullet list of attributes to layout per section"
pubDate: "March 13, 2019"
heroImage: "/img/binary-view.jpeg"
author: Kion
---

### Header

DASH v1
IMG num ofs
TEX num ofs
MAT num ofs
VERT num ofs
FACE num ofs
BONE num ofs
ANIM num ofs

### Images

IMG id | type
offset
length
width | height

### Textures

TEX id | IMG id
wrap S
wrap T
flipY

### Materials

MAT id | TEX id
use blend
blend src | dst
opacity 0.0 – 1.0
transparent | visible
vertex color bool
skinning bool
side const
diffuse red 0.0 – 1.0
diffuse green 0.0 – 1.0
diffuse blue 0.0 – 1.0
diffuse alpha 0.0 – 1.0

### Vertices

position x
position y
position z
index 0 | index 1
index 2 | index 3
weight 0
weight 1
weight 2
weight 3

### Faces

MAT id | a index
b index | c index
a color
b color
c color
a normal x
a normal y
a normal z
b normal x
b normal y
b normal z
c normal x
b normal y
b normal z
a texture u
a texture v
b texture u
b texture v
c texture u
c texture v

### Bones

bone id | parent id
position x
position y
position z
rotation x
rotation y
rotation z
scale x
scale y
scale z

### Anims

Anim id | num frames
offset
length
time

### Anim Format

Frame id | Bone id
time
pos | rot | scale
position x
position y
position z
rotation x
rotation y
rotation z
rotation w
scale x
scale y
scale z