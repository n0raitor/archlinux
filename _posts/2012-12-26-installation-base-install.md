---
title: 'Base Install'
category: Installation
layout: null
---

### (extra) If you want a gui install / config use archfi
If you are using this, you can skip until the following steps to *"Set up XFCE"*
```bash
pacman -S wget pacman-contrib
wget archfi.sf.net/archfi
sh archfi
```

## PreConfig
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
pacstrap /mnt base base-devel linux-lts linux linux-headers linux-lts-headers  linux-firmware nano vim dhcpcd lvm2 reflector  

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

====