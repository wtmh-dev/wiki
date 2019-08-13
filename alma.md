---
title: ALMA - Arch Linux Mobile Appliance
description: 
published: true
date: 2019-08-13T09:35:05.430Z
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

## step 3 >>> setup essential.sh

```
#!/bin/bash

ln -s /dev/null /etc/udev/rules.d/80-net-setup-link.rules
cp /etc/netctl/examples/ethernet-dhcp /etc/netctl/eth0-arch_usb

pacman -S openssh dialog wpa_supplicant dhclient ppp --noconfirm

ln -sf /usr/share/zoneinfo/Europe/Madrid /etc/localtime
hwclock --systohc --utc
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
locale-gen

sed -i 's/^MODULES=()/MODULES=(i915 xfs)/' /etc/mkinitcpio.conf
sed -i 's/^HOOKS=(base udev autodetect modconf block filesystems keyboard fsck)/HOOKS=(base udev autodetect modconf block encrypt lvm2 filesystems keyboard fsck)/' /etc/mkinitcpio.conf
mkinitcpio -p linux

grub-install
mkdir /run/lvm
mount --bind /hostrun/lvm /run/lvm
grub-mkconfig -o /boot/grub/grub.cfg

systemctl enable dhcpcd
systemctl enable systemd-networkd
systemctl enable sshd
```

## step 4 >>> setup wlan0Up.sh

```
#!/bin/bash

wpa_supplicant -B -i wlan0 -c <(wpa_passphrase ColaCao ccNH2ohDmmseBR6Z7Hbs)

sleep 3

dhclient wlan0
# dhcpcd wlan0

sleep 9

echo "wlan0:  ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓ ↓"
\

ifconfig wlan0 | grep "inet "

\
echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~"
```
