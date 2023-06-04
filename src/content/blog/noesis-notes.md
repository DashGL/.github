---
title: "Noesis Notes"
description: "Code snippets for working with Noesis plugins"
pubDate: "March 29, 2019"
heroImage: "/img/noesis_webshot_01-491478975.jpeg"
author: Kion
---

Got bones and weights working, next step is to figure out how to implement animations.

```python
class NoeBone:
    def __init__(self, index, name, matrix, parentName = None, parentIndex = -1):
        self.index = index
        self.name = name
        self.setMatrix(matrix)
        self.parentName = parentName
        self.parentIndex = parentIndex #parent index may be specified instead of parentName, if it's more convenient. this is an index corresponding to self.index, and not the position in the list.
    def __repr__(self):
        return "(NoeBone:" + repr(self.index) + "," + self.name + "," + repr(self.parentName) + "," + repr(self.parentIndex) + ")"

    def setMatrix(self, matrix):
        if not isinstance(matrix, NoeMat43):
            noesis.doException("Invalid type provided for bone matrix")
        self._matrix = matrix
    def getMatrix(self):
        return self._matrix


#keyframe data type
class NoeKeyFramedValue:
    #value may be NoeQuat, NoeVec3, float, etc. depending on the data type specified
    def __init__(self, time, value):
        self.time = time
        self.value = value
        self.componentIndex = 0
    def __repr__(self):
        return "NoeKFVal(time:" + repr(self.time) + " value:" + repr(self.value) + ")"

    def setComponentIndex(self, componentIndex):
        self.componentIndex = componentIndex

#keyframed bone class
class NoeKeyFramedBone:
    def __init__(self, boneIndex):
        self.boneIndex = boneIndex
        self.setRotation([])
        self.setTranslation([])
        self.setScale([])

    #for the set methods, keys should be a list of NoeKeyFramedValue or an object with similarly available members
    def setRotation(self, keys, type = noesis.NOEKF_ROTATION_QUATERNION_4, interpolationType = noesis.NOEKF_INTERPOLATE_LINEAR):
        self.rotationKeys = keys
        self.rotationType = type
        self.rotationInterpolation = interpolationType
    def setTranslation(self, keys, type = noesis.NOEKF_TRANSLATION_VECTOR_3, interpolationType = noesis.NOEKF_INTERPOLATE_LINEAR):
        self.translationKeys = keys
        self.translationType = type
        self.translationInterpolation = interpolationType
    def setScale(self, keys, type = noesis.NOEKF_SCALE_SCALAR_1, interpolationType = noesis.NOEKF_INTERPOLATE_LINEAR):
        self.scaleKeys = keys
        self.scaleType = type
        self.scaleInterpolation = interpolationType


#keyframed animation class
class NoeKeyFramedAnim:
    def __init__(self, name, bones, kfBones, frameRate = 20.0, flags = 0):
        noesis.validateListType(bones, NoeBone)
        noesis.validateListType(kfBones, NoeKeyFramedBone)
        self.name = name
        self.bones = bones
        self.kfBones = kfBones
        self.frameRate = frameRate
        self.flags = flags
    def __repr__(self):
        return "(NoeKFAnim:" + self.name + ")"


#main animation class
#bones must be a list of NoeBone objects, frameMats must be a flat list of NoeMat43 objects
class NoeAnim:
    def __init__(self, name, bones, numFrames, frameMats, frameRate = 20.0, flags = 0):
        noesis.validateListType(bones, NoeBone)
        noesis.validateListType(frameMats, NoeMat43)
        self.name = name
        self.bones = bones
        self.numFrames = numFrames
        self.frameMats = frameMats
        self.setFrameRate(frameRate)
        self.flags = flags
    def __repr__(self):
        return "(NoeAnim:" + self.name + "," + repr(self.numFrames) + "," + repr(self.frameRate) + ")"

    def setFrameRate(self, frameRate):
        self.frameRate = frameRate
```

Example Code:

```python
bones = []
numBones = bs.readInt()
for i in range(0, numBones):
    bone = noepyReadBone(bs)
    bones.append(bone)

anims = []
numAnims = bs.readInt()
for i in range(0, numAnims):
    animName = bs.readString()
    numAnimBones = bs.readInt()
    animBones = []
    for j in range(0, numAnimBones):
        animBone = noepyReadBone(bs)
        animBones.append(animBone)
    animNumFrames = bs.readInt()
    animFrameRate = bs.readFloat()
    numFrameMats = bs.readInt()
    animFrameMats = []
    for j in range(0, numFrameMats):
        frameMat = NoeMat43.fromBytes(bs.readBytes(48))
        animFrameMats.append(frameMat)
    anim = NoeAnim(animName, animBones, animNumFrames, animFrameMats, animFrameRate)
    anims.append(anim)
```

More sample codes: **./plugins/python/fmt_gamebryo_nif.py**

```python
def loadTransformData(self, bs):
    if self.nif.fileVer >= nifVersion(20, 5, 0, 2):
        self.loadObject(bs)
        self.rotKeys = []
        self.trnKeys = []
        self.sclKeys = []
        self.rotKeyType = noesis.NOEKF_INTERPOLATE_LINEAR
        self.trnKeyType = noesis.NOEKF_INTERPOLATE_LINEAR
        self.sclKeyType = noesis.NOEKF_INTERPOLATE_LINEAR

        numKeys = bs.readUInt()
        if numKeys > 0:
            rotKeyType = bs.readUInt()
            if rotKeyType == 0 or rotKeyType == 1 or rotKeyType == 4:
                self.rotKeyType = noesis.NOEKF_INTERPOLATE_LINEAR
            else:
                print("WARNING: Unsupported rotation key type:", rotKeyType)
                return
            for i in range(0, numKeys):
                if rotKeyType == 0 or rotKeyType == 1:
                    #quats
                    keyTime = bs.readFloat()
                    w = bs.readFloat()
                    keyVal = NoeQuat( (bs.readFloat(), bs.readFloat(), bs.readFloat(), w) ).toMat43(1).toQuat()
                    self.rotKeys.append(NoeKeyFramedValue(keyTime, keyVal))
                elif rotKeyType == 4:
                    #radians
                    xyz = [ [], [], [] ]
                    for i in range(0, 3):
                        numSubKeys = bs.readUInt()
                        if numSubKeys > 0:
                            radianKeyType = bs.readUInt()
                            for j in range(0, numSubKeys):
                                keyTime = bs.readFloat()
                                if radianKeyType == 0 or radianKeyType == 1:
                                    xyz[i].append(NoeKeyFramedValue(keyTime, bs.readFloat()*noesis.g_flRadToDeg))
                                elif radianKeyType == 2:
                                    #possible todo - interpolate as bezier
                                    xyz[i].append(NoeKeyFramedValue(keyTime, bs.readFloat()*noesis.g_flRadToDeg))
                                    bezierIn = bs.readFloat()
                                    bezierOut = bs.readFloat()
                                else:
                                    print("WARNING: Unsupported radian rotation type:", radianKeyType)
                                    return
                    #this is largely untested, because the model i found using it only used it for the eyes,
                    #and it's hard to tell if it really works just based on eyes. (that's what she said)
                    xyz = rapi.mergeKeyFramedFloats(xyz)
                    for anglesKey in xyz:
                        self.rotKeys.append(NoeKeyFramedValue(anglesKey.time, NoeAngles(anglesKey.value).toMat43_XYZ().toQuat()))
```

It looks like key framed value is going to be the best option for implementing the version of animations I have, which defines a time and then gives a set of transformations at that given time.