---
title: 'Other Programms'
layout: null
---

### Office

#### Suits
```
pacman -S libreoffice-still libreoffice-still-de # stable version (newer features in "fresh")
# alt. (aur): freeoffice/openoffice
```
For extra image-grid support, add the (two available) Grid buttons to your Titlebar of LibreOffice Writer

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

### Remarkable
Better MD Editor
```bash
yay -S remarkable
```

### Flameshot
Screenshot Tool
```bash
sudo pacman -S flameshot 
```
Use "Flameshot gui" to setup Shortcut


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

### Atom
A Hackable Editor
```bash
sudo pacman -S atom 
```

### Pomotroid - Graphical Pomodoro Timer
```bash
yay -S pomotroid-bin
```
This version my be a little buggy. If so, try the SNAP Version
```bash
# Install SNAP
git clone https://aur.archlinux.org/snapd.git
cd snapd
makepkg -si

sudo systemctl enable --now snapd.socket

sudo ln -s /var/lib/snapd/snap /snap

sudo snap install pomotroid
```

### 1Password
[Official Documentation](https://support.1password.com/getting-started-linux/#arch-linux)

### Additional PDF Software
Recommendation: xreader
```bash
sudo pacman -S xreader
```

[Alternatives](https://wiki.archlinux.org/index.php/PDF,_PS_and_DjVu)

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

### DaVinci Resolve
https://wiki.archlinux.org/title/DaVinci_Resolve

cat ~/.local/share/applications/com.blackmagicdesign.resolve.desktop 
`
[Desktop Entry]
Version=1.0
Type=Application
Name=DaVinci Resolve
GenericName=DaVinci Resolve
Comment=Revolutionary new tools for editing, visual effects, color correction and professional audio post production, all in a single application!
Path=/opt/resolve/
Exec=sh /home/norman/.local/bin/resolve %U
Terminal=true
MimeType=application/x-resolveproj;
Icon=/opt/resolve/graphics/DV_Resolve.png
StartupNotify=true
Name[en_US]=DaVinci Resolve
StartupWMClass=resolve
`

cat ~/.local/bin/resolve
`
#!/bin/bash
#Script to run Davinci Resolve in HiDPI
QT_DEVICE_PIXEL_RATIO=2 QT_AUTO_SCREEN_SCALE_FACTOR=true /opt/resolve/bin/resolve
`

**For Converting MP4 Videos (incompatible in free edition)**
cat ~/.local/bin/resolve-convert.sh 
`
#!/bin/bash
echo "Converting $1 ..."
ffmpeg -i $1 -c:v dnxhd -profile:v dnxhr_hq -pix_fmt yuv422p -c:a pcm_s16le -f mov $1.mov
`

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
To Fix the Graph Generation:
```
git clone https://github.com/WqyJh/qwingraph_qt5 & cd qwingraph_qt5 & sudo ./install.sh
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

### Zim
Knowledge Database
[https://wiki.archlinux.org/title/Zim](https://wiki.archlinux.org/title/Zim)

### Epic-Asset-Manager
[https://github.com/AchetaGames/Epic-Asset-Manager](https://github.com/AchetaGames/Epic-Asset-Manager)

### Gaming
https://wiki.archlinux.org/title/gaming

[itch.io](https://en.wikipedia.org/wiki/itch.io) — Indie game store.
https://itch.io/ || [itch](https://aur.archlinux.org/packages/itch/)AUR

Legendary — A free and open-source replacement for the Epic Games Launcher.
https://github.com/derrod/legendary || [legendary](https://aur.archlinux.org/packages/legendary/)AUR

Heroic Games Launcher — A GUI for legendary, an open-source alternative for the Epic Games Launcher.
https://github.com/Heroic-Games-Launcher/HeroicGamesLauncher || [heroic-games-launcher-bin](https://aur.archlinux.org/packages/heroic-games-launcher-bin/)AUR

```javascript
function sayHello(name) {
  if (!name) {
    console.log('Hello World');
  } else {
    console.log(`Hello ${name}`);
  }
}
```