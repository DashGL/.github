---
title: Introduction
description: Please god save me jeeebus
download: https://github.com/scurest/apicula
pubDate: June 21, 2023
heroImage: /resources/frontispiece.png
author: Kion
slug: abebe
tag: Tool
name: scurest
link: https://github.com/scurest
---

![Dash Model Format compared to other formats](/formats/dash_position.png)

The Dash Model Format (.dmf) is a simple-as-possible interchange format. The format is intended for individual rigged assets with animations. The intent of the format is to have an entire assets contained in a single file in a common format that can be reliably used with several different 3D content and creation tools.

## Motivation

![Motivation for usage of Dash Model Format](/formats/dmf_map.png)

When I first started working with classic formats, I thought that reverse engineering the formats would be the end of the journey and that there would be a widely supported modern format that I could port to. At first I used the Threejs json format, as it was the easiest to work with, but it was quickly depreciated after I started working with it. After that I tried to write an exporter to .dae, but found the xml format quite difficult to work with and over confusing. Gltf finally came out, and after trying to work with it, I found it too complicated for my use case. By this time I've been messing around with the API's from Threejs, Blender and Noesis trying to support common formats, when I finally came to the realization that I could define my own format that I could reliably write plugins for different applications to be able to use that format directly, as opposed to trying to write to a common format and then watch it break, when actually trying to use it.

## Basics

![References to sections in dash model format](/formats/dash_basics.png)

The Dash Model Format is a binary format built around fixed-length structures to create a model that is easy to parse in any language. The structure of the file has a magic number at the top, a table that defines 6 attributes (textures, materials, vertices, faces, bones, and animations). Each attribute in the table provides a count and an offset to a section in the file.

## Design Goals

The Dash Model format is based around a single asset. The figure above provides some contrast, simplier models only define meshes with textures but no bones or animations. More complicated formats with define entire scenes with rigged assets, lights and cameras. The Dash Model Format is intended to be a format that defines a single rigged asset. The way the file defines a single assets is intended to target a specific "Dreamcast-style" use-case, with skeletal rigging (no morph targets), and a single diffuse uv channel. The idea is to create a specific use-case to work with that all 3D modeling tools can realiably work with and target that. If any requests or required changes are needed to be implemented they will be planned and implemented in future versions.

## Versioning

The intended version scheme was to have stable releases be 1.0, 2.0, ect. But the original 1.0 release under-went changes and the first "stable" release is 1.5. So all tools will be built around targetting that. Future changes will be planned and the level of version incrementation will be based around the implementation plan. It is itended for subversions (2.1, 2.23, ect) to be developer versions, and full versions (2.0, 3.0) to be end-user-friendly releases, but we'll see how well that works out.

## File Extensions

The dash model format comes in two formats, the binary format and a companion json format. The intended distribution format is the binary format, but the tools will be designed to parse the binary format into the json format before being read into the application. And like-wise at export time, create a json represention before converting to binary so that the user can choose which format they would like to write to. Both formats will have the extension of .dmf. The binary version will have the magic number 'DASH' at the beginning, and if that is not found, the JSON format will be assumed.

## JSON Encoding

JSON encoding must use ASCII and fit inside the constraints defined in the binary version of the file format. (Name strings must fit to 32 characters)
