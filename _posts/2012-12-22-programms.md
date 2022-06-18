---
title: 'Programms'
layout: null

---

This Section contains the most recommended programms to use on your OS

### Internet

#### WIFI

Tools allowing to manipulate the Wireless Extensions

```bash
sudo pacman -S wireless_tools 
```

#### Web Browser

Most secure Browser on earth...

```bash
yay -S brave-bin
```

#### Torrent / Download-Manager

```bash
sudo pacman -S qbittorrent
yay -S jdownloader2
```

#### E-Mail

```bash
sudo pacman -S evolution
```

### Multimedia

#### GStreamer - Streaming Videos

```bash
sudo pacman -S gst-plugins-base gst-plugins-good gst-plugins-ugly
```

#### Video Player

```bash
sudo pacman -S vlc
```

#### Video Tools

Video Converter

```bash
sudo pacman -S handbrake
```

Burning

```bash
sudo pacman -S brasero  # Burning Software
yay -S balena-etcher  # Write IMAGES to USB-Drives
```

#### Graphic

Picture Manipulation

```bash
sudo pacman -S gimp
```

#### E-Books

```bash
yay -S calibre
```

### Dev

```bash
sudo pacman -S code  # vscode
```

### System

```bash
sudo pacman -S gparted bleachbit
```

#### System Recovery

Auto Backup Data (Take Snapshot) when updating / syncing pacman packages

```bash
yay -S timeshift-autosnap
```

If the Daily/Weekly/Monthly Tasks do not work, try: 

```bash
sudo systemctl enable cronie.service
```

### Office

#### Suits

```bash
sudo pacman -S libreoffice-still libreoffice-still-de # stable version (newer features in "fresh")
```

For extra image-grid support, add the (two available) Grid buttons to your Titlebar of LibreOffice Writer

#### office language aids

```bash
sudo pacman -S aspell aspell-de aspell-en
sudo pacman -S hunspell hunspell-en_us hunspell-de mythes 
sudo pacman -S mythes mythes-en mythes-de 
sudo pacman -S languagetool enchant
yay -S libreoffice-extension-languagetool
sudo pacman -S libmythes
sudo pacman -S speech-dispatcher
# optional #
# hyphen  # Hyphenation rules
```

### Pycharm-Professional and co. (Jetbrains Toolbox)

[Download Link](https://www.jetbrains.com/de-de/toolbox-app/)

### VirtualBox

Follow the instructions on the following [Site](https://wiki.archlinux.org/title/VirtualBox).
German: [VirtualBox – wiki.archlinux.de](https://wiki.archlinux.de/title/VirtualBox)

IMPORTANT
Arch module for linux and dkms for linux-lts.
Use modprobe vboxdrv.

### Git Tools

```bash
yay -S github-desktop-bin
yay -S gitkraken
```

### Python Tools

```bash
sudo pacman -S python-pip

# For Python Code Checking
pip3 install pylint
```

### ZOOM Client

```bash
yay -S zoom
```

### Spotify

```bash
yay -S spotify
```

### Remarkable

Better MD Editor

```bash
flatpak install marktext
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

Install Haskel Syntax Support

```bash
apm install language-haskell ide-haskell ide-haskell-cabal
```

**Use Tab Key and Spaces** See Soft Tabs and Tab Length under Settings > Editor Settings.
To toggle indentation modes quickly you can use Ctrl-Shift-P and search for Editor: Toggle Soft Tabs.

**Spell Checking** https://cschnack.de/blog/2019/atom-spell-lang-swop/

### Pomotroid - Graphical Pomodoro Timer

```bash
yay -S pomotroid
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

### Haskell

```bash
pacman -S ghc
pacman -S cabal-install
pacman -S haskell-language-server
```

GHCup

```bash
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```

To start a simple repl, run:

```bash
ghci
```

To start a new haskell project in the current directory, run:

```bash
cabal init --interactive
```

To install other GHC versions and tools, run:

```bash
ghcup tui
```

If you are new to Haskell, check out [First steps - GHCup](https://www.haskell.org/ghcup/steps/)

### KDE Video Editor - KDEnlive

A simple video editor, but not that advanced compared to Davinci Resolve

```bash
yay -S kdenlive
```

### DaVinci Resolve

[DaVinci Resolve - ArchWiki](https://wiki.archlinux.org/title/DaVinci_Resolve)

cat ~/.local/share/applications/com.blackmagicdesign.resolve.desktop `[Desktop Entry] Version=1.0 Type=Application Name=DaVinci Resolve GenericName=DaVinci Resolve Comment=Revolutionary new tools for editing, visual effects, color correction and professional audio post production, all in a single application! Path=/opt/resolve/ Exec=sh /home/norman/.local/bin/resolve %U Terminal=true MimeType=application/x-resolveproj; Icon=/opt/resolve/graphics/DV_Resolve.png StartupNotify=true Name[en_US]=DaVinci Resolve StartupWMClass=resolve`

cat ~/.local/bin/resolve `#!/bin/bash #Script to run Davinci Resolve in HiDPI QT_DEVICE_PIXEL_RATIO=2 QT_AUTO_SCREEN_SCALE_FACTOR=true /opt/resolve/bin/resolve`

**For Converting MP4 Videos (incompatible in free edition)** cat ~/.local/bin/resolve-convert.sh `#!/bin/bash echo "Converting $1 ..." ffmpeg -i $1 -c:v dnxhd -profile:v dnxhr_hq -pix_fmt yuv422p -c:a pcm_s16le -f mov $1.mov`

### The Package Manager of Arch Linux

[Link](https://wiki.archlinux.de/title/Graphische_Paketmanager)

**Pamac**

A Package Manager for Arch Linux Pacman Packages

```bash
sudo pacman -S appstream-glib
yay -S archlinux-appstream-data-pamac
yay -S pamac-aur-git

yay -S libpamac-aur
```

### No Adobe - Use the Libre Graphics Suite

[Follow this link](https://github.com/AppImage/AppImageKit/wiki/Libre-Graphics-Suite)

### VeraCrypt

Multi-Platform encryption and decryption tool

```bash
sudo pacman -S veracrypt
```

#### Install Awesome Extensions

```bash
sudo pacman -S nautilus-terminal
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

### Netbeans

[Netbeans - ArchWiki](https://wiki.archlinux.org/title/Netbeans)

### .RST Reader

**Restview**

```bash
mkdir restview
cd restview/
virtualenv-2.7 venv # The second system I used just used 'virtualenv venv'
./venv/bin/pip install restview
./venv/bin/restview ~/path/MANUAL.rst
```

### yED Graph Editor

Try It ;-)

#### Alternative

use *Filelight* #GUI Tool

#### oder

*Baobab* #Gnome Tool

### Disk Cleaning Program

*BleachBit*

### Zim

Knowledge Database [Zim - ArchWiki](https://wiki.archlinux.org/title/Zim)

### Epic-Asset-Manager

[GitHub - AchetaGames/Epic-Asset-Manager: A frontend to Assets purchased on Epic Games Store](https://github.com/AchetaGames/Epic-Asset-Manager)

### Gaming

[Gaming - ArchWiki](https://wiki.archlinux.org/title/gaming)

[itch.io](https://en.wikipedia.org/wiki/itch.io) — Indie game store. [https://itch.io/](https://itch.io/) || [itch](https://aur.archlinux.org/packages/itch/)AUR

Legendary — A free and open-source replacement for the Epic Games Launcher. [GitHub - derrod/legendary: Legendary - A free and open-source replacement for the Epic Games Launcher](https://github.com/derrod/legendary) || [legendary](https://aur.archlinux.org/packages/legendary/)AUR

Heroic Games Launcher — A GUI for legendary, an open-source alternative for the Epic Games Launcher. [GitHub - Heroic-Games-Launcher/HeroicGamesLauncher: A Native GOG and Epic Games Launcher for Linux, Windows and Mac.](https://github.com/Heroic-Games-Launcher/HeroicGamesLauncher) || [heroic-games-launcher-bin](https://aur.archlinux.org/packages/heroic-games-launcher-bin/)AUR