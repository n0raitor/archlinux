---
title: 'Post Installation'
category: Installation
layout: null
---

### Updates

```bash
# Update System
pacman -Syu

nano /etc/pacman.conf
# Uncomment Color
# Uncomment ParallelDownloads
# Add ILoveCandy (after ParallelDownloads)
# uncomment multilib Repo (2 Lines)

pacman -Sy

### Segnature Security ###
pacman -S archlinux-keyring  # Update keyring
pacman-key --refresh-keys  # Refresh pacman keys

# If it fails (lots of red lines at once): Make sure your System Time is right
```

### Add your Accounts

```bash
useradd -m -g users -s /bin/bash <username>
passwd <name>
EDITOR=nano visudo
--> Uncomment: # %wheel ALL=(ALL) ALL
gpasswd -a <username> wheel  # add user to wheel group
gpasswd -a <username> audio
gpasswd -a <username> video
gpasswd -a <username> games
gpasswd -a <username> power
gpasswd -a <username> optical
```

### YAY

#### Install YAY

Change to User - This is required, because yay can't run as root because it uses the fake root to install packages.

```bash
su <username>
```

```bash
# Install YAY - simple AUR Package Installation
pacman -S git
git clone https://aur.archlinux.org/yay.git
cd yay/
makepkg -si PKGBUILD
```

NOW STAY IN USER MODE to get the commands working

### Booting

#### Num Lock activation

[Activating numlock on bootup - ArchWiki](https://wiki.archlinux.org/title/Activating_numlock_on_bootup)

#### Microcode

*Note: Replace the intel with amd, if your cpu is an amd-cpu*

```bash
pacman -S intel-ucode
```

### Power management

#### ACPI Replacement

[Power management with Systemd - ArchWiki](https://wiki.archlinux.org/title/Power_management#Power_management_with_systemd)

#### Laptops

**Powertop** is a tool provided by Intel to enable various powersaving modes in userspace, kernel and hardware. It is possible to monitor processes and show which of them are utilizing the CPU and wake it from its Idle-States, allowing to identify applications with particular high power demands.

```bash
sudo pacman -S powertop
```

### Multimedia

#### Sound System

```bash
pacman -S alsa alsa-plugins alsa-utils pulseaudio pulseaudio-alsa 
pacman -S pulseaudio-bluetooth  # Bluetooth Support
pacman -S pulseaudio-equalizer  # Optional
```

### Networking

Configuration tools for Linux networking

```bash
pacman -S net-tools wget curl
```

#### Networmanager

```bash
sudo pacman -S networkmanager 
sudo systemctl enable NetworkManager
```

#### SSH

```bash
sudo pacman -S openssh
sudo systemctl enable sshd
```

#### Setting up a firewall

```bash
sudo pacman -S ufw gufw
sudo ufw enable
sudo ufw status verbose
sudo systemctl enable ufw.service
```

### Input devices

libinput is a dependency of Xorg and Wayland by default

### System services

**Printer**

```bash
sudo pacman -S cups cups-pdf
sudo systemctl enable cups
sudo systemctt disable systemd-resolved.service
sudo systemctl enable avahi-daemon
```

**Bluetooth**

```bash
pacman -S bluez bluez-utils

# Optional
sudo systemctl enable bluetooth
```

### Graphical User Interface (GUI)

#### Display server

**Xorg**

```bash
pacman -S xorg
```

#### Display drivers

**Proprietary**

```bash
sudo pacman -S nvidia nvidia-lts nvidia-settings
# optional #
# virtualbox-guest-utils  # (For Virtualbox)
# nvidia-dkms  # (For custom kernel)
# nvidia-390xx-dkms  # (AUR) (for custom kernel) -> for my older laptop
```

<u>Optional</u>: Use nvidia-xconfig for auti xorg server config and adjustments. Check on */etc/x11/xorg.conf* if everything went well.

#### Desktop environments

*In Chapter: GNOME*

#### Display manager

*In Chapter: GNOME->GDM*

#### XDG User Dirs

```bash
sudo pacman -S xdg-user-dirs
```

### Recommended System Packages

**USBUtils**

A collection of USB tools to query connected USB devices

```bash
sudo pacman -S usbutils 
```

**dialog**

A tool to display dialog boxes from shell scripts

```bash
sudo pacman -S dialog  
```

**Compression Tools**

```bash
sudo pacman -S zip unzip unrar p7zip lzop
```

**Filesystem**

```bash
pacman -S os-prober dosfstools ntfs-3g gvfs
```

### Must Have for next Step

**Timeshift**

A system restore utility for Linux

```bash
yay -S timeshift-bin
```
