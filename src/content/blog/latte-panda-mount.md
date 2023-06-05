---
title: "Mounting the Latte Panda"
description: "Making notes on how to format and mount partitions using Debian"
pubDate: "August 12, 2019"
heroImage: "/img/mounting-hdd-linux-cmd-987793055.jpeg"
author: Kion
---

So something that’s kind of bullshit is the Debian installer wasn’t able to partition the onboard sd-card slot and mount /var for me during the installation. I figured that I could just mount the micro-sd card post install, except the micro-sd reader wasn’t seen at all by the operating system. So I ended up having to get a usb adapter for the micro-sd card. Kind of annoyed as if I wanted to use a USB drive for storage, I probably would have bought one. Storage is probably one of the wonky aspects of the first board, and by adding m.2 slots something they address in the latte panda alpha. I don’t have that kind of money which is why I’m using the old cheap one, but it’s still weird that this board doesn’t have a sata or m.2 sata slot, it would make the board a lot more functional.

So now that I have my micro-sd card mounted via usb like a pleb, the next step is to mount it on /var. So I need to copy everything in var to the sd card, add the micro-sd card to fstab, and then reboot and pray everything works.

```bash
$ sudo mount /dev/sda1 /mnt
$ sudo /bin/cp -arpx /var/\* /mnt
$ sudo touch /mnt/sd.txt
```

Looks like I need the uuid to mount with fstab. I don’t think there should be too much problem posting the actual values for a local drive.

```bash
$ sudo blkid
/dev/mmcblk0: PTUUID="a7e4664a-ce38-4434-9c46-977861c882fa" PTTYPE="gpt"
/dev/mmcblk0p1: UUID="C07C-6F0D" TYPE="vfat" PARTUUID="62b5c747-0bcd-4662-b4e4-a0f673edfb53"
/dev/mmcblk0p2: UUID="5f141daa-1b45-49a0-97b1-629e9b81eaed" TYPE="ext4" PARTUUID="ab33c7b1-e45c-4e7d-a5b7-edc7fe87f9cb"
/dev/mmcblk0p3: UUID="c452f942-30d8-4d91-aa64-9541a0174257" TYPE="swap" PARTUUID="1e044fb1-40dd-4e15-a2ff-60b0118f5375"
/dev/sda1: UUID="526074a8-1ce7-4e30-a3df-2f693383ec78" TYPE="ext4" PARTUUID="17869b7d-01"
```

So I can probably copy the root mount (since it’s for ext4), and then use the same format, change the uuid and mount on `/var`.

```bash
\# # / was on /dev/mmcblk0p2 during installation
UUID=5f141daa-1b45-49a0-97b1-629e9b81eaed /               ext4    errors=remount-ro 0       1
# /boot/efi was on /dev/mmcblk0p1 during installation
UUID=C07C-6F0D  /boot/efi       vfat    umask=0077      0       1
# swap was on /dev/mmcblk0p3 during installation
UUID=c452f942-30d8-4d91-aa64-9541a0174257 none            swap    sw              0       0
UUID=526074a8-1ce7-4e30-a3df-2f693383ec78 /var               ext4    errors=remount-ro 0       1
```

Next step is to reboot and hope the the latte panda can actually reboot. This is a completely different side note, but the latte panda has three buttons. And I always have to mash random combinations of each button several times until it actually boots. So i’m hoping it will cooperate and reboot for me on the network, otherwise I might have to rethink this.

It looks like the latte won’t boot without a button being pressed. That is pretty objectively terrible. Though after a successful reboot it looks like thr sd.txt file I made is there, so I’ll just try and avoid rebooting over the network, when possible. And continue to ponder the utility of using a micro-sd as a standard USB drive. In terms of price it’s probably the same thing as smaller drives generally end up being integrated micro-sd adapaters, hard drives make noise when reading from them, and sata-to-usb SSD’s are kind of in their own class of stupid. I’m mostly salty about not being able to use the on-board micro-sd card.