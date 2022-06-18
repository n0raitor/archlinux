---
title: 'Music Production'
layout: null
---

# Arch Linux - Music Production - Installation Guide

## Pre Requirements

- Follow the Installation Guide on [the Website](https://normannator.de/archlinux) and choose your way into Arch.
- Package Requirements
  - Timeshift
  - YAY
  - octopi (if you prefer a Frontend for Pacman)

This Setup Process was tested with Manjaro KDE. (Pulse Audio included)

## Installation Guide

### JACK Audio Server

Hold all audio work together. Make sure Version 2 is installed (has more features, do not use Version 1!).

```bash
sudo pacman -S jack2
```

### Cadence JACK toolbox

Control Panel for Production Audio work

```bash
sudo pacman -S cadence
```

Open Cadence and Coose in the Settings:

- Driver -> ALSA -> Select Interface
- Engine -> Check Realtime
- (evtl. Realitme Priority: 95)
  Do not try to Start the JACK Server now, we do that later

Feel free to experiment with some of the shipped internal tools of *Cadence* later ;)

### Configuring User Permissions (the audio group)

We make sure now, to get the user in the group audio get *real-time scheduling* (the permission to get more CPU time)
This steps are explained in more details [here](https://jackaudio.org/faq/linux_rt_config.html)

```bash
groups #show groups

cd /etc/security/
sudo nano limits.conf
Ctrl+W # Search: audio
# -> If not found, follow on
# Go to end of the file
```

Insert this:

```bash
# audio group
@audio        -        rtprio        95
@audio        -        memlock        unlimited
```

Save with: Ctrl+O, Enter, Ctrl+W, Enter

Now log out and log back in to make the changes take affect.

Now Start the Cadence Server - Everything should work

### Ardour - DAW

```bash
sudo pacman -S ardour

# Optional
sudo pacman -S xjadeo  # A simple video player that is synchronized to jack (work with film files on music production base)
sudo pacman -S harvid  # HTTP Ardour Video Daemon
```

**Now Start Ardour**
On Audio/MIDI Setup, choose: Audio System -> JACK + Connect to JACK

If an Error appears, make sure JACK has the right audio group privileges.

Change Font Size: Preferences -> Appearance -> GUI and Font Scaling

Maybe also take a look at [PW](https://pipewire.org/)

### Maybe install RealTime Kernel

Linux Real Time Kernel might increase Performance but it is not necessary.
In Manjaro, the Kernel Menu make it easy to install the RT Kernal of Linux
If you do so, reboot your Machine.

```bash
uname -a  # Check if the installation was successful
```

Make sure to test the RT Kernal first, before removing the other on!

### Install more Audio Software

If you want to install lots of Plugins / VST in a row, use the *pro-audio* group for your installation (Nice overview in *Octopi*)

#### Zyn-Fusion (new user interface to ZynAddSubFX 3)

```bash
yay -S zyn-fusion
```

In Ardour: Press Shift + E to open the Editor Mixer

Add Plugins: Preferences -> Plugins -> Scan for Plugins

Testing:

- CTRL + Shift + N: Create Midi Track
- Select as Instrument: ZynAddSubFX
- (Name should be one String with no White spaces, I guess...)
- Create
  - If Not Visible in Editor Mixer, click on Fader (in the Editor Mixer) -> New Plugin -> Plugin Manager

NOTE: Test all Plugins / VST your will install below

#### Surge

```bash
yay -S surge-synthesizer-bin
```

#### Calf

```bash
sudo pacman -S calf
```

#### LSP

```bash
sudo pacman -S lsp-plugins
```

#### Tap

```bash
sudo pacman -S tap-plugins
```

#### More Plugins (form the pro-audio Group -> Pacman)

- audacity
- calf
- caps
- carla
- dpf-plugins
- dragonfly-reverb
- drumgizmo
- ebumeter
- eq10q
- geonkick
- guitarix
- helm
- ir.lv2
- liquidsfz
- lsp-plugins
- mda.lv2
- ninjas2
- noise-repellent
- samplev1
- setbfree
- sherlock.lv2
- sonic-visualiser
- swh-plugins
- tap-plugins
- wolf-shaper
- wolf-spectrum
- x42-plugins
- zam-plugins
- zita-ajbridge
- zita-njbridge
- zita-rev1

#### Install Plugins from the AUR

- Bitrot-git

- invada-studio-plugins-lv2

- 

- 

- 

#### PulseAudio Jack Module

```bash
sudo pacman -S pulseaudio-jack
# or
yay -S vcvrack-bin  # -> ASLA to JACK, Sound to System-Sound
```
