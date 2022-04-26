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

## Config - OS

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

##### old solution
wpa_passphrase  SSID  Passwort  > /etc/wpa_supplicant/wpa_supplicant.conf
wpa_supplicant -i wlp0s1 -D wext -c /etc/wpa_supplicant/wpa_supplicant.conf -B
dhcpcd wlp0s1
```
```bash
# Alternative
ip a # search Wifi Adapter
nmtui
-> Activate Connection->...
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

### (optional) Encrypt Home Partition
**Notice! This only makes sense, if your home partiton is on an other drive than your root partition or you did not encrypt your root partition**
```bash
# !!! Backup all your Data and copy the backup to another drive or to the root drive  before this step !!!
# Logged out.
# Switch to a console with Ctrl+Alt+F2.
# Login as a root and check that your user own no processes:
ps -U username

umount /home
lsblk  # to check if umount was successful

gdisk /dev/<home-device>  # Type as LUKS
cryptsetup luksFormat /dev/<home-device>
cryptsetup open /dev/<home-device> crypt_home
mkfs.ext4 /dev/mapper/crypt_home
mount /dev/mapper/crypt_home /home

# Now copy your Backups to the Home directory (sudo recommended)

# Change files to the right rights (important, if you backup and restore your files as root or with sudo)
cd /home/
chown -R <user> <user>/
chgrp -R users <user>/

lsblk -f  # Note down the UUID of your device
# <device>
# - <device-partition>
# - - <crypto 2>  UUID

nano /etc/crypttab  # Edit this file and insert the following row
# crypthome     UUID=<UUID enter here>    none                    luks,timeout=180

cp /etc/fstab /etc/fstab.bak  # Backup Your FSTAB File
nano /etc/fstab  # Edit this file
### File content ###
# <file system> <dir> <type> <options> <dump> <pass>

# /dev/mapper/vg1-root
UUID=...				/		ext4	rw,relatime     0 1

# /dev/<home-device>
/dev/mapper/crypthome	/home	ext4	rw,relatime     0 2

# /dev/sdb128
UUID=...				/boot	vfat	rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,utf8,errors=remount-ro   0 2

# /dev/mapper/vg1-swap
UUID=...				none	swap	defaults        0 0

## alternatively
genfstab -U


### If Reboot does not work, try to rebuild the grub cfg.
# Press Ctrl + Alt + F2
cd /boot/grub
mv grub.cfg grub.cfg.bak
grub-mkconfig -o /boot/grub/grub.cfg
reboot

### If Startup works ... Continue

```bash
# Reboot and make sure that you can login to your desktop
# If everything workes fine, feel free to remove your Backup (done before)
```


### Generate SSH-Key
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
yay -S flat-remix cantatel-fonts
# sudo pacman -S futurist - Deprevated
```

**Setup Useful Shortcuts**
- Super + T: Terminal
- Super + C: Chrome
- Super + F: File Manager
