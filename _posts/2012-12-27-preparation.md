---
title: 'Preparation'
layout: null
---

### Run the Arch Linux Disc
Burn and Run the ArchLinux ISO until the command prompt appears.

### Set up Base
```bash
loadkeys <language>  # e.g. de or de-latin1
```

### Internet Connection
```bash
ip a # check internet connection (alternative: ip link)
```

#### On Wifi
```bash
# newer way:
[iwctl](https://wiki.archlinux.org/index.php/Iwctl)
```bash
iwctl
# Use help for support
device list  # list network devices
station <device> scan  # scan for networks
station <device> get-networks  # get all networks
station <device> connect SSID  # login into your wifi
```

##### old solution
wpa_passphrase  SSID  Passwort  > /etc/wpa_supplicant/wpa_supplicant.conf
wpa_supplicant -i wlp0s1 -D wext -c /etc/wpa_supplicant/wpa_supplicant.conf -B
dhcpcd wlp0s1
```

#### On LAN
```bash
dhcpcd <ethernet-adapter>
```

### Update Database
```bash
pacman -Syyy
```
