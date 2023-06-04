---
title: "Linux – FBX SDK Install Debian"
description: "Writing up documentation for how to install FBX SDK on Debian Linux"
pubDate: "April 14, 2019"
heroImage: "/img/file_document_format_28-512-3098560071.png"
author: Kion
---


[](https://blog.dashgl.com/?p=252) [Kion](https://blog.dashgl.com/?author=1) [fbx](https://blog.dashgl.com/?cat=7)

I’ve had enough people ask me about FBX support that I figured I should start looking into how to support this file format. Autodesk supplies SDK for converting to and converting from their format here: https://www.autodesk.com/developer-network/platform-technologies/fbx-sdk-2019-0. Specifically it seems like there’s a library for working with C/C++, and then python bindings as well. I’m going to be honest and say that python is probably the better choice to work with, but we’ll start with the C version.

So first step is we download the SDK for linux, run the installer and extract to the local directory.  
From there we need to start by trying to build the examples.

$ sudo apt-get install make gcc

Okay and then when we build we get the following error:

kion@server1:~/Gitlab/fbx\_c\_sdk/samples/Animation$ make
mkdir -p ../../obj/x86/release/Animation
gcc4  -m32  -DFBXSDK\_SHARED -I../../include -c main.cxx -o main.o
make: gcc4: Command not found
Makefile:67: recipe for target 'main.o' failed
make: \*\*\* \[main.o\] Error 127

I guess we could try to be lazy and include an alias for gcc4 to gcc in bashrc. Let’s try to edit the makefile to see if that makes it work.

We get another error:

kion@server1:~/Gitlab/fbx\_c\_sdk/samples/Animation$ make
mkdir -p ../../obj/x86/release/Animation
gcc  -m32  -DFBXSDK\_SHARED -I../../include -c main.cxx -o main.o
gcc: error trying to exec 'cc1plus': execvp: No such file or directory
Makefile:67: recipe for target 'main.o' failed
make: \*\*\* \[main.o\] Error 1

Let’s try to apt-get our way out of this

$ sudo apt-get install --reinstall build-essential

Okay, error fixed, next error:

kion@server1:~/Gitlab/fbx\_c\_sdk/samples/Animation$ make
mkdir -p ../../obj/x86/release/Animation
gcc  -m32  -DFBXSDK\_SHARED -I../../include -c main.cxx -o main.o
In file included from /usr/include/c++/6/stdlib.h:36:0,
                 from ../../include/fbxsdk/fbxsdk\_def.h:23,
                 from ../../include/fbxsdk.h:43,
                 from main.cxx:29:
/usr/include/c++/6/cstdlib:41:28: fatal error: bits/c++config.h: No such file or directory
 #include 

So next apt-get command attept

sudo apt-get install gcc-multilib g++-multilib

Another quick error in the make file, CC and LD need to be gcc, everything else is generally a path, so it can stay as gcc4 (or you can rename/copy the lib/gcc4 folder).

And now I’m getting the error

kion@server1:~/Gitlab/fbx\_c\_sdk/samples/Animation$ make
mkdir -p ../../obj/x86/release/Animation
gcc  -m32  -DFBXSDK\_SHARED -I../../include -c main.cxx -o main.o
mv main.o ../../obj/x86/release/Animation
mkdir -p ../../obj/x86/release/Animation
gcc  -m32  -DFBXSDK\_SHARED -I../../include -c ../Common/Common.cxx -o ../../obj/x86/release/Animation/Common.o
mkdir -p ../../bin/x86/release/Animation
gcc  -m32  -o ../../bin/x86/release/Animation/Animation ../../obj/x86/release/Animation/main.o ../../obj/x86/release/Animation/Common.o -L../../lib/gcc4/x86/release -lfbxsdk -lm -lrt -luuid -lstdc++ -lpthread -ldl -Wl,-rpath /home/kion/Gitlab/fbx\_c\_sdk/samples/Animation/../../lib/gcc4/x86/release
/usr/bin/ld: cannot find -luuid

So there’s some uuid library, So far tried util-linux, uuid-dev and libuuid1. None of them seem to make it work so far. I think there’s some kind of flag that needs to be passed in for cross compiling uuid. Though I’m running this on an old(er) debian server. So I might try to use a different distribution.

Edit:

CentoS worked!! Here’s the magic yum stuff:

sudo yum install gcc make gcc-c++ glibc-devel.i686 glibc-devel libuuid-devel libuuid-devel.i686

Edit:

Tried adding 32 bit library to Debian. Still no luck, but at least it works on CentOS which is okay for now.

sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install libuuid1:i386

Edit:

Finally got it working. All it needs after this is

$ sudo apt-get install uuid-dev uuid-dev:i386

And the make file for the samples will run. I should probably create a clean version of the install on Debian.