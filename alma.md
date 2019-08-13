---
title: ALMA - Arch Linux Mobile Appliance
description: 
published: true
date: 2019-08-13T05:08:02.571Z
tags: 
---

# creating nomad conf

## step 1 >>> create alma drive

```
sudo alma create /dev/disk/by-id/usb-SanDisk_Ultra_USB_3.0_XXXXXXXXXXXXXXXXXXX-0:0 -e
```

## step 2 >>> chroot to move into the fresh installation of arch

```
sudo alma chroot /dev/disk/by-id/usb-SanDisk_Ultra_USB_3.0_XXXXXXXXXXXXXXXXXXX-0:0
```

## step 3 >>> setup essential config >>> essential.sh

```

```