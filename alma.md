---
title: ALMA - Arch Linux Mobile Appliance
description: 
published: true
date: 2019-08-13T04:59:44.094Z
tags: 
---

# creating nomad conf

## step 1

```
sudo alma create /dev/disk/by-id/usb-SanDisk_Ultra_USB_3.0_XXXXXXXXXXXXXXXXXXX-0:0 -e

# use chroot to move into the fresh installation of arch

sudo alma chroot /dev/disk/by-id/usb-SanDisk_Ultra_USB_3.0_XXXXXXXXXXXXXXXXXXX-0:0


```