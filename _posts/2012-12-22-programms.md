---
title: 'Programms'
layout: null
---
This Section contains the most recommended programms to use on your OS
### Internet

#### WIFI
```bash
pacman -S  wireless_tools wpa_supplicant  mtools 
```

#### Web Browser
```
pacman -S chromium 
yay -S brave-bin  # Most secure Browser on earth...
# firefox 
# midori 
# vivaldi vivaldi-ffmpeg-codecs 
# pepper-flash, firefox-i18n, flashplugin, freshplaerplugin (aur), opera, opera-ffmpeg-codecs, seamonkey, seamonkey-i18n (aur), falkon
```

#### Torrent / Download-Manager
```
pacman -S qbittorrent
yay -S jdownloader2
# optional #
# transmission-gtk/qt
# ktorrent
# deluge
# tixati
```

#### E-Mail
```
pacman -S thunderbird thunderbird-i18n-de
# optional #
# evolution + evolution-*
```

### Multimedia

#### GStreamer - Streaming Videos
```
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
```

#### Video Player
```
pacman -S vlc  # (QT) Video Player

# optional #
# celluloid
# smplayer  # (QT) Video Player
# smtube  # (QT) Youtube Player
# mpv  # (GTK) Recommended for smplayer
```

#### Video Tools
```
sudo pacman -S handbrake
# optional #
# avidemux-qt  # (QT) Video Editor
# simplescreenrecorder  # (QT) Screen Recorder
```

#### Burner
```
sudo pacman -S brasero  # Burning Software
yay -S balena-etcher  # Write IMAGES to USB-Drives


# optional #
# k3b (QT)
# xfburn (GTK)
# brasero (GTK)
# gnomebaker (AUR)
```

#### Graphic
```bash
pacman -S gimp
# optional #
# inkscape (GTK)
# dia (GTK)
# krita (QT)
```

#### E-Books
```bash
yay -S calibre
```

### Dev
```bash
pacman -S code  # vscode
# optional #
# notepadqq (QT)
# geany (GTK)
# geany-plugins (GTK)
# kdevelop (QT)
# gource (Git code animation)
```

### System
```bash
pacman -S gparted bleachbit
# optional #
# keepassxc  # (QT) Password Manager
# keepass  # (MONO) Password Manager
# wine + wine_gecko + wine-mono # MS Windows App Support
# zenity + winetricks  # Recommended for Wine
# q4wine
# gdmap
# k4dirstat (aur)
# conky  # [Link to Tutorial Video](https://www.youtube.com/watch?v=QB8cjKpdVQY&ab_channel=ChrisTitusTech)
```

#### System Recovery
```
yay -S timeshift
```
If the Daily/Weekly/Monthly Tasks do not work, try: 
```
systemctl status cronie.service
Start and enable evtl
```