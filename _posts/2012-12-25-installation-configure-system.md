---
title: 'Configure System'
category: Installation
layout: null

---

*Note: Again, this is for Germany and German, feel free to use your timezone / language / locale informations* 

### Gererate fstab

```bash
genfstab -U /mnt >> /mnt/etc/fstab  # (alt. change -U to -L to use Label instead of UUID
cat /mnt/etc/fstab  # check gen
```

#### If you are using the encrypted home partition

```bash
lsblk -f  # Note down the UUID of your home-partition (not the mapper!)
```

```bash
# Edit this file and insert the following row
nano /etc/crypttab  
```

```
# crypt_home     UUID=<UUID enter here>    none                    luks,timeout=180
```

```bash
cp /etc/fstab /etc/fstab.bak  # Backup Your FSTAB File
nano /etc/fstab  # Edit this file
```

```
### File content ###
# <file system> <dir> <type> <options> <dump> <pass>

# /dev/mapper/vg1-root
UUID=...                /        ext4    rw,relatime     0 1

# /dev/<home-device>
/dev/mapper/crypt_home    /home    ext4    rw,relatime     0 2

# /dev/sdb128
UUID=...                /boot    vfat    rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=ascii,shortname=mixed,utf8,errors=remount-ro   0 2

# /dev/mapper/vg1-swap
UUID=...                none    swap    defaults        0 0
```

### Switch to Arch-Chroot

```bash
arch-chroot /mnt  # Change to Root Directory
```

### Set Computer Hostname

```bash
echo myhost > /etc/hostname
```

### Set Keyboard Layout

```bash
echo KEYMAP=de-latin1 > /etc/vconsole.conf
```

### Set Font (optional)

```bash
echo FONT=lat9w-16 >> /etc/vconsole.conf
```

### Set Locale

```bash
echo LANG=de_DE.UTF-8 > /etc/locale.conf  # Set Language
nano /etc/locale.gen
```

-> uncomment this:

```
#de_DE.UTF-8 UTF-8
#de_DE ISO-8859-1
#de_DE@euro ISO-8859-15
#en_US.UTF-8
```

```bash
locale-gen
```

### Set Time

```bash
ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime  # Set Timezone
hwclock --systohc --utc # Sync Hardware-Clock - Run to generate /etc/adjtime
```

### Set Root Password

```bash
passwd root
```

### Edit Hosts file

```bash
cat /etc/hosts  # set Hosts file
```

Add:

```
# <ip-address>    <hostname.domain.org>        <hostname>
127.0.0.1         localhost.localdomain        localhost
::1               localhost.localdomain        localhost
127.0.0.1         <hostname>.localdomain       <hostname>
```

### Edit Mirror List

```bash
reflector --verbose --country 'Germany' -l 200 -p https --sort rate --save /etc/pacman.d/mirrorlist
```

Alternative:

```bash
nano /etc/pacman.d/mirrorlist
```

### Edit mkinitcpio.conf and generate

```bash
nano /etc/mkinitcpio.conf
```

On Hooks: Add these words, separated with a space, between *block* and *filesystem*:

* encrypt
* lvm2

#### Generate mkinitcpio

Run both for bleeding edge and lts kernel support / integration

```bash
mkinitcpio -p linux-lts
mkinitcpio -p linux
```

### Bootloader

```bash
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```

#### Configure Root Partition Decryption on Startup

```bash
nano /etc/default/grub
```

1. Remove the *#* in front of *GRUB_ENABLE_CRYPTODISK=y*
2. On *GRUB_CMDLINE_LINUX_DEFAULT* add this line between *="* and *log.level=3 quiet*:
   cryptdevice=/dev/device-with-luks:cryptlvm:allow-discards 
   * Replace *device-with-luks* with the root device id (e.g. sda1, check *lsblk*)

#### Retaining boot messages

If you wish to get more detailed boot messages on startup, configure this:

```bash
nano /etc/systemd/system/getty.target.wants/getty@tty1.service 


# Set
[Service]
TTYVTDisallocate=no
```

Fix Quiet Log in Grub Config:

```bash
nano /etc/default/grub

# Remove the "quiet" on GRUB_CMDLINE_LINUX_DEFAULT
```

#### Generate Grub Configuration

```bash
grub-mkconfig -o /boot/grub/grub.cfg  # Generate Grub config file
```
