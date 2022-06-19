---
title: 'GNOME and Appearance'
category: Desktop Environment
layout: null
---

### Gnome

```bash
sudo pacman -S gnome gdm gnome-shell-extensions gnome-tweaks
sudo systemctl enable gdm.service
```

(Optional) If you wish to use the Gnome-Software-Center instead of Pamac for GUI-Package-Management, feel free to install *gnome-software-packagekit-plugin* to get access to the pacman repos inside of the Gnome-Software-Center

(Optional) If you prefer to get your Apps organized in Folders in the Gnome-Shell-Overview automatically, feel free to install [https://github.com:BenJetson/gnome-dash-fix.git](https://github.com:BenJetson/gnome-dash-fix.git)

# At This Point, finally a REBOOT

```bash
exit  # leave arch-chroot
umount -a  # Unmount all devices
reboot
```

# After Reboot

Create a Snapshot with **Timeshift**

* Open Timeshift and create a Snapshot Task

* Press Create and wait until the backup / snapshot is done

* Rename the Snapshot (add a Comment) "INIT"

For HiDPI (choose one):

- Open Settings -> Display and Adjust Scaling to 200%

- Open Gnome Tweaks -> Fonts and Adjust Scaling to 1.25

General INIT Maintenance:

- Go through the settings of Gnome and adjust it to your choice.
  
  - We will edit the default Applications later
  
  - Add Keyboard shortcut:
    
    - gnome-terminal (on Ctrl+Alt+T)
    
    - nautilus (on Ctrl+Alt+E)

- Enable Firewall

- Gnome Num-Lock:
  
  ```bash
  # Set NumKey Enabled
  gsettings set org.gnome.desktop.peripherals.keyboard numlock-state true
  ```

#### Gnome Improvements

**Get a Transparent Terminal**

```bash
yay -S gnome-terminal-transparency

#-> Type YES to remove the old terminal
```

**Extensions in the Browser**:

Prepare (Chromium Based Browser):

```bash
yay -S chrome-gnome-shell
```

Visit [https://extensions.gnome.org/](https://extensions.gnome.org/)

* User Themes

* Places Status Indicator

* AppIndicator and KStatusNotifierItem Support

* Removable Drive Menu

* Sound Input & Output Device Chooser

* Caffeine

* Arch Linux Updates Indicator 

* Bluetooth Quick Connect 

* OpenWeather 

* Favourites in AppGrid 

* ddterm

* Resource Monitor or System Monitor

* [Blur my Shell](https://extensions.gnome.org/extension/3193/blur-my-shell/)
  
  * IMPORTANT: The default dock (or Dash-to-Dock Ext) can get a shadow effect by enabling this extension. Tweak the settings (especially: Panel and Dock, disable it) to get rid of it

* DEP: Dynamic Panel Transparency

* Frippery Move Clock

* DEP: Panel OSD

* Gnome 4x UI Improvements 

* DEP: Shell Configurator

* Workspace Indicator

* NEW:
  
  * Desktop Cube
  
  * Useless Gaps
  
  * Fly-Pie
  
  * Compiz windows effect

### Appearance

#### Fonts

These Fonts are for System Appearance and Document Creation Use-Cases

```bash
sudo pacman -S ttf-bitstream-vera ttf-dejavu ttf-liberation adobe-source-sans-pro-fonts
sudo pacman -S ttf-anonymous-pro ttf-droid ttf-ubuntu-font-family ttf-roboto ttf-roboto-mono ttf-font-awesome
yay -S ttf-hackgen ttf-gentium-basic ttf-ms-fonts
sudo pacman -S ttf-fira-code
yay -S ttf-nerd-fonts-hack-complete-git 
# For More Fonts: pacman -Ss ttf
```

##### Styles in Gnome

For Kali-Style use:

- Cantarell Regular 11
- Cantarell Regular 11
- Fira Code Medium 10
- Cantarell Bold 11
- Hinting: Medium
- Gray
- Scaling: 1,35

#### Themes

Use Flat-Remix, Orchis, Nordic or Skeuos

#### Misc

**Arch Artworks**

```bash
yay -S archlinux-artwork
```

**Arch Wallpaper**

```bash
yay -S arch-logo-dark-wallpapers
yay -S arch-linux-2d-wallpapers
```

#### Terminal Color Scheme

[Gogh](http://mayccoll.github.io/Gogh/)

```bash
bash -c "$(curl -sLo- https://git.io/vQgMr)"gpar
```

### Wallpaper

https://wallhaven.cc/
