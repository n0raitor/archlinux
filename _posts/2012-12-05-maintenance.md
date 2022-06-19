---
title: 'Maintenance'
layout: null

---

### Setup CUPS

**Preconditions**

```bash
sudo pacman -S system-config-printer
```

**Install Your Brother Printer Driver**

```bash
yay -S <Printer Driver ID> (Search yay -Ss or in Pamac)  --  e.g. brother-dcpj315w
sudo lpadmin -p dcpj315w -v lpd://${printer-ip}/BINARY_P1

# opt-dep
yay -S brscan3

```

Finally:

* get IP-Addresse of your Printer: 
* Open CUPS web interface and use "search new Printer"
* Select your Network Printer and click "add"
* Follow the instructions and use the driver you just installed
* Use the "system-config-printer" application to remove the printer "dcpj315w"
* Print :)

NOTE: You may test this multiple times: Use the Test-Print Button to test the connectivity

### Activating NumLock on Bootup

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
 sudo pacman -S os-prober
```

```bash
 sudo pacman -S ntfs-3g  #opt: If target Os is Windows
```

**Mount Dir of OS**

```bash
sudo mkdir -p /mnt/os-mount
sudo mount /dev/<partition-of-os-to-dual-boot> /mnt/os-mount
```

**run os-prober**

```bash
os-prober
```

**recreate Grub Config**

```bash
 grub-mkconfig -o /boot/grub/grub.cfg
```

If a message appears: *Os-prober will not be executed*
Solution: Set the named constant to the default/grub file as False (don't forget to uncommand the line)

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

**GDM**
Like Gnome, GDM auto-scales by default

### AUR Checksum Skip

```bash
makepkg --skipchecksums -si
```