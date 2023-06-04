---
title: "FBX SDK Linux Install (Centos 7)"
description: "Instructions for how into install FBX SDK on CentOS 7"
pubDate: "March 5, 2019"
heroImage: "/img/FBX_600x300-3424614133.jpeg"
author: Kion
---


I bought a LattePanda to serve as a standard device for testing with the FBX SDK. Since the board is a x86_64 processor with 2GB of memory and 32GB of onboard storage for about a hundred dollars, I think it serves as a representation of the minimum requires for compiling the FBX SDK and hardware that theoretically anyone should be able to get their hands on to recreate these steps.

For an opperating system I’m using Linux and more specifically Centos 7. The reasons for using Linux should hopefully be self-evident to the kind of person reading this (open source). And the distribution used is CentOS 7 mostly because it works, although using the Redhat ecosystem isn’t likely a downside. I wasn’t able to get Debian working, and I’m not familiar with Arch or OpenSuse enough to write instructions.

For prerequisites, we have the CentOS 7 installed on a computer (the latter panda in this case). We have access to the console. And we are logged in as a user with root (sudo) privileges. We will be installing the FBX SDK to the home directory.

### Install Prerequisites

```bash
$ sudo yum install gcc make gcc-c++ glibc-devel.i686 glibc-devel libuuid-devel libuuid-devel.i686 libstdc++-devel.i686 libstdc++-devel wget
```

Download and Install SDK (to user’s home directory)

```bash
$ cd /home/$USER/
$ wget http://download.autodesk.com/us/fbx/2019/2019.0/fbx20190_fbxsdk_linux.tar.gz
$ tar xvzf fbx20190_fbxsdk_linux.tar.gz
$ rm fbx20190_fbxsdk_linux.tar.gz
$ ./fbx20190_fbxsdk_linux
```

To install the SDK, you will be asked the path for the directory, the default will be the current (home) directory. You will then be asked to agree to the license agreeement, and if you want to read the read me or not, which you can answer “yes” or “no” accordingly.

The following files should now be inside you home directory:

```bash
-rwxr-xr-x.  1 kion kion 74396274 Aug 15  2017 fbx20190_fbxsdk_linux
-rw-rw-r--.  1 kion kion      105 Aug 15  2017 FBX_SDK_Online_Documentation.html
drwxrwxr-x.  3 kion kion       36 Aug 15  2017 include
-rw-rw-r--.  1 kion kion     1407 Aug 15  2017 Install_FbxSdk.txt
drwxrwxrwx.  3 kion kion       17 Mar  5 22:47 lib
-rw-rw-r--.  1 kion kion    96857 Aug 15  2017 License.txt
drwxrwxr-x.  3 kion kion       17 Mar  5 22:44 obj
drwxrwxrwx. 28 kion kion     4096 Aug 15  2017 samples
```

We need to make a few changes. First we don’t need the installer, so we can remove that. Next we need to rename the “gcc4” folder in the “lib” directory to “gcc”. And then to get the samples to compile, we need to replace the “gcc4” references in the makefiles to “gcc”.

```bash
$ rm fbx20190_fbxsdk_linux
$ mv lib/gcc4 lib/gcc
$ find samples -type f -name 'Makefile' | xargs sed -i 's/gcc4/gcc/g'
$ chmod -R 771 lib
$ chmod -R 771 samples
```

From there the sample programs should be able to complile.

```bash
$ cd samples/Animation
$ make
mkdir -p ../../obj/x86/release/Animation
gcc  -m32  -DFBXSDK_SHARED -I../../include -c main.cxx -o main.o
mv main.o ../../obj/x86/release/Animation
mkdir -p ../../obj/x86/release/Animation
gcc  -m32  -DFBXSDK_SHARED -I../../include -c ../Common/Common.cxx -o ../../obj/x86/release/Animation/Common.o
mkdir -p ../../bin/x86/release/Animation
gcc  -m32  -o ../../bin/x86/release/Animation/Animation ../../obj/x86/release/Animation/main.o ../../obj/x86/release/Animation/Common.o -L../../lib/gcc/x86/release -lfbxsdk -lm -lrt -luuid -lstdc++ -lpthread -ldl -Wl,-rpath /home/kion/samples/Animation/../../lib/gcc/x86/release
```

From there you should be able to run the executable which will be located in the bin folder of the directory which the FBX SDK was installed (in this case the user’s home directory).

```bash
$ cd ../../bin/x86/release/Animation
$ ./Animation
Autodesk FBX SDK version 2019.0 Release (b92b15b23)
Program Success!
```