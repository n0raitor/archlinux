---
title: 'After Reboot'
layout: null
---

### Exit and Reboot

```bash
exit
umount -a
reboot
```

### Connect to Internet

#### With Wifi

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

#### With Ethernet

```bash
ip a # search for Ethernet adapter
dhcpcd <adapter>
```

### Use Reflector as above

```bash
reflector --verbose --country 'Germany' -l 200 -p https --sort rate --save /etc/pacman.d/mirrorlist
```

### Update System

```bash
pacman -Syu
```

###### Generate SSH-Key

```bash
ssh-keygen -t rsa -b 4096
```

### Main Maintenance

**SSH**

```bash
# sudo systemctl enable sshd
# sudo systemctl start sshd
# -> Already done, if not, run this commands
```

**OpenVPN**

```bash
# Setup OpenVPN
sudo pacman -S openvpn networkmanager-openvpn
systemctl restart networkmanager
# import openvpn config with:
openvpn import --config <config-incl-path>
nmcli connection import type openvpn file <file-to-ovpn>
```

**Misc**

```bash
sudo pacman -S ffmpegthumbnailer  # Lightweight video thumbnailer that can be used by file managers.
sudo pacman -S raw-thumbnailer  # A lightweight and fast raw image thumbnailer that can be used by file managers.

sudo pacman -S xarchiver file-roller ark archlinux-wallpaper xwallpaper archivetools archlinux-menu archlinux-themes-slim archlinux-xdg-menu fastjar
```

**Theme and Fonts - For Kali Look**
If not already done in XFCE

```bash
sudo pacman -S arc-gtk-theme arc-icon-theme
yay -S flat-remix cantarel-fonts
# sudo pacman -S futurist - Deprevated
```

**Setup Useful Shortcuts**

- Super + T: Terminal
- Super + C: Chrome
- Super + F: File Manager
