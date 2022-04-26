---
title: 'Post Installation'
category: Installation
layout: null
---

### Updates
```bash
# Install Contributed scripts and tools for pacman systems
pacman -S pacman-contrib

# Update System
pacman -Syu

# Clean orphan
pacman -Rns $(pacman -Qqtd)

# Clean Cache
pacman -Sc

# Edit pacman.conf
nano /etc/pacman.conf
# Uncomment Color
# Add ILoveCandy
# uncomment multilib
pacman -Sy

### Optional - Segnature Security ###
pacman -S archlinux-keyring  # Update keyring
pacman-key --refresh-keys  # Refresh pacman keys
```
### Add Accounts
```bash
useradd -m -g users -s /bin/bash <name>
passwd <name>
EDITOR=nano visudo
--> Uncomment: # %wheel ALL=(ALL) ALL
gpasswd -a <user> wheel  # add user to wheel group
gpasswd -a <user> audio,video,games,power,optical
```

### YAY
#### Install YAY
Change to User - This is required, because yay can't run as root because it uses the fake root to install packages.

If you prefer to stay as root, feel free to replace this instead of an yay command (1. Method)
```bash
# the command yay -S <name>
git clone https://aur.archlinux.org/<name>.git
cd <name>
makepkg -si
```
If you prefer using yay as root, use the following command instead (2. Method)
```bash
# the command yay -S <name>
sudo -u <username-that-is-not-root> yay -S <name>
```

#### Installation (if you want to use yay)
```bash
su <uname>
```
```bash
# Install YAY - simple AUR Package Installation
pacman -S git
git clone https://aur.archlinux.org/yay.git
cd yay/
makepkg -si PKGBUILD
# Alternativ Package Installer: trizen, aurman, yaourt (End of life)
```
If you are using root to install this packages, run this commands as they are. If you use an other user to install the packages, use the prefix ```sudo ```

### Console

#### Generic
```
pacman -S usbutils dialog powertop
# optional #
# lsof  # Lists open files for running Unix processes
# dmidecode  # Hardware Infos
# nmon  # Systen Monitor
# mc  # Dual Pane File Explorer
# neofetch  # system information tool
# fwupd  # Firmware upgrade
# gpm  # Console Mouse Support
# liveroot  # (AUR) root overlay fs
```

#### Compression Tools
```
pacman -S zip unzip unrar p7zip lzop
```

#### Network Tools
```
pacman -S rsync traceroute bind net-tools
# optional #
# dnsutils  # DNS tools (nslookup)
# nmap  # Network Scanner
# netdiscover  # (AUR) Network Scanner with MAC vendors
# speedtest-cli  # SpeedTest
# arp-scan  # ARP SCANNER
# wavemon  # WIFI monitor
# dsniff  # tools for network auditing and penetration
# mitmproxy  # SSL-capable MITM HTTP proxy
# sslstrip  # tool to hijack HTTPS in MITM attack
```

### System
#### Services
```
pacman -S networkmanager openssh xdg-user-dirs intel/amd-ucode (bluez bluez-utils) pkgstats
pacman -S bluez bluez-utils  # For Bluetooth compatibles
# optional #
# cronie  # Cron tasks server
# numlockon  # Numlock on on tty
# net-snmp  # SNMP Server
# samba  # Server SMB
# syslog-ng
# ntp
# haveged
```

#### Filesystem
```
pacman -S os-prober dosfstools ntfs-3g gvfs
# optional #
# snapper  # snapshot manager (ext4, lvm, btrfs)
# btrfs-progs  # BTRFS ffile utils
# exfat-utils  # EXFAT file support
# gptfdisk
# autofs
# fuse2
# fuse3
# fuseiso
# sshfs  # ssh client
# cifs-utils  # SMB mound command
# smbclient  # SMB full client
# nfs-utils
# open-iscsi
# glusterfs
# hfsprogs (AUR)
# mtpfs
# unionfs-fuse
# nilfs-utils
# s3fs-fuse
```

#### Sound
```
pacman -S alsa-plugin alsa-utils pulseaudio pulseaudio-alsa 
pacman -S pulseaudio-bluetooth  # Bluetooth Support
pacman -S pulseaudio-equalizer  # Optional
```

#### Printer
```
pacman -S cups ghostscript cups-pdf hplip
# optional #
# gutenprint  # Top quality printer drivers for POSIX systems
# foomatic-db foomatic-db-*  # Foomatic - The collected knowledge about printers, drivers, and driver options in XML files, used by foomatic-db-engine to generate PPD files.
```

#### Fonts 
```
pacman -S ttf-bitstream-vera ttf-dejavu ttf-liberation adobe-source-sans-pro-fonts
# optional #
# font-bh-ttf
# gsfonts
# sdl_ttf
# xorg-fonts-type1
pacman -S ttf-anonymous-pro ttf-droid   ttf-ubuntu-font-family ttf-roboto ttf-roboto-mono ttf-font-awesome
yay -S ttf-hackgen ttf-gentium-basic ttf-ms-fonts
pacman -S ttf-fira-code
# For More Fonts: pacman -Ss ttf
```

#### input drivers
```
pacman -S xf86-input-synaptics  # optional: Laptop Touchpad-Driver
# optional #
# xf86-input-libinput  #  Generic input driver for the X.Org server based on libinput
# xf86-input-elographics
# xf86-input-evdev
# xf86-input-vmmouse  # (VMWare)
# xf86-input-void
# xf86-input-wacom
# other: Saitek-*, Madcatz-*
```
