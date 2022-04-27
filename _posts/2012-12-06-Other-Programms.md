---
title: 'Other Programms'
layout: null
---

### Office

#### Suits
```
pacman -S libreoffice-still  # stable version (newer features in "fresh")
# alt. (aur): freeoffice/openoffice
```

#### office language aids
```
pacman -S hunspell hunspell-en_us hunspell-en_gb hunspell-de mythes mythes-en mythes-de aspell aspell-en aspell-de languagetool enchant
yay -S libreoffice-extension-languagetool
pacman -S libmythes
# optional #
# hyphen  # Hyphenation rules
```

#### Tools
```
# galculator  # GTK+ based scientific calculator
# qalculate-gtk  #  GTK frontend for libqalculate
```

### Redshift
```bash
# GNOME has it's own
sudo pacman -S redshift
(sudo systemctl enable redshift)
```

### Kali-Undercover
```bash
yay -S kali-undercover
alias ku=/usr/bin/kali-undercover

#### cmatrix is the opposite ;)
```

### Pycharm-Professional and co. (Jetbrains Toolbox)
[Download Link](https://www.jetbrains.com/de-de/toolbox-app/)


### VirtualBox
Follow the instructions on the following [Site](https://wiki.archlinux.org/title/VirtualBox).
German: https://wiki.archlinux.de/title/VirtualBox

IMPORTANT
Arch module for linux and dkms for linux-lts.
Use modprobe vboxdrv.

### Git Tools
```bash
# Version Control
yay -S github-desktop-bin
yay -S gitkraken
```

### KVM Integration
```bash
sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat
```

### Python Tools
```bash
sudo pacman -S python-pip

# For Python Code Checking
pip3 install pylint
```

### ZOOM Client
```bash
# install Zoom Client
yay -S zoom
```

### Spotify
```bash
# Music Streaming
yay -S spotify
```

### Slack
```bash
# communication
yay -S slack-desktop
```

### Rambox
```bash
# Instant Messaginger
yay -S rambox
```

### 1Password
[Official Documentation](https://support.1password.com/getting-started-linux/#arch-linux)

### Additional PDF Software
[Link](https://wiki.archlinux.org/index.php/PDF,_PS_and_DjVu)

### Kali Linux Shell
If you want a more "Kali" like shell, feel free to install [this](https://wiki.archlinux.org/index.php/zsh) shell.

### Ubuntu System Optimizer - Stacer
```bash
yay -S stacer
```

### KDE Video Editor - KDEnlive
A simple video editor, but not that advanced compared to Davinci Resolve
```bash
yay -S kdenlive
```

### The Package Manager of Arch Linux
[Link](https://wiki.archlinux.de/title/Graphische_Paketmanager)


### NitroShare- File Transfer Software
[NitroShare.net](https://nitroshare.net/)
Allows cross-platform file sharing
```bash
yay -S nitroshare
```

### SimpleNote
Take notes and sync them
[Follow the instructions](https://snapcraft.io/install/simplenote/arch)

### No Adobe - Use the Libre Graphics Suite
[Follow this link](https://github.com/AppImage/AppImageKit/wiki/Libre-Graphics-Suite)

### Alternative Terminal: Terminator
For More information click [here](https://www.youtube.com/watch?v=iaXQdyHRL8M&t=215s&ab_channel=ChrisTitusTech)

### VeraCrypt
Multi-Platform encryption and decryption tool
```bash
sudo pacman -Ss veracrypt
```

### Nautilus - Awesome File Manager
```bash
sudo pacman -S nautilus
```
#### Install Awesome Extensions
```
sudo pacman -S nautilus-terminal
yay -S nautilus-open-any-terminal
```

### Guitar Pro 6
```bash
yay -S guitar-pro
```

### IDA
```bash
yay -S ida-free
```

### Ghidra
```bash
yay -S ghidra
```

### Capstone
```bash
Use Capstone to Deassemble Binary Files
```

### yED Graph Editor
Try It ;-)

### Bildschirmaufnahmen mit Kazam
```bash
yay -S kazam
```

#### Alternative
use *Filelight* #GUI Tool

#### oder
*Baobab*    #Gnome Tool

### Disk Cleaning Program
*BleachBit*

### Pamac
A Package Manager for Arch Linux Pacman Packages
```bash
sudo pacman -S appstream-glib
yay -S archlinux-appstream-data-pamac
yay -S pamac-aur-git

yay -S libpamac-aur
```