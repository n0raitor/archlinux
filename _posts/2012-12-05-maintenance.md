---
title: 'Maintenance'
layout: null
---
### Setup CUPS
**Preconditions**
```bash
sudo pacman -S cups    # manjaro-printer (optional)
sudo systemctl enable cups
sudo systemctl start cups
sudo pacman -S system-config-printer
```
**Install Your Brother Printer Driver**
```bash
yay -S <Printer Driver ID> (Search yay -Ss or in Pamac)  --  e.g. brother-dcpj315w
sudo lpadmin -p dcpj315w -v lpd://${printer-ip}/BINARY_P1
```
Finally:
* get IP-Addresse of your Printer: 
* Open CUPS web interface and use "search new Printer"
* Select your Network Printer and click "add"
* Follow the instructions and use the driver you just installed
* Use the "system-config-printer" application to remove the printer "dcpj315w"
* Print :)

NOTE: You may test this multiple times: Use the Test-Print Button to test the connectivity

### Activate Numberpad on Bootup
[https://wiki.archlinux.org/title/Activating_numlock_on_bootup](https://wiki.archlinux.org/title/Activating_numlock_on_bootup)
**KDE Plasma**
Go to *System Settings > Input Devices > Keyboard*, in the *Hardware* tab, in the *NumLock on Plasma Startup* section, choose the desired NumLock behavior.

**GNOME**
Run the following command:
```bash
gsettings set org.gnome.desktop.peripherals.keyboard numlock-state true
```

In order to remember last state of numlock key (whether you disabled or enabled), use:

```bash
gsettings set org.gnome.desktop.peripherals.keyboard remember-numlock-state true
```
Note: The key *numlock-state* was moved from *org.gnome.settings-daemon.peripherals.keyboard* since GNOME 3.34
Alternatively, you can use add *numlockx* on (from numlockx to a startup script, like */.bashrc* (if using Bash) or */.profile.*

**Xfce**
In the file ~/.config/xfce4/xfconf/xfce-perchannel-xml/keyboards.xml, make sure the following values are set to true:
```
<property name="Numlock" type="bool" value="true"/>
<property name="RestoreNumlock" type="bool" value="true"/>
```
Note: If the file does not exist then open Settings > Keyboard, then check and uncheck the Restore num lock state on startup. This will create the *keyboards.xml* file.

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
```
 sudo pacman -S os-probe
```
```
 sudo pacman -S ntfs-3g  #opt: If target Os is Windows
```

**Mount Dir of OS**
 ```
 sudo mkdir -p /mnt/os-mount
 sudo mount /dev/<partition-of-os-to-dual-boot> /mnt/os-mount
```

**run os-prober**
```
os-prober
```

**recreate Grub Config**
```
 grub-mkconfig -o /boot/grub/grub.cfg
```

If a message appears: *Os-prober will not be executed*
Solution: Set the named constant to the default/grub file as False

### Extra

**Set Route with net-tools manually**
```bash
# e.g.
sudo route add -net 172.16.0.0/16 tun0
```

### Cleanup Tips

#### Clean PKG
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

#### Remove Orphans
```bash
sudo pacman -R $(pacman -Qtdq)
```

#### Clean Cache in /home
```bash
du -sh ~/.cache/    # Size of Cache
rm -rf ~/.cache/*
```

#### Remove old config files
-> ~/.config und ~/.local/share
Remove them manualy! (only uninstalled files recommended)

#### Find and remove duplicates, empty files, empty dirs, broken symlinks
```bash
sudo pacman -S rmlint
rmlint --help
rmlint /home/user     #Scans folder and saves special script to remove findings (read results)
```

#### Find the largest files and directories
```bash
sudo pacman -S ncdu
ncdu   # Run tool (command line)
```

#### Clean Trash (Remember)
You know how to do that ;)

### Resize Partition(s)
[https://wiki.ubuntuusers.de/Dateisystemgr%C3%B6%C3%9Fe_%C3%A4ndern/](Source)

Um die Größe von Dateisystemen ändern zu können, müssen je nach Dateisystem verschiedene Hilfsprogramme ("tools") installiert sein:
* e2fsprogs (liefert resize2fs)
* reiserfsprogs
* xfsprogs
* jfsutils
* ntfs-3g

#### Anpassen der Dateisysteme**
**ext2 / ext3 / ext4**
Um ein ext3-Dateisystem anzupassen, darf es nicht eingehängt oder fehlerhaft sein. Seit Linux Kernel 2.6.10 kann man ext3 (nicht ext2)-Dateisysteme auch im eingehängten Zustand vergrößern, nicht jedoch verkleinern! Es ist sinnvoll, jedoch nicht nötig, das Dateisystem zuvor mit "e2fsck" auf Fehler zu überprüfen.
```bash
sudo resize2fs -p /dev/gerätename   # Vergrößert das Dateisystem bis zur maximalen Größe des Logical Volumes oder der Partition
sudo resize2fs -M /dev/gerätename   # Verkleinert das Dateisystem bis zur minimalen Größe des Logical Volumes oder der Partition
sudo resize2fs -p /dev/gerätename 5G   # Vergrößert bzw. Verkleinert das Dateisystem auf 5 Gibibyte Gesamtgröße
sudo resize2fs -P /dev/gerätename   # Gibt die Minimalgröße an, wie weit das Dateisystem verkleinert werden kann
```
NOTE: Für Größenangaben (im Beispiel 5G) verwendet "resize2fs" den Teiler 1024, nicht 1000!
NOTE: Der im Beispiel verwendete Parameter -p von "resize2fs" dient dazu, einen Fortschrittsbalken beim Anpassen des Dateisystems anzuzeigen.

**NTFS**
Um ein NTFS-Dateisystem anzupassen, darf es nicht eingehängt oder fehlerhaft sein. Auf dem Dateisystem dürfen keine NTFS-verschlüsselten oder NTFS-komprimierten Dateien oder Ordner liegen, ein vorherige Defragmentierung ist ratsam, jedoch nicht erforderlich.
```bash
sudo ntfsresize /dev/gerätename   # Vergrößert das Dateisystem bis zur maximalen Größe des Logical Volumes oder der Partition
sudo ntfsresize -n --size 5G /dev/gerätename  # Vergrößert bzw. Verkleinert das Dateisystem auf 5 Gigabyte Gesamtgröße, Testlauf im Read-only-Modus (empfehlenswert!)
sudo ntfsresize --size 5G /dev/gerätename     # Vergrößert bzw. Verkleinert das Dateisystem auf 5 Gigabyte Gesamtgröße
sudo ntfsresize -i /dev/gerätename  # Zeigt an, auf welche Größe das Dateisystem minimal verkleinert werden kann 
```
NOTE: Der --size Parameter von "ntfsresize" verwendet den Teiler 1000 statt der üblichen 1024 beim Umrechnen von Gigabyte in Megabyte usw..., dies sollte beim Anpassen des Dateisystems beachtet werden! Bei der Verkleinerung ist darauf zu achten mindestens ca. 70 Mbyte über dem Minimalwert (Parameter -i) einzugeben!
NOTE: Um die NTFS-Kompression einzelner Dateien auf einem NTFS-Laufwerk als vorbereitende Maßnahme für ntfsresize abzustellen bzw. rückgängig zu machen, kann man entweder die Eigenschaften jeder einzelnen Datei manuell bearbeiten oder aber in der Windows-Eingabeaufforderung den folgenden Befehl zur Dekomprimierung aller betroffenen Datein nutzen:
```
compact /U /S:\ /I 
```

**FAT32**
Ein FAT32-Dateisystem kann mit dem Programm Parted angepasst werden. Zunächst muss das Dateisystem mit umount ausgehängt werden. Anschließend startet man Parted als root wie hier beschrieben. Mit dem Befehl
```
print
```
kann man sich einen Überblick über die Partitionen der Festplatte geben lassen. Um den Vergrößerungs- bzw- Verkleinerungsvorgang zu starten, schreibt man
```
resize [NR] [START] [ENDE] 
```
in die Befehlszeile von Parted, wobei [NR]für die Nummer der Partition laut der "print"-Ausgabe ist; [START] steht für die neue Startposition der Partition (z.B. 0 für den Anfang der Festplatte, 200MB als absolute Größe oder 10% als relative Größe), und [ENDE] bezeichnet die Endposition (ebenfalls als absolute oder relative Größe).


### Improve Performance

[https://wiki.archlinux.org/title/improving_performance](https://wiki.archlinux.org/title/improving_performance)

### HiDPI Support of Desktop Environments
[Source](https://archived.forum.manjaro.org/t/hidpi-support-in-manjaro/26088)
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

**KDE/Plasma**
Support for HiDPI in plasma is very good after a few adjustments. Sometimes gtk application will not scale quite as well on plasma as they do on other DEs but that is relatively rare.

* To install kde/plasma reference the appropriate Wiki page
* Go into "System Settings". Under "Display and Monitor" click "Scale Display". Drag the "Scale" slider to an appropriate amount. Log out and then back in
* If your cursor needs resizing you can do so in "System Settings". Under "Workspace Theme", select "Cursor Theme" and then adjust the size on the bottom right hand side
* Right click on the panel and go to panel options and then unlock widgets. Click on the handle for the panel and drag the "Height" box until the panel is an appropriate size
* If you would like to resize the items in the systray you do so by following these steps:
	* In your "Home" directory there is a hidden directory called ".config"
	* Edit the file "plasma-org.kde.plasma.desktop-appletsrc"
	* Search the file for "SystrayContainmentId" there will be a number after the "=" sign
	* Scroll down to "[Containments][8][General]" replacing "8" with your number from the previous step
	* If iconSize is not already in that section add a line that reads "iconSize=2". Replace 2 with your desired scale factor
	* Logout for the changes to take effect

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

**Budgie**
Budgie is a simple and intuitive desktop. It is currently a derivative of Gnome and as a result Budgie's scaling is very good. Budgie scales in 1x increments up to 3x.

* To install Budgie reference the appropriate Wiki page
* Budgie autoscales the interface by default. If it doesn't you can run the run the "dconf Editor" and go under org->gnome->desktop->interface->scaling-factor and change it manually
* To change the cursor size run the "dconf Editor" and go under org->gnome->desktop->interface->cursor-size

**Cinnamon**
Cinnamon has top notch support for HiDPI. It autoscales everything by default and pretty much "just works". Cinnamon scales in 1x increments up to 3x.

* To install Cinnamon reference the appropriate Wiki page 400
* The interface autoscales but if you want to change it manually you can in System Settings->General
* If you want to change the cursor size you can use dconf-editor
	* If dconf-editor is not installed, you can install it with: sudo pacman -S dconf-editor
	* Then run dconf-editor and browse to org->cinnamon->desktop->interface->cursor-size

**Deepin**
Deepin has added support for hidpi displays in recent versions. The scaling is manual but it is simple to use. Deepin scales in .25x increments up to 2x.

* To enable scaling adjust the slider in display settings

**Enlightenment**
HiDPI support in Enlightenment is usable but there are some definite issues. The window borders are very difficult to target and some UI elements do not scale.

* To install Enlightenment reference the appropriate Wiki page
* Open the file .Xresources in your home folder. Find the line that reads "Xft.dpi=" and change it to something suitable. This will adjust the font sizes in applications
* Logout and login to Enlightenment
* Enlightenment walks you through an initial setup process and one of the steps is to select the scaling factor. If you miss it you can find it later in the "Settings Panel" on the "Look" tab under "Scaling"
* Open the "Settings Panel". Under the "Input" tab, select the "Mouse" item and adjust the cursor size

**LXQT**
LXQT is the next generation of LXDE and in its current state is actually capable of decent support for HiDPI but it takes some work to get there.

* To install LXQT you need the following packages/groups:
	* lxqt - The lxqt install lxqt
	* menda-lxqt-panel - This installs a Manjaro LXQT theme and it is important because it is one of the few themes that handles font scaling without overlapping the icons and text in the application menu
	* To install both of the above from a terminal use: sudo pacman -S lxqt menda-lxqt-panel
* Open the file .Xresources in your home folder. Find the line that reads "Xft.dpi=" and change it to something suitable. This will adjust the font sizes Logout and login to LXQT
* After installation, LXQT has no icon theme selected by default so first we should fix that. Open the "LXQT Configuration Center". Open "Appearance" and select an "Icon Theme". While you are in there choose the Menda LXQT theme
* Right-click on the panel and select "Configure Panel". Change "Size" and "Icon Size" appropriately
* Open the "PCManFM File Manager". Under the Edit->Preferences menu item. Select "Display" and set all the sizes appropriately
* Openbox, the default LXQT window manager does not support HiDPI very well so you will need to download a specific HiDPI theme or use something else. kwin is a good alternative for LXQT.
* Option 1 - Install an Opnbox HiDPI theme. This is the easier solution but Openbox themes are fixed pixels so you need to find one with your dimensions in mind
	* If the theme is packages as an Openbox Theme Archive(obt) then open the "LXQT Configuration Center", go into "Openbox Settings" and select "Install a new theme"
	* If not, you will need to uncompress it and then place it in the ".themes" folder in your Home folder. If that folder does not exist then you need to create it. mkdir ~/.themes Then open the "LXQT Configuration Center", go into "Openbox Settings" and select the theme from the list
* Option 2 - Install kwin, the window manager from KDE
	* Use sudo pacman -S kwin systemsettings to install kwin and the "KDE System Settings" application used to configure it
	* Open the "LXQT Configuration Center" again and this time look under "Session Settings". Change the "Window Manager" to be kwin_x11
	* Logout and back in again to activate kwin as the window manager

**MATE**
MATE has added some support for hidpi displays. Scaling is automatic. hidpi is basically an on/off switch where it doesn't scale or scales at 2x. Application scaling works pretty well but I noticed a lot of layout issues with MATE panels and widgets when scaling was enabled.

* To enable scaling go into the "MATE Tweak" settings under "Window" set the "HIDPI" setting as desired.
* If 2x is too big or too small for your application some fine tuning can be achieved by changing the font resolution. In "Appearance Preferences" select the "Fonts" tab and then click "Details". You can then adjust the DPI to suit your preference.

### Window Managers
**SDDM**
SDDM has support for hidpi via manual or automatic scaling

To set SDDM to auto scale:

* Check for the presence of the file /etc/sddm.conf
* If you have an sddm.conf file then edit both the [X11] section and the [Wayland] section to include the following line:
EnableHiDPI=true
* If not, add a config file to /etc/sddm.conf.d with the following contents(create the directory if it does not exist):
[Wayland]
EnableHiDPI=true
[X11]
EnableHiDPI=true

To set SDDM to use a manual DPI setting:
* Check for the presence of the file /etc/sddm.conf
* If you have an sddm.conf file then look in the [X11] for the line that starts ServerArguments=and add -dpi X to the end of that line where X is your target DPI. For example, mine looks like this: ServerArguments=-nolisten tcp -dpi 192
* If you don't have sddm.conf then check the directory /etc/sddm.conf.d for any files that have the line starts ServerArguments=and add -dpi X to the end of that line where X is your target DPI. For example, mine looks like this: ServerArguments=-nolisten tcp -dpi 192
* If the /etc/sddm.conf.d doesn't exist or doesn't contain a file with the ServerArguments line then create it and add a file with the following contents:
[X11]
ServerArguments=-nolisten tcp -dpi 192
Replace '192' with an appropriate target DPI for your configuration

It is worth noting that in my testing enable auto-scaling scaled a 15.6" 4K display by a very small amount and I was forced to use the manual configuration to get good results.

**GDM**
Like Gnome, GDM auto-scales by default


### Alternatives ####
(lazy edition ;)Installation medium with installer https://archlinux.org/news/installation-medium-with-installer/