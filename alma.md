---
title: ALMA - Arch Linux Mobile Appliance
description: 
published: true
date: 2019-08-15T20:10:06.248Z
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

## step 3 >>> prepare the script

```
cd && vi setup.sh
```

> replace all the values inside *« »*

```
#!/bin/bash

echo 'creating up.sh ################################################################################'

cat > /root/up.sh <<EOL
#!/bin/bash

ln -s /dev/null /etc/udev/rules.d/80-net-setup-link.rules
cp /etc/netctl/examples/ethernet-dhcp /etc/netctl/eth0-arch_usb

#install-packages#

echo 'PermitRootLogin yes' >> /etc/ssh/sshd_config

systemctl enable sshd
systemctl enable dnsmasq
systemctl enable dhclient
systemctl enable netctl-ifplugd@eth0.service
systemctl enable netctl-auto@wlan0.service
systemctl enable dhcpcd

systemctl disable systemd-networkd

echo 'server=8.8.8.8' >> /etc/dnsmasq.conf
echo 'server=8.8.4.4' >> /etc/dnsmasq.conf
echo '#############################' >> /etc/resolvconf.conf
echo 'name_servers="::1 127.0.0.1"' >> /etc/resolvconf.conf
echo -e '«pass»\n«pass»' | (passwd root)

rm /etc/systemd/system/multi-user.target.wants/NetworkManager.service
EOL

echo 'distribution based for up.sh ################################################################################'

if [ -f /etc/redhat-release ] ; then
    DIST='RedHat'
    PSUEDONAME=$(sed s/.*\(// < /etc/redhat-release | sed s/\)//)
    REV=$(sed s/.*release\ // < /etc/redhat-release | sed s/\ .*//)
    sed -i 's/#install-packages#/yum install -y iw wget awk msmtp msmtp-mta s-nail dhcpcd ifplugd sudo dnsmasq dialog wireless_tools polkit net-tools openssh/g' /root/up.sh
    echo "$DIST based update"

  elif [ -f /etc/SuSE-release ] ; then
    DIST=$(tr "\n" ' ' < /etc/SuSE-release | sed s/VERSION.*//)
    REV=$(tr "\n" ' ' < /etc/SuSE-release| sed s/.*=\ //)

  elif [ -f /etc/mandrake-release ] ; then
    DIST='Mandrake'
    PSUEDONAME=$(sed s/.*\(// < /etc/mandrake-release | sed s/\)//)
    REV=$(sed s/.*release\ // < /etc/mandrake-release | sed s/\ .*//)

  elif [ -f /etc/debian_version ] ; then	
    if [ "$(awk -F= '/DISTRIB_ID/ {print $2}' /etc/lsb-release)" = "Ubuntu" ]; then
      DIST="Ubuntu"
      sed -i 's/#install-packages#/apt install -y iw wget awk msmtp msmtp-mta s-nail dhcpcd ifplugd sudo dnsmasq dialog wireless_tools polkit net-tools openssh/g' /root/up.sh
      echo "$DIST based update"
    else
      DIST="Debian $(cat /etc/debian_version)"
      REV=""
      sed -i 's/#install-packages#/apt install -y iw wget awk msmtp msmtp-mta s-nail dhcpcd ifplugd sudo dnsmasq dialog wireless_tools polkit net-tools openssh/g' /root/up.sh
      echo "$DIST based update"
    fi

  elif [ -f /etc/arch-release ] ; then
    DIST="Arch"
    sed -i 's/#install-packages#/pacman -S --noconfirm iw wget awk msmtp msmtp-mta s-nail dhcpcd ifplugd sudo dnsmasq dialog wireless_tools polkit net-tools openssh/g' /root/up.sh
    echo "$DIST based update"
fi

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

echo 'creating wireless-access-profile ################################################################################'

cat > /etc/netctl/wlan0-«wireless-AP-name» <<EOL
Description='Automatically Connect'
Interface=wlan0
Connection=wireless
Security=wpa
ESSID=«wireless-AP-name»
IP=dhcp
Key=«wifi-AP-password»
EOL


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

## step 4 >>> run setup.sh

```
chmod +x /root/setup.sh && sh /root/setup.sh
```
