---
title: 'Other Programms'
layout: null
---
**Redshift**
```bash
# GNOME has it's own
sudo pacman -S redshift
(sudo systemctl enable redshift)
```

**Install Timeshift - Backup Your Data!**
```bash
yay -S timeshift
```

**Brave**
Most secure Browser on earth...
```bash
yay -S brave-bin
```

### And More Programms (all optional)

**Kali-Undercover**
```bash
yay -S kali-undercover
alias ku=/usr/bin/kali-undercover

#### cmatrix is the opposite ;)
```

**Pycharm-Professional and co. (Jetbrains Toolbox)**
[Download Link](https://www.jetbrains.com/de-de/toolbox-app/)


**VirtualBox**
Follow the instructions on the following [Site](https://wiki.archlinux.org/title/VirtualBox).
German: https://wiki.archlinux.de/title/VirtualBox

IMPORTANT
Arch module for linux and dkms for linux-lts.
Use modprobe vboxdrv.

**Deprecated - Just read the link above!!!**
```bash
sudo pacman -S virtualbox-host-modules-arch virtualbox-guest-iso
sudo gpasswd -a <user> vboxusers
systemctl enable dkms
sudo nano /etc/modules-load.d/my-modules.conf
--> add: vboxdrv
reboot # optional
```

**Git Tools**
```bash
# Version Control
yay -S github-desktop-bin
yay -S gitkraken
```

**KVM Integration**
```bash
sudo pacman -S qemu virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat
```

**Python Tools**
```bash
sudo pacman -S python-pip

# For Python Code Checking
pip3 install pylint
```

**ZOOM Client**
```bash
# install Zoom Client
yay -S zoom
```

**Balena-Etcher**
```bash
# Write IMAGES to USB-Drives
yay -S balena-etcher
```

**Spotify**
```bash
# Music Streaming
yay -S spotify
```

**Slack**
```bash
# communication
yay -S slack-desktop
```

**Rambox**
```bash
# Instant Messaginger
yay -S rambox
```

**1Password**
[Official Documentation](https://support.1password.com/getting-started-linux/#arch-linux)

**Additional PDF Software**
[Link](https://wiki.archlinux.org/index.php/PDF,_PS_and_DjVu)

**Kali Linux Shell**
If you want a more "Kali" like shell, feel free to install [this](https://wiki.archlinux.org/index.php/zsh) shell.

**Ubuntu System Optimizer - Stacer**
```bash
yay -S stacer
```

**Ebook Reader **
```bash
yay -S calibre
```

**KDE Video Editor - KDEnlive**
A simple video editor, but not that advanced compared to Davinci Resolve
```bash
yay -S kdenlive
```

**The Package Manager of Arch Linux**
[Link](https://wiki.archlinux.de/title/Graphische_Paketmanager)

**VLC**
No Words needed
```bash
sudo pacman -S vlc
```

**NitroShare- File Transfer Software**
[NitroShare.net](https://nitroshare.net/)
Allows cross-platform file sharing
```bash
yay -S nitroshare
```

**Chromium**
No Words needed - no data send to google

**SimpleNote**
Take notes and sync them
[Follow the instructions](https://snapcraft.io/install/simplenote/arch)

**No Adobe - Use the Libre Graphics Suite**
[Follow this link](https://github.com/AppImage/AppImageKit/wiki/Libre-Graphics-Suite)

**Alternative Terminal: Terminator**
For More information click [here](https://www.youtube.com/watch?v=iaXQdyHRL8M&t=215s&ab_channel=ChrisTitusTech)

**Thunderbird**
```bash
sudo pacman -S thunderbird thunderbird-i18n-de
```

**Brave**
```bash
yay -S brave-bin
```

**Handbrake**
```bash
sudo pacman -S handbrake
```

**Brasero**
Burning Software
```bash
sudo pacman -S brasero
```

**JDownloader 2**
Download Manager for stable downloads of large files.
```bash
yay -S jdownloader2
```

**VeraCrypt**
Multi-Platform encryption and decryption tool
```bash
sudo pacman -Ss veracrypt
```

**Nautilus - Awesome File Manager**
```bash
sudo pacman -S nautilus

# Install Awesome Extensions
sudo pacman -S nautilus-terminal
yay -S nautilus-open-any-terminal
```

**Guitar Pro 6**
```bash
yay -S guitar-pro
```

**IDA**
```bash
yay -S ida-free
```

**Capstone**
```bash
Use Capstone to Deassemble Binary Files
```

**yED Graph Editor**
Try It ;-)

**Bildschirmaufnahmen mit Kazam**
```bash
yay -S kazam
```

**Nautilus - Awesome File Manager**
```bash

```

**Man Page - German Edition**
```bash
sudo pacman -S man-pages-de
alias man="LANG=de_DE.UTF-8 man"  # Write to ~/.bashrc or ~/.zshrc
```

#### Alternative
use *Filelight* #GUI Tool

#### oder
*Baobab*    #Gnome Tool

### Disk Cleaning Program
*BleachBit*

