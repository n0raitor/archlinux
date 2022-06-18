---
title: 'After Reboot'
layout: null
---

### Use Reflector as above

```bash
reflector --verbose --country 'Germany' -l 200 -p https --sort rate --save /etc/pacman.d/mirrorlist
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
