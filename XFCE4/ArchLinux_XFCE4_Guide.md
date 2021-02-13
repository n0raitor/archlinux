

# Arch Linux XFCE4 Installation GUIDE

In this Installation Guide, I will explain my default XFCE4 Setup for ArchLinux with LUKS Drive-Encryption and Kali-Linux Similar Look and Feel of XFCE4. (EFI Style)

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
wpa_passphrase  SSID  Passwort  > /etc/wpa_supplicant/wpa_supplicant.conf
wpa_supplicant -i wlp0s1 -D wext -c /etc/wpa_supplicant/wpa_supplicant.conf -B
dhcpcd wlp0s1
```

#### On LAN
```bash
dhcpcd <ethernet-adapter>
```

### Create Local Mirror List
```bash 
reflector --verbose --country 'Germany' -l 200 -p https --sort rate --save /etc/pacman.d/mirrorlist
```

### Update Database
```bash
pacman -Syyy
```

### Configure Partitions
I use *gdisk* for GPT Tables
```bash
lsblk
gdisk /dev/<device>

n, 128, -200M, Enter, ef00  # Create EFI Partition
n, Enter, Enter, Enter, 8e00  # Create Linux LVM Partition
w
```
### Encrypt Root Partition
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
mkfs.ext7 /dev/vg1/root
mkswap /dev/vg1/swap
```

#### Mount Partitions
```bash
mount /dev/vg1/root /mnt
mkdir /mnt/boot
mount /dev/sda128 /mnt/boot
swapon /dev/vg1/swap
```

### Install Base Packages
```bash
pacstrap /mnt base base-devel linux linux-firmware nano dhcpcd intel/amd-ucode lvm2
```

### Generate Filesystem-Table
```bash
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab  # check gen
```

### Change to Root
```bash
arch-chroot /mnt
```

### Generate and Edit Config Files
```bash
echo myhost > /etc/hostname  # Set Hostname
echo LANG=de_DE.UTF-8 > /etc/locale.conf  # Set Language

nano /etc/locale.gen
-> uncomment this:
#de_DE.UTF-8 UTF-8
#de_DE ISO-8859-1
#de_DE@euro ISO-8859-15
#en_US.UTF-8
locale-gen

echo KEYMAP=de-latin1 > /etc/vconsole.conf  # Set Keyboard Layout
echo FONT=lat9w-16 >> /etc/vconsole.conf  # Set Console Font (optional)

ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime  # Set Timezone
hwclock --sysohc  # Sync Hardware-Clock to System-Clock (optional)

cat /etc/hosts  # set Hosts file
-->
#<ip-address>	<hostname.domain.org>	<hostname>
127.0.0.1		localhost.localdomain	localhost
::1				localhost.localdomain	localhost
127.0.0.1		<hostname>.localdomain	<hostname>
```

### Install Packages
```bash
pacman -S grub efibootmgr networkmanager network-manager-applet wireless_tools wpa_supplicant dialog os-prober mtools dosfstools linux-headers git reflector bash-completion
(bluez bluez-utils pulseaudio-bluetooth) # Bluetooth
(cups) # Printing
xdg-utils xdg-user-dirs # Home Directories
```

### Set Root Password
```bash
passwd
```

### Install GRUB
```bash
nano /etc/mkinitcpio.conf
Add:
... autodetect keyboard keymap modconf block encrypt lvm2 filesystems ...
mkinitcpio -p linux

grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

blkid # BlockID
-> Copy or note down UUID of root partition (encrypted)
nano /etc/default/grub
--> Edit: 
...
GRUB_CMDLINE_LINUX="cryptdevice=UUID="<UUID-copied>:cryptlvm root=/dev/vg1/root"

grub-mkconfig -o /boot/grub/grub.cfg # Generate Grub config file
```

### Set up Services
```bash
systemctl enable NetworkManager
systemctl enable cups.service # previously installed

pacman -S acpid dbus avahi

systemctl enable acpid
systemctl enable avahi-daemon

systemctl enable --now systemd-timesyncd.service  # Sync System Time with atomic clock
```

### Set up User and Groups
```bash
useradd -m -g users -s /bin/bash <name>
passwd <name>
EDITOR=nano visudo
--> Uncomment: # %wheel ALL=(ALL) ALL
gpasswd -a <user> wheel  # add user to wheel group
gpasswd -a <user> audio,video,games,power
```

### Exit and Reboot
```bash
exit
umount -a
reboot
```

## Install Desktop Environment

### Connect to Internet

#### With Wifi
```bash
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

### Install YAY for simple AUR Package Installation
```bash
git clone https://aur.archlinux.org/yay.git
cd yay/
makepkg -si PKGBUILD
```

### Install Graphics Drivers
```bash
sudo pacman -S nvidia nvidia-utils nvidia-settings
```
also check out this [link](https://wiki.archlinux.org/index.php/NVIDIA)

In my case of Laptop:
```bash
yay -S nvidia-390xx-dkms
```

### Install Browser
```bash
sudo pacman -S vivaldi vivaldi-ffmpeg-codecs
```

### Install XFCE4 and Greeter
```bash
sudo pacman -S xorg lightdm lightdm-gtk-greeter lightdm-gkt-greeter-settings xfce4 xfce4-goodies

sudo systemctl enable lightdm
reboot
```

## Set up XFCE4

### Install Timeshift
```bash
yay -S timeshift
```

### Install VirtualBox
```bash
sudo pacman -S virtualbox virtualbox-host-modules-arch virtualbox-guest-iso
sudo gpasswd -a <user> vboxusers
systemctl enable dkms
sudo nano /etc/modules-load.d/my-modules.conf
--> add: vboxdrv
reboot # optional
```

### Generate SSH-Key
```bash
ssh-keygen
```

### Main Maintenance
```bash
sudo pacman -S xfce4-session arandr zip unzip (bash-completion)
sudo pacman -S links geany udisks2
sudo pacman -S xfce4-whiskermenu-plugin
sudo pacman -S alacarte menulibre xame gambas3-gv-gtk libcanberra libcanberra-pulse sound-theme-freedesktop
yay -S sound-theme-smooth xfce4-volumed-pulse xfce4-mixer 
sudo pacman -S xfce4-pulseaudio-plugin xscreensaver udevil udiskie thunar gvfs thunar-volman thunar-archive-plugin thunar-media-tags-plugin

sudo pacman -S networkmanager-openvpn
systemctl restart networkmanager

sudo pacman -S ffmpegthumbnailer tumbler raw-thumbnailer libgsf catfish mlocate
yay -S env-modules env-modules-tcl
yay -S xfce4-sensors-plugin-nvidia

sudo pacman -S thunarx xarchiver file-roller (ark) archlinux-wallpaper xwallpaper archivetools archivetools archlinux-menu archlinux-themes-slim archlinux-xdg-menu fastjar p7zip

pacman -S xf86-input-synaptics (optional: Laptop Touchpad-Driver)
```

### XFCE4 Settings

#### Popup Menu on Windows-Icon
In Keyboard Shortcuts set ```xfce4-popup-whiskermenu``` run on windows-key

#### setup Theme and Fonts
```bash
yay -S ttf-hackgen (ttf-fira-code)
sudo pacman -S ttf-fira-code arc-gtk-theme arc-icon-theme
yay -S flat-remix catarell catarell-fonts
sudo pacman -S futurist
```
#### setup undercover
```bash
yay -S kali-undercover
alias ku=/usr/bin/kali-undercover
```
#### add additional packages
```bash
sudo pacman -S gufw openssh
sudo systemctl enable openssh.service
sudo systemctl enable sshd
sudo systemctl start sshd

sudo pacman -S redshift
(sudo systemctl enable redshift)

yay -S xfce4-screensaver mugshot gnome-system-tools

git clone https://aur.archlinux.org/gnome-system-tools.git
cd gnome-system-tools/
nano PKGBUILD (change FTP (first to HTTP/S)
makepkg -si
yay -S gnome-doc-utils
yay -S liboobs -> git clone https://aur.archlinux.org/liboobs.git
cd liboobs
makepkg -si (maybe edit PKGBUILD)
yay -S system-tools-backends
makepkg -si

yay -S gnome-system-tools
# Now Clean Your AUR Clones Directory

sudo pacman -S ntfs-3g openvpn network-manager-applet networkmanager networkmanager-openvpn
# import openvpn config with:
openvpn import --config <config-incl-path>
nmcli connection import type openvpn file <file-to-ovpn>

yay -S pycharm-professional
```

> Written with [StackEdit](https://stackedit.io/).
