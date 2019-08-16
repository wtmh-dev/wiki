---
title: raspbian - buster
description: 
published: true
date: 2019-08-16T21:38:03.863Z
tags: 
---

# creating raspbian buster auto-interfaces with ArchLinux

## step 0 >>> 

### [install dependecies][1]

```
systemctl enable systemd-binfmt.service
systemctl start systemd-binfmt.service
yay -S --noconfirm qemu-arch-extra
yay -S --noconfirm qemu-user

# mount partition
mount -o rw /dev/mmcblk0p2  /mnt/raspbian
mount -o rw /dev/mmcblk0p1 /mnt/raspbian/boot
```

## step 1 >>>

### create auto-load supplicant wireless conf.

> vi /mnt/raspbian/boot/wpa_supplicant.conf

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
ap_scan=1
fast_reauth=1
country=ES

network={
	ssid="Your network's SSID"
	psk="Your network's password/psk"
	id_str="0"
	priority=100
}
```

### allow ssh connection 

> touch /mnt/raspbian/boot/ssh

## step 2 >>> chroot

```
# mount binds
mount --bind /dev /mnt/raspbian/dev/
mount --bind /sys /mnt/raspbian/sys/
mount --bind /proc /mnt/raspbian/proc/
mount --bind /dev/pts /mnt/raspbian/dev/pts

# ld.so.preload fix
sed -i 's/^/#/g' /mnt/raspbian/etc/ld.so.preload

# copy qemu binary
cp /usr/bin/qemu-arm-static /mnt/raspbian/usr/bin/

# chroot to raspbian
chroot /mnt/raspbian /bin/bash
```

## step 3 >>> do stuff into raspbian...

```
sudo su && cd && vi setup.sh
```

> replace all the values inside « »

```
#!/bin/bash

echo 'creating up.sh ################################################################################'

cat > /root/up.sh <<EOL
#!/bin/bash

apt install -y wget mawk msmtp msmtp-mta s-nail
EOL

echo 'chmod +x files ################################################################################'

chmod +x /root/up.sh

echo 'running up.sh ################################################################################'

sh /root/up.sh

echo 'creating nid.sh ################################################################################'

cat > /root/nid.sh <<'EOF'
#!/usr/bin/env bash
while true
do
  if ping -q -c 1 -W 1 8.8.8.8 >/dev/null; then
    IPPUB=`wget -qO- http://ipecho.net/plain ; echo`
    IPPRVWLAN0=`/sbin/ifconfig wlan0 | /usr/bin/awk '/inet /{print substr($2,1)}'`
    IPPRVETH0=`/sbin/ifconfig eth0 | /usr/bin/awk '/inet /{print substr($2,1)}'`
    HOSTNAME=`hostname -f`
    echo -e "Subject: $HOSTNAME boot details\r\n\r\n───────────────────────────────────────\n »»» Online details for $HOSTNAME: \n ··· Public  IP:       $IPPUB\n ··· Local   IP wlan0: $IPPRVWLAN0\n ··· Local   IP eth0:  $IPPRVETH0\n »»» Date: $(date +"%a %x %X")\n───────────────────────────────────────" | msmtp -a default «destination-user@domain.tld»
    exit 0
    else
      sleep 5
  fi
done
EOF

echo 'chmod +x files ################################################################################'

chmod +x /root/nid.sh


echo 'adding msmtp to root ################################################################################'

cat > /root/.msmtprc <<EOL
# Set default values for all following accounts.
defaults
auth           on
tls            on
tls_trust_file /etc/ssl/certs/ca-certificates.crt
logfile        ~/.msmtp.log

# OwnMailServer
account        «account-name»
host           «smtp.domain.tld»
port           587
from           «Anibal Aguila»
user           «user@domain.tld»
password       «email-password»

# Set a default account
account default : «account-name»
EOL

echo 'chmod 600 .msmtprc ################################################################################'

chmod 600 /root/.msmtprc

echo 'setting msmtp as mta ################################################################################'

echo "set mta=/usr/bin/msmtp" >> /etc/mail.rc

echo 'creating alliases main ################################################################################'

echo "aliases               /etc/aliases" >> /etc/msmtprc

echo 'creating alliases details ################################################################################'

cat > /etc/aliases <<EOL
# Send root to Joe and Jane
root: joe_smith@example.com, jane_chang@example.com
   
# Send everything else to admin
default: «user@domain.tld»
EOL


echo 'creating systemd timer ################################################################################'

cat > /etc/systemd/system/nid.timer <<EOL
[Unit]
Description=Send Public and Private IP interfaces at boot system

[Timer]
OnBootSec=1min
Unit=nid.service

[Install]
WantedBy=basic.target
EOL

echo 'creating systemd service ################################################################################'

cat > /etc/systemd/system/nid.service <<EOL
[Unit]
Description=Send Public and Private IP interfaces at boot system

[Service]
Type=oneshot
ExecStart=/bin/sh /root/nid.sh
EOL

echo 'enabling nid.timmer ################################################################################'

systemctl enable nid.timer

echo 'DONE ################################################################################'
```

## step 4 >>>

```
# revert ld.so.preload fix
sed -i 's/^#//g' /mnt/raspbian/etc/ld.so.preload

# unmount everything
umount /mnt/raspbian/{dev/pts,dev,sys,proc,boot,}
```
[1]: https://gist.github.com/htruong/0271d84ae81ee1d301293d126a5ad716