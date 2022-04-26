---
title: 'Programms'
layout: null
---

#### Display Manager
I choosed lightdm-gtk-greeter
Alternatives:
* gdm
* sddm
* lxdm
-> See Your Desktop Environment Conditions

#### Applications

##### Office
```bash
### Suits
pacman -S libreoffice-still  # stable version (newer features in "fresh")
# alt. (aur): freeoffice/openoffice

### office language aids
pacman -S hunspell-en hunspell-de mythes-en mythes-de aspell-en aspell-de languagetool enchant
yay -S libreoffice-extension-languagetool
pacman -S libmythes
# optional #
# hyphen  # Hyphenation rules

### Tools
# galculator  # GTK+ based scientific calculator
# qalculate-gtk  #  GTK frontend for libqalculate
```

##### Internet
```bash
### Web Browser
pacman -S chromium firefox midori vivaldi vivaldi-ffmpeg-codecs  # midori is a Light Browser
# optional #
# pepper-flash, firefox-i18n, flashplugin, freshplaerplugin (aur), opera, opera-ffmpeg-codecs, seamonkey, seamonkey-i18n (aur), falkon

### Torrent
pacman -S qbittorrent
# optional #
# transmission-gtk/qt
# ktorrent
# deluge
# tixati

### E-Mail
pacman -S thunderbird
# optional #
# evolution + evolution-*
```

##### Multimedia
```bash
### GStreamer - Streaming Videos
pacman -S gst-plugins-base gst-plugins-good gst-plugins-ugly
# gst-plugins-libav - deprecated

### Audio Player
# Optional #
# clementine  # (QT) Audio Player
# mixx  # (QT) Audio Player
# quodlibet  # (GTK) Audio Player
# elisa  # (QT) Audio Player
# amarok  # (AUR) (QT) Audio Player
# guayadeque  # (AUR) (GTK) Audio Player
# gmusicbrowser  # (AUR) (GTK) Audio Player

### Video Player
pacman -S celluloid
# optional #
# vlc  # (QT) Video Player
# smplayer  # (QT) Video Player
# smtube  # (QT) Youtube Player
# mpv  # (GTK) Recommended for smplayer

### Video Tools
# optional #
# avidemux-qt  # (QT) Video Editor
# simplescreenrecorder  # (QT) Screen Recorder

### Burner
# optional #
# k3b (QT)
# xfburn (GTK)
# brasero (GTK)
# gnomebaker (AUR)
```

##### Graphic
```bash
pacman -S gimp
# optional #
# inkscape (GTK)
# dia (GTK)
# krita (QT)
```

##### Dev
```bash
pacman -S code  # vscode
# optional #
# notepadqq (QT)
# geany (GTK)
# geany-plugins (GTK)
# kdevelop (QT)
# gource (Git code animation)
```

##### System
```bash
pacman -S gparted bleachbit virtualbox
yay -S virtualbox-ext-oracle  # (AUR) Ext Pack
# optional #
# keepassxc  # (QT) Password Manager
# keepass  # (MONO) Password Manager
# wine + wine_gecko + wine-mono # MS Windows App Support
# zenity + winetricks  # Recommended for Wine
# q4wine
# gdmap
# k4dirstat (aur)
# conky  # [Link to Tutorial Video](https://www.youtube.com/watch?v=QB8cjKpdVQY&ab_channel=ChrisTitusTech)

### Pacman GUI
# optional #
# octopo  # (AUR) (QT)
# pamac-aur  # (AUR) (GTK)
```
