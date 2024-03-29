---
title: 'Programms'
layout: null

---

This Section contains the most recommended programms to use on your OS

### Snap

```bash
yay -S snapd
sudo systemctl enable snapd.socket
sudo systemctl start snapd.socket
sudo ln -s /var/lib/snapd/snap /snap
sudo snap install snap-store
```

### FlatPak

```bash
sudo pacman -S flatpak

# integriert das Flathub Repositorium, das eine zentrale Sammelstelle für flatpaks darstellt.
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

******

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

#### Tor

```bash
sudo pacman -S torbrowser-launcher
```

**Alternative**

```bash
sudo pacman -S tor
```

*Nyx* is a command line monitor for Tor, it provides bandwidth usage, connection details and on-the-fly configuration editing

```bash
sudo pacman -S nyx
```

#### Torrent / Download-Manager

```bash
sudo pacman -S qbittorrent
yay -S jdownloader2
```

#### E-Mail

**Proton Mail Bridge**

```bash
sudo pacman -S --needed gnome-keyring  # Required Dependency, otherwise Login will fail
yay -S protonmail-bridge-bin
```

**Evolution - E-Mail Client**

```bash
sudo pacman -S evolution
```

******

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

**OBS Studio**

```bash
sudo pacman -S obs-studio
```

**KDE Video Editor - KDEnlive**

A simple video editor, but not that advanced compared to Davinci Resolve

```bash
yay -S kdenlive
```

or

**OpenShot**

```bash
sudo pacman -S openshot
```

**DaVinci Resolve**

[DaVinci Resolve - ArchWiki](https://wiki.archlinux.org/title/DaVinci_Resolve)

cat ~/.local/share/applications/com.blackmagicdesign.resolve.desktop `[Desktop Entry] Version=1.0 Type=Application Name=DaVinci Resolve GenericName=DaVinci Resolve Comment=Revolutionary new tools for editing, visual effects, color correction and professional audio post production, all in a single application! Path=/opt/resolve/ Exec=sh /home/norman/.local/bin/resolve %U Terminal=true MimeType=application/x-resolveproj; Icon=/opt/resolve/graphics/DV_Resolve.png StartupNotify=true Name[en_US]=DaVinci Resolve StartupWMClass=resolve`

cat ~/.local/bin/resolve `#!/bin/bash #Script to run Davinci Resolve in HiDPI QT_DEVICE_PIXEL_RATIO=2 QT_AUTO_SCREEN_SCALE_FACTOR=true /opt/resolve/bin/resolve`

*For Converting MP4 Videos (incompatible in free edition)* cat ~/.local/bin/resolve-convert.sh `#!/bin/bash echo "Converting 1 ..." ffmpeg -i 1 -c:v dnxhd -profile:v dnxhr_hq -pix_fmt yuv422p -c:a pcm_s16le -f mov $1.mov

**Video Converter**

```bash
sudo pacman -S handbrake
```

**Burning**

```bash
sudo pacman -S brasero  # Burning Software
yay -S balena-etcher  # Write IMAGES to USB-Drives
```

#### Music

```bash
sudo pacman -S audacity
```

#### Spotify

```bash
curl -sS https://download.spotify.com/debian/pubkey_5E3C45D7B312C643.gpg | gpg --import -
yay -S spotify
```

#### Guitar Pro 6

```bash
yay -S guitar-pro
```

#### Graphic

Blender

```bash
sudo pacman -S blender
```

Image Manipulation

```bash
sudo pacman -S gimp gimp-help-de
```

Lightroom Alternative

```bash
sudo pacman -S darktable
```

Krita

*Edit and paint images*

```bash
sudo pacman -S krita
```

yED Graph Editor

```bash
yay -S yed 
```

#### E-Books

```bash
sudo pacman -S calibre
```

*****

### Development

**Gnome Builder**

```bash
sudo pacman -S gnome-builder
```

**VS Code**

Open Source:

```bash
yay -S vscodium-bin
```

Alternative:

```bash
sudo pacman -S code  # vscode
```

**Bootstrap Studio**

```bash
yay -S bootstrap-studio
```

**Atom**

A Hackable Editor

```bash
snap install atom --classic
```

Install Haskel Syntax Support

```bash
apm install language-haskell ide-haskell ide-haskell-cabal
```

*Use Tab Key and Spaces* See Soft Tabs and Tab Length under Settings > Editor Settings.
To toggle indentation modes quickly you can use Ctrl-Shift-P and search for Editor: Toggle Soft Tabs.

*Spell Checking* https://cschnack.de/blog/2019/atom-spell-lang-swop/

**Git Tools**

```bash
yay -S github-desktop-bin
yay -S gitkraken
```

**Python Tools**

```bash
sudo pacman -S python-pip

# For Python Code Checking
pip3 install pylint
```

**iPython**

```bash
sudo pacman -S ipython
```

**Jetbrains Toolbox**

```bash
yay -S jetbrains-toolbox
```

**Haskell**

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

**Netbeans**

[Netbeans - ArchWiki](https://wiki.archlinux.org/title/Netbeans)

*****

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

*****

### Office

#### Zotero

```bash
yay -S zotero-bin
```

#### PDF

Recommendation: xreader

```bash
sudo pacman -S xreader
```

[Alternatives](https://wiki.archlinux.org/index.php/PDF,_PS_and_DjVu)

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

*****

### VirtualBox

Follow the instructions on the following [Site](https://wiki.archlinux.org/title/VirtualBox).
German: [VirtualBox – wiki.archlinux.de](https://wiki.archlinux.de/title/VirtualBox)

IMPORTANT
Arch module for linux and dkms for linux-lts.
Use modprobe vboxdrv.

```bash
sudo pacman -S virtualbox virtualbox-host-dkms linux-lts-headers 

sudo modprobe vboxdrv

sudo nano /etc/modules-load.d/vboxdrv.conf
# Add vboxdrv
yay -S virtualbox-ext-oracle
```

### Emulation

#### Bottles

```bash
flatpak install flathub com.usebottles.bottles
```

*****

### Communication

#### ZOOM Client

```bash
yay -S zoom
```

#### Telegram

```bash
sudo pacman -S telegram-desktop 
```

#### Discord

```bash
sudo pacman -S discord
```

#### Signal

```bash
flatpak install flathub org.signal.Signal
```

#### Rambox

```bash
# Instant Messaginger
yay -S rambox 
```

*****

### MarkText

Better MD Editor

```bash
flatpak install marktext
```

*****

### Pomotroid - Graphical Pomodoro Timer

```bash
snap install pomotroid
```

*****

### 1Password

[Official Documentation](https://support.1password.com/getting-started-linux/#arch-linux)

```bash
 curl -sS https://downloads.1password.com/linux/keys/1password.asc | gpg --import
 yay -S 1password
```

### Kali Linux Shell

If you want a more "Kali" like shell, feel free to install [this](https://wiki.archlinux.org/index.php/zsh) shell.

### Ubuntu System Optimizer - Stacer

```bash
yay -S stacer
```

### The Package Manager of Arch Linux

[Link](https://wiki.archlinux.de/title/Graphische_Paketmanager)

**Pamac**

A Package Manager for Arch Linux Pacman Packages

```bash
sudo pacman -S appstream-glib
yay -S archlinux-appstream-data-pamac

yay -S snapd-glib

git clone https://aur.archlinux.org/libpamac-aur.git
cd libpamac
nano PKGBUILD  # Edit PKGBUILD
# SET
ENABLE_FLATPAK=1
ENABLE_SNAPD=1

makepkg -si


yay -S pamac-aur
```

### No Adobe - Use the Libre Graphics Suite

[Follow this link](https://github.com/AppImage/AppImageKit/wiki/Libre-Graphics-Suite)

### VeraCrypt

Multi-Platform encryption and decryption tool

```bash
sudo pacman -S veracrypt
```

#### Install Awesome Nautilus Extensions

```bash
sudo pacman -S nautilus-terminal
```

### Reverse Engineering

#### IDA

**Requirement**

```bash
git clone https://github.com/WqyJh/qwingraph_qt5.git
cd qwingraph_qt5
sudo ./install.sh
```

```bash
yay -S ida-free
```

#### Ghidra

```bash
yay -S ghidra
```

#### EDB - Debugger

```bash
yay -S edb-debugger-git
```

#### Maltego

```bash
yay -S maltego

python3 -m venv ~/Dokumente/venv/mcti
# Create VENV and install maltego-trx and requests
# TODO Copy Transforms and Inport Configs auto inport
```

### Penetration Testing

#### Burp suite

Burp Suite is an integrated platform for performing security testing of web applications. Its various tools work seamlessly together to support the entire testing process, from initial mapping and analysis of an application's attack surface, to finding and exploiting security vulnerabilities.

This will install Burp Suite Community (free edition):

```bash
yay -S burpsuite
```

#### nmap

```bash
sudo pacman -S nmap
```

#### Wireshark

```bash
sudo pacman -S wireshark-qt
```

#### Hashcat

```bash
sudo pacman -S hashcat
```

*****

### .RST Reader

**Restview**

```bash
sudo pacman --needed -S python-virtualenv

mkdir -p ~/venv/restview
cd ~/venv/restview
virtualenv venv # The second system I used just used 'virtualenv venv'
./venv/bin/pip install restview
./venv/bin/restview ~/path/MANUAL.rst
```

****

### Disk Cleaning Program

*BleachBit*

```bash
sudo pacman -S bleachbit 
```

*Filelight* #GUI Tool

```bash
sudo pacman -S filelight 
```

*Baobab* #Gnome Tool

```bash
sudo pacman -S baobab 
```

****

### Zim

Knowledge Database [Zim - ArchWiki](https://wiki.archlinux.org/title/Zim)

****

### Game Development

#### Unreal Engine 4

[Unreal Engine 4 - ArchWiki](https://wiki.archlinux.org/title/Unreal_Engine_4)

#### Epic-Asset-Manager

[GitHub - AchetaGames/Epic-Asset-Manager: A frontend to Assets purchased on Epic Games Store](https://github.com/AchetaGames/Epic-Asset-Manager)

****

### Gaming

**Steam**

```bash
sudo pacman -S steam steam-native-runtime
```

**Minecraft**

```bash
yay -S minecraft-launcher
yay -S multimc-bin  # MC Mod Manage Launcher
```

[Gaming - ArchWiki](https://wiki.archlinux.org/title/gaming)

**Lutris**

```bash
sudo pacman -S lutris
```

[itch.io](https://en.wikipedia.org/wiki/itch.io) — Indie game store. [https://itch.io/](https://itch.io/) || [itch](https://aur.archlinux.org/packages/itch/)AUR

```bash
yay -S itch-bin
```

Legendary — A free and open-source replacement for the Epic Games Launcher. [GitHub - derrod/legendary: Legendary - A free and open-source replacement for the Epic Games Launcher](https://github.com/derrod/legendary) || [legendary](https://aur.archlinux.org/packages/legendary/)AUR

Heroic Games Launcher — A GUI for legendary, an open-source alternative for the Epic Games Launcher. [GitHub - Heroic-Games-Launcher/HeroicGamesLauncher: A Native GOG and Epic Games Launcher for Linux, Windows and Mac.](https://github.com/Heroic-Games-Launcher/HeroicGamesLauncher) || [heroic-games-launcher-bin](https://aur.archlinux.org/packages/heroic-games-launcher-bin/)AUR

****

### Synology

```bash
yay -S synology-drive
yay -S synology-note-station
```

****

### Advanced Power Management

```bash
sudo pacman -S tlp tlp-rdw
```

****

### ZSH

```bash
sudo pacman -S zsh zsh-syntax-highlighting zsh-autosuggestions zsh-theme-powerlevel10k
```

Extra

```bash
pamac install pamac-zsh-completions
```