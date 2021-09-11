

# Arch Linux XFCE4 Installation GUIDE

In this Installation Guide, I will explain my default XFCE4 Setup for ArchLinux with LUKS Drive-Encryption and Kali-Linux Similar Look and Feel of XFCE4. (EFI Style)
For a much simpler installation using a Desktop Environment from a Live Arch Linux OS, feel free to use my Prebuild Arch Linux Live IMAGES on SourceForge. 
[![Download ArchLinux Live ISO Prebuild](https://a.fsdn.com/con/app/sf-download-button)](https://sourceforge.net/projects/archlinux-live-iso-prebuild/files/latest/download)

## Run the Arch Linux Disc
Burn and Run the ArchLinux ISO until the command prompt appears.

## Set up Base
```bash
loadkeys <language>  # e.g. de or de-latin1
```

### Internet Connection
```bash
ip a # check internet connection (alternative: ip link)
```

#### On Wifi
```bash
# newer way:
[iwctl](https://wiki.archlinux.org/index.php/Iwctl)
```bash
iwctl
# Use help for support
device list  # list network devices
station <device> scan  # scan for networks
station <device> get-networks  # get all networks
station <device> connect SSID  # login into your wifi
```

##### old solution
wpa_passphrase  SSID  Passwort  > /etc/wpa_supplicant/wpa_supplicant.conf
wpa_supplicant -i wlp0s1 -D wext -c /etc/wpa_supplicant/wpa_supplicant.conf -B
dhcpcd wlp0s1
```

#### On LAN
```bash
dhcpcd <ethernet-adapter>
```

### Update Database
```bash
pacman -Syyy
```

### (extra) If you want a gui install / config use archfi
If you are using this, you can skip until the following steps to *"Set up XFCE"*
```bash
pacman -S wget pacman-contrib
wget archfi.sf.net/archfi
sh archfi
```

### PreConfig
```bash
export EDITOR=nano <or vim>
```

### Configure Partitions
I use *gdisk* for GPT Tables
```bash
lsblk  # Print Devices
gdisk /dev/<os-device>

n, 128, -200M, Enter, ef00  # Create EFI Partition
n, Enter, Enter, Enter, 8e00  # Create Linux LVM Partition
w

# If you are using a separate home drive, use this
gdisk /dev/<home-device>
n, Enter, Enter, Enter, Enter  # Create Linux Filesystem Partition for the entire drive
```
#### Encrypt Root Partition
```bash
cryptsetup luksFormat /dev/sda1  # Root Partition Device
Confirm and Enter Passphrase.

cryptsetup open /dev/sda1 cryptlvm  # Open Created LVM Device
Enter Passphrase

pvcreate /dev/mapper/cryptlvm  # create Physical Volume

vgcreate vg1 /dev/mapper/cryptlvm  # create Volume Group
```

#### Create Logical Volumes
```bash
lvcreate -L <size-of-ram>G vg1 -n swap
lvcreate -l 100%FREE vg1 -n root
```
Check your work with *lsblk*

#### Format Logical Volumes and Partitions
```bash
mkfs.fat -F32 /dev/sda128
mkfs.ext4 /dev/vg1/root
mkswap /dev/vg1/swap

mkfs.ext4 /dev/<home-partition>
```

##### Mount Partitions
```bash
mount /dev/vg1/root /mnt
mkdir /mnt/home
mount /dev/<home-partition> /mnt/home
mkdir /mnt/boot
mount /dev/sda128 /mnt/boot
swapon /dev/vg1/swap
```

### Create Local Mirror List
```bash 
reflector --verbose --country 'Germany' -l 200 -p https --sort rate --save /etc/pacman.d/mirrorlist

# Alternative
nano /etc/pacman.d/mirrorlist
```

### Install Base Packages
```bash
pacstrap /mnt base base-devel linux<-lts> linux-firmware nano <vim> dhcpcd lvm2 reflector   # <> ist optional

### you can add these packages, to the command above if you like:
# File Systems
dosfstools  # DOS filesystem utilities
btrfs-progs  # Btrfs filesystem utilities
xfsprogs  # XFS filesystem utilities
f2fs-tools  # Tools for Flash-Friendly File System (F2FS)
jfsutils  #  JFS filesystem utilities
reiserfsprogs  # Reiserfs utilities
dmraid  # Device mapper RAID interface

```
[f2fs-tools](https://archlinux.org/packages/extra/x86_64/f2fs-tools/ "View package details for f2fs-tools")

### Config Arch Linux

```bash
arch-chroot /mnt /root  # Change to Root Directory
```

#### Pre Config
```bash
# Set Computer Hostname
echo myhost > /etc/hostname 
 
# Set Keyboard Layout
echo KEYMAP=de-latin1 > /etc/vconsole.conf  

# Set Font (optional)
echo FONT=lat9w-16 >> /etc/vconsole.conf  

# Set Locale
echo LANG=de_DE.UTF-8 > /etc/locale.conf  # Set Language
nano /etc/locale.gen
-> uncomment this:
#de_DE.UTF-8 UTF-8
#de_DE ISO-8859-1
#de_DE@euro ISO-8859-15
#en_US.UTF-8
locale-gen

# Set Time
ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime  # Set Timezone
hwclock --systohc --utc # Sync Hardware-Clock

# Set Root Password
passwd root
exit

# Gererate fstab
genfstab -U /mnt >> /mnt/etc/fstab  # (alt. change -U to -L to use Label instead of UUID
cat /mnt/etc/fstab  # check gen

arch-chroot /mnt  # Switch Back to Root
# Edit Fstab (optional)
nano /etc/fstab

# Edit Crypttab (optional) -> Not used here
# nano /mnt/etc/crypttab  # Config encrypted block devices
```

#### Edit Hosts file
```bash
cat /etc/hosts  # set Hosts file
-->
#<ip-address>	<hostname.domain.org>	<hostname>
127.0.0.1		localhost.localdomain	localhost
::1				localhost.localdomain	localhost
127.0.0.1		<hostname>.localdomain	<hostname>
```

#### Edit Mirror List
```bash 
reflector --verbose --country 'Germany' -l 200 -p https --sort rate --save /etc/pacman.d/mirrorlist

# Alternative
nano /etc/pacman.d/mirrorlist
```

#### Edit mkinitcpio.conf and generate
```bash
nano /etc/mkinitcpio.conf
# Add:
# ... autodetect keyboard keymap modconf block encrypt lvm2 filesystems ...

# Generate mkinitcpio
mkinitcpio -p linux-lts
```

#### Bootloader
(I used grub, you can use *syslinux* instead, if you want)
```bash
pacman -S grub efibootmgr 
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

blkid  # BlockID
-> Copy or note down UUID of root partition (encrypted)
nano /etc/default/grub
--> Edit: 
...
GRUB_CMDLINE_LINUX="cryptdevice=UUID="<UUID-copied>":cryptlvm root=/dev/vg1/root"

grub-mkconfig -o /boot/grub/grub.cfg  # Generate Grub config file
```

## Set up Arch Linux Root

### Updates
```bash
# Install Contributed scripts and tools for pacman systems
pacman -S pacman-contrib

# Update System
pacman -Syu

# Clean orphan
pacman -Rns $(pacman -Qqtd)

# Clean Cache
pacman -Sc

# Edit pacman.conf
nano /etc/pacman.conf
# Uncomment Color
# Add ILoveCandy
# uncomment multilib
pacman -Sy

### Optional - Segnature Security ###
pacman -S archlinux-keyring  # Update keyring
pacman-key --refresh-keys  # Refresh pacman keys
# evtl. edit pacman.conf -> SigLevel
gpg --recv-keys  # Add GPG key (optional-additional)
```
### Add Accounts
```bash
useradd -m -g users -s /bin/bash <name>
passwd <name>
EDITOR=nano visudo
--> Uncomment: # %wheel ALL=(ALL) ALL
gpasswd -a <user> wheel  # add user to wheel group
gpasswd -a <user> audio,video,games,power,optical
```

### Install

#### Install YAY
Change to User - This is required, because yay can't run as root because it uses the fake root to install packages.

If you prefer to stay as root, feel free to replace this instead of an yay command (1. Method)
```bash
# the command yay -S <name>
git clone https://aur.archlinux.org/<name>.git
cd <name>
makepkg -si
```
If you prefer using yay as root, use the following command instead (2. Method)
```bash
# the command yay -S <name>
sudo -u <username-that-is-not-root> yay -S <name>
```

##### Installation (if you want to use yay)
```bash
su <uname>
```
```bash
# Install YAY - simple AUR Package Installation
pacman -S git
git clone https://aur.archlinux.org/yay.git
cd yay/
makepkg -si PKGBUILD
# Alternativ Package Installer: trizen, aurman, yaourt (End of life)
```
If you are using root to install this packages, run this commands as they are. If you use an other user to install the packages, use the prefix ```sudo ```

#### Console
```bash
### Generic
pacman -S usbutils dialog powertop
# optional #
# lsof  # Lists open files for running Unix processes
# dmidecode  # Hardware Infos
# nmon  # Systen Monitor
# mc  # Dual Pane File Explorer
# neofetch  # system information tool
# fwupd  # Firmware upgrade
# gpm  # Console Mouse Support
# liveroot  # (AUR) root overlay fs

### Compression Tools
pacman -S zip unzip unrar p7zip lzop

### Network Tools
pacman -S rsync traceroute bind
# optional #
# dnsutils  # DNS tools (nslookup)
# nmap  # Network Scanner
# netdiscover  # (AUR) Network Scanner with MAC vendors
# speedtest-cli  # SpeedTest
# arp-scan  # ARP SCANNER
# wavemon  # WIFI monitor
# net-tools  # (deprecated) old ifconfig
# dsniff  # tools for network auditing and penetration
# mitmproxy  # SSL-capable MITM HTTP proxy
# sslstrip  # tool to hijack HTTPS in MITM attack

### Web Browser
# optional: elinks, links, lynx

### Recovery Tools
# optional # 
# ddrescue  # HD recovery tool
# dd_rescue  # HD recovery tool
# partclone  # Copy used block on partition
```

#### System
```bash
### Services
pacman -S networkmanager openssh xdg-user-dirs intel/amd-ucode (bluez bluez-utils) pkgstats 
# optional #
# cronie  # Cron tasks server
# numlockon  # Numlock on on tty
# net-snmp  # SNMP Server
# samba  # Server SMB
# syslog-ng
# ntp
# haveged

### Filesystem
pacman -S os-prober dosfstools ntfs-3g gvfs 
# optional #
# snapper  # snapshot manager (ext4, lvm, btrfs)
# btrfs-progs  # BTRFS ffile utils
# exfat-utils  # EXFAT file support
# gptfdisk
# autofs 
# fuse2 
# fuse3
# fuseiso 
# sshfs  # ssh client
# cifs-utils  # SMB mound command
# smbclient  # SMB full client
# nfs-utils
# open-iscsi
# glusterfs
# hfsprogs (AUR)
# mtpfs
# unionfs-fuse
# nilfs-utils
# s3fs-fuse

### Sound
pacman -S alsa-plugin alsa-utils pulseaudio pulseaudio-alsa (pulseaudio-bluetooth)
# optional: pulseaudio-equalizer

### Printer
pacman -S cups ghostscript cups-pdf hplip
# optional #
# gutenprint  # Top quality printer drivers for POSIX systems
# foomatic-db foomatic-db-*  # Foomatic - The collected knowledge about printers, drivers, and driver options in XML files, used by foomatic-db-engine to generate PPD files.
```

#### XOrg
```bash
### Print GPU Info
lspci -k | grep -A 2 -E "(VGA|3D)"

### Install
pacman -S --needed xorg

### Fonts - Default
pacman -S ttf-bitstream-vera ttf-dejavu ttf-liberation adobe-source-sans-pro-fonts
# optional #
# font-bh-ttf
# gsfonts 
# sdl_ttf
# xorg-fonts-type1 

### Fonts - Misc
pacman -S ttf-anonymous-pro ttf-droid   ttf-ubuntu-font-family ttf-roboto ttf-roboto-mono ttf-font-awesome
# for my purpose
yay -S ttf-hackgen ttf-gentium-basic ttf-ms-fonts
pacman -S ttf-fira-code
# For More Fonts: pacman -Ss ttf

### input drivers
pacman -S xf86-input-synaptics (optional: Laptop Touchpad-Driver)
# optional #
# xf86-input-libinput  #  Generic input driver for the X.Org server based on libinput
# xf86-input-elographics
# xf86-input-evdev
# xf86-input-vmmouse  # (VMWare)
# xf86-input-void
# xf86-input-wacom
# other: Saitek-*, Madcatz-*

### video drivers

# Proprietary
sudo pacman -S nvidia (if linux-lts -> nvidia-lts)
# optional #
# virtualbox-guest-utils  # (For Virtualbox)
# nvidia-390xx  # (AUR) End of life
# nvidia-dkms  # (For custom kernel)
# nvidia-390xx-dkms  # (AUR) (for custom kernel) -> for my older laptop
# optional: nvidia-settings

# Opensource
# xf86-video-*
# e.g. nouveau

# ! Only use one Proprietary or Opensource Video Driver !
```
also check out this [link](https://wiki.archlinux.org/index.php/NVIDIA)


#### Desktop Environment
Choose one of the following:
1. [Plasma5](https://wiki.archlinux.de/title/Plasma) (*Components: partitionmanager gnome-keyring xsettingsd kdeconnect sshfs systemdgenie*)  # QT
2. XFCE4  # GTK
3. Gnome  # GTK
4. Cinnamon
5. LXDE
6. LXDE-GTK3
7. LXQt
8. Mate
9. Enlightenment
10. Openbox
11. Deepin

**See next Chapter**

*TODO - Package installation guide for the Desktop Environments above* (mit archdi - hint)

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

### Config

#### Bash
```bash
/etc/profile.d/editor.sh  # set default global editor
nano /etc/profile.d/alias.sh
```
use the the folowing rows:
``` alias <left>="<right">```
The (n) notation means, that there are n ways of signing this alias. Do not use this (n) e.g. (1) in the alias notation!
![enter image description here](https://normannator.de/archlinux/IMG/Alias.PNG)

Edit the ~/.bashrc file to add alias permanently for your user.

Example Alias Config Section:
```bash
# Main ALIAS
alias ls="ls --color=auto"
alias l="ls --color=auto -lA --time-style long-iso"
alias ll="ls --color=auto -la --time-style long-iso"
alias cd..="cd .."
alias ..="cd .."
alias ...="cd ../../"
alias ....="cd ../../../"
alias .....="cd ../../../../"
alias ff="find / -name"
alias f="find . -name"
alias grep="grep --color=auto"
alias egrep="egrep --color=auto"
alias fgrep="fgrep --color=auto"
alias ip="alias ip='ip -c'"
alias pacman="pacman --color auto"
alias pactree="pactree --color"
alias vdir="vdir --color=auto"
alias watch="watch --color"
alias man="color function"
alias yay="sudo -u norman yay"
alias browser="midori &"

# Kali Undercover Mode
alias panic="kali-undercover"
alias ku="kali-undercover"
alias qwer="kali-undercover"

# CUSTOM Additional ALIAS
alias pgadmin="sudo /usr/pgadmin4/bin/pgadmin4"
alias haskel="ghci"
alias cyberghost="sudo cyberghostvpn --traffic sudo cyberghostvpn --traffic --connect --country-code"
alias cyberghostlist="cyberghostvpn --traffic --country-code"
alias spotifys="spotify --force-device-scale-factor=2.0 %U"
alias vi="vim"
```

**Optional**
```bash
nano /etc/profile.d/ps1.sh  # (optional)
# Minimal				/home #
# User					user:/home #
# User and Hostname		user@hostname:/home #

# Now update .bashrc  # (optional)
```

#### Firewall (optional)
```bash
# edit IPv4			/etc/iptables/iptables.rules
# edit IPv6			/etc/iptables/ip6tables.rules

# load Rules
# iptables-restore & ip6tables-restore

# Start At Boot
# systemctl enable iptables & systemctl enable ip6tables

# Generate Default Rules
# /etc/iptables/iptables.rules & /etc/iptables/ip6tables.rules
```
**!!! For a simple Firewall !!!**
```bash
sudo pacman -S ufw
sudo ufw enable
sudo ufw status verbose
sudo systemctl enable ufw.service
```

#### Systemd

##### Enable installed services
```bash
Enable or Disable Services
systemctl enable NetworkManager
systemctl enable cups.service # previously installed
systemctl enable sshd.service # ssh Service
```
##### Set up important Services
```bash
pacman -S acpid dbus avahi

systemctl enable acpid
systemctl enable avahi-daemon
```

##### Timedatectl  (optional)
The hardware clock can be queried and set with the _timedatectl_ command
```bash
# Enable  # timedatectl set-ntp true
# Edit  # /etc/systemd/timesyncd.conf
systemctl enable --now systemd-timesyncd.service  # Sync System Time with atomic clock
```

#### Boot (optional and redundant -> done above)
```bash
### initcpio
# edit /etc/mkinitcpio.conf
# mkinitcpio -p linux(-lts)
```


### Install other optional Packages
```bash
pacman -S  wireless_tools wpa_supplicant  mtools  linux-<lts->headers git bash-completion jre8/11-openjdk 
xdg-utils  # Home Directories

pacman -S  gst-libav gst-plugins-good  # Multimedia graph framework
pacman -S icedtea-web  # Additional components for OpenJDK - Browser plug-in and Web Start implementation (optional)
```

### Install Desktop Environment
------
#### XDLE
```bash
pacman -S ttf-dejavu ttf-liberation ttf-bitstream-vera lxde xorg xorg-video-drivers xorg-input-drivers gamin\
gnome-icon-theme tango-icon-theme gtk-engines pm-utils udisks
```
##### Configuration for Login Managers

###### GDM or KDM

No manual configuration is needed. Just select LXDE from the available sessions listed by the display manager. If you don't see LXDE, restart your gdm or kdm, or reboot.

###### SLIM

With this display manager, some manual configuration is needed. Please refer to their  [official document](http://slim.berlios.de/manual.php)  and write your /etc/slim.conf and ~/.xinitrc. The command you should put in your ~/.xinitrc to start LXDE is:
```bash
exec startlxde
```
###### XDM

Put this line at the end of "~/.xinitrc".
```bash
exec startlxde
```
Be sure to comment all other entries.

###### No display manager, use startx

Put this line at the end of "~/.xinitrc" or "/etc/X11/Xsession".
```bash
exec startlxde
```

#### XFCE4
```bash
sudo pacman -S --needed xfce4 xfce4-goodies lightdm lightdm-gtk-greeter lightdm-gkt-greeter-settings (gvfs-afc) udisks2 network-manager-applet

sudo systemctl enable lightdm
sudo systemctl start lightdm
```
**XFCE Specific Config**
```bash
sudo pacman -S xfce4-session xorg-xrandr
sudo pacman -S xfce4-whiskermenu-plugin
sudo yay -S alacarte-xfce (not tested)
yay -S menulibre xame
sudo pacman -S gambas3-gb-gtk3 libcanberra libcanberra-pulse sound-theme-freedesktop
# Description #
# alacarte  # Menu editor for gnome
# menulibre  # An advanced menu editor that provides modern features in a clean, easy-to-use interface
# xame  # XFCE Applications Menu Editor
# gambas3-gb-gtk3  # GTK3 toolkit component
# libcanberra + libcanberra-pulse  # A small and lightweight implementation of the XDG Sound Theme Specification
# sound-theme-freedesktop  # Freedesktop sound theme

yay -S sound-theme-smooth xfce4-volumed-pulse xfce4-mixer 
sudo pacman -S xfce4-pulseaudio-plugin xscreensaver udevil udiskie thunar thunar-volman thunar-archive-plugin thunar-media-tags-plugin
# Description #
# udevil  # Mount and unmount without password
# udiskie  # Removable disk automounter using udisks

sudo pacman -S tumbler libgsf catfish mlocate  # Thunar Specific Packages
yay -S env-modules # alternatively env-modules-tcl
# yay -S xfce4-sensors-plugin-nvidia - DEPRECATED

Description
# tumbler  # D-Bus service for applications to request thumbnails
# libgsf  # An extensible I/O abstraction library for dealing with structured file formats
# catfish  # Versatile file searching tool
# mlocate  # Merging locate/updatedb implementation
# env-modules/-tcl  # Provides for an easy dynamic modification of a user's environment via modulefile.
```

**XFCE4 Settings**

#### Popup Menu on Windows-Icon
In Keyboard Shortcuts set ```xfce4-popup-whiskermenu``` run on windows-key

yay -S xfce4-screensaver mugshot gnome-system-tools

```bash
# To Install this, you need to do some more complicated stuff
git clone https://aur.archlinux.org/gnome-system-tools.git
cd gnome-system-tools/
makepkg -si
yay -S gnome-doc-utils
yay -S liboobs -> git clone https://aur.archlinux.org/liboobs.git
cd liboobs
nano PKGBUILD (change FTP (first to HTTP/S)
makepkg -si (maybe edit PKGBUILD)
yay -S system-tools-backends
makepkg -si

yay -S gnome-system-tools

# Now Clean Your AUR Clones Directory
```

**Make XFCE4 look good!**

**Theme and Fonts - For Kali Look**
```bash
sudo pacman -S arc-gtk-theme arc-icon-theme
yay -S flat-remix cantatel-fonts
# sudo pacman -S futurist - Deprevated
```

**Plank**
```bash
yay -S plank  # Dock like OSX
yay -S plank-theme-arc
```

**Desktop**
1. Right-Clock on Desktop -> Settings:
2. Select *usr/share/backgrounds/archlinux* as background folder and choose a nice background
3. Symbols -> Default Icons: 
	* Deselect Home and Filesystem.
	* Iconsize: 40
	* Show icon tooltips. Size: 100

**Theme, Icon and Font**
Settings->Appearance:
1. Style: Arc-Dark
2. Icons: Flat-Remix-Blue-Dark
3. Fonts: 
	* Default Font: Cantarel Regular - 11
	* Default Monospace Font: Fira Code Medium - 10
	* Rendering:
		* Hinting: Low (Gering)
		* Sub-Pixel order: RGB

Settings->Window Manager:
1. Style: 
	* Style: Arc-Dark
	* Title Font: Cantarel Bold - 9
	* Hide arrow up

For more Themes visit [this Page](https://www.xfce-look.org/)

##### Terminal
![enter image description here](https://normannator.de/archlinux/IMG/term-1.png)
![enter image description here](https://normannator.de/archlinux/IMG/term-2.png)
![enter image description here](https://normannator.de/archlinux/IMG/term-3.png)
![enter image description here](https://normannator.de/archlinux/IMG/term-4.png)
![enter image description here](https://normannator.de/archlinux/IMG/term-5.png)
![enter image description here](https://normannator.de/archlinux/IMG/term-6.png)

**LightDM Settings**
Theme: Arc-Dark
Icon: Arc
Font: Cantarel Bold - 10

**Screensaver**
Settings->Screensaver: 
* Floating XFCE
* Disable: Lock Screen with System Sleep
Settings->Keyboard: xflock4 (default Ctrl+Alt+L/Delete)

**Whisker-Menu**
Delete the Old Menu Button and add the installed Wisker Menu to the Top Panel.
Right Click and Open Settings of Whisker Menu:
* List view
* Symbol-Size: Small
* Symbol Category: Smaller
* Head->Symbol only + Symbol -> ArchLinux Symbol 
* Behaviour: Select on Hover

Settings->Keyboard->Shortcuts: add on pressing *super* (Windows Button): xfce4-popup-whiskermenu

**More Configuration**
Open Whisker-Menu and Klick on the User Icon:
-> Mugshot should open: Enter your Credentials

In Settings: Type "User" and Open: *User Settings* (gnome-system-tools)
Administrate User and Groups ...

Logout and Login again that the changes take effect.

**Config Light DM Greeter**
```bash
sudo pacman lightdm-webkit2-greeter

# Open /etc/lightdm/lightdm.conf
user-session=awesome # your user-session -> ls /usr/share/xsessions/*.desktop

greeter-session=lightdm-webkit2-greeter
```
----
-----
----
#### GNOME
```bash
sudo pacman -S gnome gnome-extra gdm  
sudo systemctl enable gdm.service  
sudo systemctl start gdm.service
#Download Gnome-Extensions aus dem Gnome-Software-Store  

sudo pacman -S gnome-software-packagekit-plugin #Damit man auf das AUR und Pacman mit dem Softwarecenter von Gnome zugreifen kann.  
#github.com:BenJetson/gnome-dash-fix.git #Gnome Kategorien in Anwendungsmenü automatisch generieren  

gsettings set #org.gnome.desktop.peripherals.touchpad click-method areas #Lösen von Gnome problemen für Touchpads von Laptops  

yay -S gnome-terminal-transparency  
#-> Say YES to deinstall the old terminal  
```
**Extensions im Browser (mit Plugin und gnome-shell-extension (yay))**:
* [Blur my Shell](https://extensions.gnome.org/extension/3193/blur-my-shell/)
* cpufreq
* Dynamic Panel Transparency
* Espresso
* Frippery Move Clock
* Panel OSD
* Screenshot Tool
* Shell Configurator
* Sound Input & Output Device Chooser
* Tray Icons: Reloaded
* Time ++ (Pomodoro,Timer,...)
* DEFAULT EXTENSIONS
	* Applications Menu
	* Places Status Indicator
	* Removable Drive Menu
	* User Themes
	* Workspace Indicator

**Gnome Tweaks**:

Stop the computer from suspending when the lid is closed:
1.  start  Tweaks.
2.  Select the  General  tab.
3.  Switch the  Suspend when laptop lid is closed  switch to off.
4. Close the  Tweaks  window.

Other:
```bash
mkdir -p ~/.icons
mkdir -p ~/.themes
```
* Appearance
	* [Flat-Remix-GTK-Blue](https://www.gnome-look.org/p/1214931/)
		* Extract Archive and move to: */usr/share/themes/*
	* [Sweet-cursors](https://www.gnome-look.org/p/1393084/)
		* Extract Archive and move to: *~/.icons*
	* [Falt-Remix-Blue-Light/Dark](https://www.gnome-look.org/p/1012430/)
		* Extract Archive and move to: *~/.icons*
		* Alternative: [Neonyt](https://www.gnome-look.org/find?search=Neonyt)
	* [Flat-Remix-Blue-fullPanel](https://www.gnome-look.org/p/1013030/)
		* Extract Archive and move to: *~/.themes*
* Fonts
	* Cantarell Regular 11
	* Cantarell Regular 11
	* Fira Code Medium 10
	* Cantarell Bold 11
	* Hinting: Medium
	* Gray
	* Scaling: 1,35
* Startup
	* Thunderbird
	* (JetBrains Toolbox (auto))
	* (Insync (auto))

**Wallpaper**
* [Desert Blue](https://www.pling.com/p/1361442/)
* [Neonyt](https://www.pling.com/p/1463678/)

---
### Exit and Reboot
```bash
exit
umount -a
reboot
```

## Config - OS

### Connect to Internet

#### With Wifi
```bash
# newer way:
[iwctl](https://wiki.archlinux.org/index.php/Iwctl)
```bash
iwctl
# Use help for support
device list  # list network devices
station <device> scan  # scan for networks
station <device> get-networks  # get all networks
station <device> connect SSID  # login into your wifi
```

##### old solution
wpa_passphrase  SSID  Passwort  > /etc/wpa_supplicant/wpa_supplicant.conf
wpa_supplicant -i wlp0s1 -D wext -c /etc/wpa_supplicant/wpa_supplicant.conf -B
dhcpcd wlp0s1
```
```bash
# Alternative
ip a # search Wifi Adapter
nmtui
-> Activate Connection->...
```

#### With Ethernet
```bash
ip a # search for Ethernet adapter
dhcpcd <adapter>
```

### Use Reflector as above
```bash
reflector --verbose --country 'Germany' -l 200 -p https --sort rate --save /etc/pacman.d/mirrorlist
```

### Update System
```bash 
pacman -Syu
```

### (optional) Encrypt Home Partition
**Notice! This only makes sense, if your home partiton is on an other drive than your root partition or you did not encrypt your root partition**
```bash
# !!! Backup all your Data and copy the backup to another drive or to the root drive  before this step !!!
# Logged out. 
# Switch to a console with Ctrl+Alt+F2. 
# Login as a root and check that your user own no processes: 
ps -U username

umount /home
lsblk  # to check if umount was successful

gdisk /dev/<home-device>  # Type as LUKS
cryptsetup luksFormat /dev/<home-device>
cryptsetup open /dev/<home-device> crypt_home
mkfs.ext4 /dev/mapper/crypt_home
mount /dev/mapper/crypt_home /home

# Now copy your Backups to the Home directory (sudo recommended)

# Change files to the right rights (important, if you backup and restore your files as root or with sudo)
cd /home/
chown -R <user> <user>/
chgrp -R users <user>/

lsblk -f  # Note down the UUID of your device
# <device>
# - <device-partition>
# - - <crypto 2>  UUID

nano /etc/crypttab  # Edit this file and insert the following row
# crypthome     UUID=<UUID enter here>    none                    luks,timeout=180

cp /etc/fstab /etc/fstab.bak  # Backup Your FSTAB File
nano /etc/fstab  # Edit this file
### File content ###
# <file system> <dir> <type> <options> <dump> <pass>

# /dev/mapper/vg1-root
UUID=...				/		ext4	rw,relatime     0 1

# /dev/<home-device>
/dev/mapper/crypthome	/home	ext4	rw,relatime     0 2

# /dev/sdb128
UUID=...				/boot	vfat	rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,utf8,errors=remount-ro   0 2

# /dev/mapper/vg1-swap
UUID=...				none	swap	defaults        0 0

## alternatively
genfstab -U


### If Reboot does not work, try to rebuild the grub cfg.
# Press Ctrl + Alt + F2
cd /boot/grub
mv grub.cfg grub.cfg.bak
grub-mkconfig -o /boot/grub/grub.cfg
reboot

### If Startup works ... Continue

```bash
# Reboot and make sure that you can login to your desktop
# If everything workes fine, feel free to remove your Backup (done before)
```


### Generate SSH-Key
```bash
ssh-keygen -t rsa -b 4096
```

### Main Maintenance
**SSH**
```bash
# sudo systemctl enable sshd
# sudo systemctl start sshd
# -> Already done, if not, run this commands
```
**OpenVPN**
```bash
# Setup OpenVPN
sudo pacman -S openvpn networkmanager-openvpn
systemctl restart networkmanager
# import openvpn config with:
openvpn import --config <config-incl-path>
nmcli connection import type openvpn file <file-to-ovpn>
```

**Misc**
```bash
sudo pacman -S ffmpegthumbnailer  # Lightweight video thumbnailer that can be used by file managers.
sudo pacman -S raw-thumbnailer  # A lightweight and fast raw image thumbnailer that can be used by file managers.

sudo pacman -S xarchiver file-roller ark archlinux-wallpaper xwallpaper archivetools archlinux-menu archlinux-themes-slim archlinux-xdg-menu fastjar
```

**Theme and Fonts - For Kali Look**
If not already done in XFCE
```bash
sudo pacman -S arc-gtk-theme arc-icon-theme
yay -S flat-remix cantatel-fonts
# sudo pacman -S futurist - Deprevated
```

**Setup Useful Shortcuts**
- Super + T: Terminal
- Super + C: Chrome
- Super + F: File Manager

---
### Must Have Programms / Packages

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
yay -S github-desktop
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


## Post Steps and Maintenance

### Remove Orphans
```bash
sudo pacman -Rns $(pacman -Qtdq)  # Removes unused Packages
yay -Sc  # Removes Cache of YAY

# Also: https://ostechnix.com/recommended-way-clean-package-cache-arch-linux/
```

### Optimize pacman's database access speeds
```bash
sudo pacman-optimize
```

### Reduce Swappiness
Forces System to use as much RAM as possible and reduces hard drive access
```bash
cat /proc/sys/vm/swappiness  # 60 by DEFAULT
sudo nano /etc/sysctl.d/100-archlinux.conf
-> vm.swappiness=10
reboot

cat /proc/sys/vm/swappiness  # should be 10 now
```

### Trim SSD
Enable Trim for SSD - optimize performance of ssd-drive
```bash
sudo systemctl status fstrim.timer  # should be inactive, if not, skip to next heading

sudo systemctl start fstrim.timer 
sudo systemctl status fstrim.timer  # should be active now
```

### Set up XKILL Shotcut
Check the shotcuts menu. If xkill is added, skip to the next heading. If not, add this:
Ctrl+Alt+X        ->        xkill


### Make Arch Stable (Golden Rules)
1. Do Backups with Timeshift and BtrFS
2. Update only every Week
3. Install Linux and Linux-LTS but us the LTS Kernal. If the System breaks, you can switch to the non LTS Kernal when the system is not booting. (use: ```uname -sr``` to check the current used kernel)
4. Ignore Linux Packages on Update (pacman.conf -> ignorepkg = Linux)
5. Do not install out of date packages! Use the AUR Page and search under package actions: *Flagged out-of-date (2019 <year not updeated>)*. You can also see this in the aur-tool logs!


### Check for errors
```bash
sudo systemctl --failed sudo journalctl -p 3 -xb
```

### (optional) Backup your System manually
```bash
sudo rsync -aAXvP --delete --exclude=/dev/* --exclude=/proc/* --exclude=/sys/* --exclude=/tmp/* --exclude=/run/* --exclude=/mnt/* --exclude=/media/* --exclude=/lost+found --exclude=/home/.ecryptfs / /mnt/backupDestination/
```

## Set Dual Boot
 ### In general
 ```bash
 sudo pacman -S os-probe
 
 sudo pacman -S ntfs-3g  #opt: If target Os is Windows

 # Mount Dir of OS
 sudo mkdir -p /mnt/os-mount
 sudo mount /dev/<partition-of-os-to-dual-boot> /mnt/os-mount
 
 # run os-prober
 os-prober
 
 # recreate Grub Config
 grub-mkconfig -o /boot/grub/grub.cfg
```


## Extra

**Most**
```bash
#Install Most for Manual Page highlighting

sudo pacman -S most  
export PAGER=most

# See results on "man mv"
```

**Pamac**
A Package Manager for Arch Linux Pacman Packages
```bash
sudo pacman -S appstream-glib 
yay -S archlinux-appstream-data-pamac
yay -S pamac-aur-git

# If "Missing Dependency: pamac-aur-git"
yay -S libpamac
```

## Cleanup Tips

### Clean PKG
* sudo pacman -Sc
* or install *pacman-contrib*
	* paccache -h    # help page
	* ... -d  #Dry Mode (just list packages to remove)
	* .... -r  #Remove Packages from Dry Mode
* oder mit paccache.timer jeden Monat automatisch bereinigen
	* sudo nano /etc/systemd/system/paccache.timer
	* For this enter:
		```
		[Unit]
		Description=Clean-up old pacman pkg

		[Timer]
		OnCalendar=monthly
		Persistent=true

		[Install]
		WantedBy=multi-user.target
		```

### Remove Orphans
```bash
sudo pacman -R $(pacman -Qtdq)
```

### Clean Cache in /home
```bash
du -sh ~/.cache/    # Size of Cache
rm -rf ~/.cache/*
```

### Remove old config files
-> ~/.config und ~/.local/share
Remove them manualy! (only uninstalled files recommended)

### Find and remove duplicates, empty files, empty dirs, broken symlinks
```bash
sudo pacman -S rmlint
rmlint --help
rmlint /home/user     #Scans folder and saves special script to remove findings (read results)
```

### Find the largest files and directories
```bash
sudo pacman -S ncdu
ncdu   # Run tool (command line)
```

#### Alternative
use *Filelight* #GUI Tool

#### oder
*Baobab*    #Gnome Tool

### Disk Cleaning Program
*BleachBit*

### Clean Trash (Remember)
You know how to do that ;)


## Improve Performance

https://wiki.archlinux.org/title/improving_performance

## Arch Linux for Music Production
[This Guide shows the steps to make Arch Linux a open source Music Production System](https://normannator.de/archlinux/musicproduction/index.html)
