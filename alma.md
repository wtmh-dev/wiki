---
title: ALMA - Arch Linux Mobile Appliance
description: 
published: true
date: 2019-08-13T14:34:13.537Z
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

## step 3 >>> vi essential.sh

```
#!/bin/bash

ln -s /dev/null /etc/udev/rules.d/80-net-setup-link.rules
cp /etc/netctl/examples/ethernet-dhcp /etc/netctl/eth0-arch_usb

pacman -S iw dhclient ifplugd sudo dnsmasq dialog wireless_tools polkit net-tools openssh --noconfirm

echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config

systemctl enable sshd
systemctl enable dnsmasq
systemctl enable dhclient
systemctl enable netctl-ifplugd@eth0.service
systemctl enable netctl-auto@wlan0.service

systemctl disable dhcpcd
systemctl disable systemd-networkd

echo 'server=8.8.8.8' >> /etc/dnsmasq.conf
echo 'server=8.8.4.4' >> /etc/dnsmasq.conf
echo '#############################' >> /etc/resolvconf.conf
echo 'name_servers="::1 127.0.0.1"' >> /etc/resolvconf.conf
echo -e 'asdc\nasdc' | (passwd root)

rm /etc/systemd/system/multi-user.target.wants/NetworkManager.service

echo 'now move the wlan0-< ESSID > to /etc/netctl/'
```

## step 4 >>> vi /etc/netctl/wlan0-< ESSID >

```
Description='Automatically generated profile by wifi-menu'
Interface=wlan0
Connection=wireless
Security=wpa
ESSID=XXXXXXXXX
IP=dhcp
Key=XXXXXXXXX
```
