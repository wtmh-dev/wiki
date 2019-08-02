---
title: hardware specifications
description: How To Find Hardware Specifications On Linux
published: true
date: 2019-08-02T14:52:54.594Z
tags: 
---

# Header

## 1\. LSHW

**Lshw (Hardware Lister)** is a simple, yet full\-featured utility that provides detailed information on the hardware configuration of a Linux system. It can report exact memory configuration, firmware version, mainboard configuration, CPU version and speed, cache configuration, bus speed etc. Information can be output in plain text, XML or HTML.

It currently supports DMI (x86 and EFI only), Open Firmware device tree (PowerPC only), PCI/AGP, ISA PnP (x86), CPUID (x86), IDE/ATA/ATAPI, PCMCIA (only tested on x86), USB and SCSI.

```
man lshw
sudo lshw
sudo lshw -html
sudo lshw -xml
sudo lshw -json
sudo lshw -short
sudo lshw -businfo
sudo lshw -class processor
sudo lshw -class system
sudo lshw -class disk
sudo lshw -class network
sudo lshw -class memory
sudo lshw -class storage -class power -class volume
sudo lshw -short -class processor
lshw -sanitize
```


