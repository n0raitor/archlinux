---
title: 'Maintenance'
layout: null
---

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

# For Full Pamac
yay -S libpamac-full
```

**Set Route with net-tools manually**
```bash
# e.g.
sudo route add -net 172.16.0.0/16 tun0
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

### Clean Trash (Remember)
You know how to do that ;)


## Improve Performance

https://wiki.archlinux.org/title/improving_performance

## HiDPI Support of Desktop Environments
[Source](https://github.com/normannator/archlinux/issues/source)
**Gnome**
HiDPI Support in Gnome is excellent. It auto-scales by default and is completely usable out of the box

To install gnome reference the appropriate Wiki page 679
Gnome autoscales the interface by default. If it doesn't you can run the run the "dconf Editor" and go under org->gnome->desktop->interface->scaling-factor and change it manually
To change the cursor size run the "dconf Editor" and go under org->gnome->desktop->interface->cursor-size

**XFCE**
With a decent amount of tweaking XFCE is usable on HiDPI screens. A few elements won't be scaled properly and some things don't layout ideally such as the settings menu but overall it is workable

To install XFCE reference the appropriate Wiki page 299
Under "Settings", open "Appearance". Select the "Fonts" tab and set an appropriate DPI setting
Under "Settings", open "Window Manager". Select the "Style" tab and choose a HiDPI style. There is one called "Default-hidpi" if you are having trouble finding one
Open "File Manager"(Thunar) and change the zoom to an appropriate level in the "View" menu
Under "Settings", open "Panel"
On the "Display" tab, drag the "Row Size" slider to an appropriate value
On the "Items" tab select "Whisker Menu" and then click on the edit button. Set the icon sizes on the "Appearance" tab to your liking
Stay on the "Items" tab and select "Notifications Area" and then click on the edit button. Change "Maximum Icon Size" to an appropriate value
If you end up with two rows of window buttons and you find them too difficult to see or target there are two different workarounds
You can reduce the "Row Size" until it falls to a single row
Alternatively, on the "Items" tab select "Window Buttons" and then click on the edit button. Uncheck "Show Button Labels". This will give you full size icons
Under "Settings", open "Desktop" and select the "Icons" tab. Change the "Icon Size" to something suitable

**LXDE**
HiDPI support in LXDE is usable after some tweaking.

To install LXDE reference the appropriate Wiki page 38
Open the file .Xresources in your home folder. Find the line that reads "Xft.dpi=" and change it to something suitable. This will adjust the font sizes
Logout and login to LXDE
Right click on the panel and select "Panel Preferences"
In the "Geometry" tab, adjust the "Height" and "Icon Size" to your liking
In the "Panel Applets" tab select "Task Bar" and click "Preferences" to adjust the width of the task buttons
Open the "PCManFM File Manager". Open the Edit->Preferences menu item. Under "Display" set all the sizes appropriately
Openbox, the default LXDE window manager in Manjaro does not support HiDPI very well so you will need to download a specific HiDPI theme or use something else. Manjaro includes HiDPI themes for the xfce window manager or you could install kwin
Option 1 - Install an Opnbox HiDPI theme. This is the easiest solution but Openbox themes are fixed pixels so you need to find one with your dimensions in mind
If the theme is packages as an Openbox Theme Archive(obt) then open the "LXQT Configuration Center", go into "Openbox Settings" and select "Install a new theme"
If not, you will need to uncompress it and then place it in the ".themes" folder in your Home folder. If that folder does not exist then you need to create it. mkdir ~/.themes Then open the "LXQT Configuration Center", go into "Openbox Settings" and select the theme from the list
Option 2 - Install xfwm4, the window manager from XFCE. Manjaro includes some HiDPI themes for XFCE and these themes are often a good match for the default LXDE theme
Install the following packages:
xfwm4 - The window manager
xfwm4-themes - Themes for xfwm
xfce4-settings-manager - The settings manager so you can adjust the themes and other window manager settings
To install these all at once you can use: sudo pacman -S xfwm4 xfwm4-themes xfce4-settings-manager
Open the "Desktop Session Settings" application. On the "Advanced Options" tab, change the "Window Manager" to be xfwm4
Logout and back in again to activate xfwm4 as the window manager
Run xfce4-settings-manager from a terminal or the "Run" box. Select "Window Manager" and choose a HiDPI theme from the list
Option 3 - Install kwin, the window manager from KDE
Use sudo pacman -S kwin systemsettings to install kwin and the "KDE System Settings" application used to configure it
Open the "Desktop Session Settings" application. On the "Advanced Options" tab, change the "Window Manager" to be kwin_x11
Logout and back in again to activate kwin as the window manager
