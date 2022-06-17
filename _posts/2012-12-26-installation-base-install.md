---
title: 'Base Install'
category: Installation
layout: null
---

## PreConfig

```bash
export EDITOR=nano
```

### Configure Partitions

I use *gdisk* for GPT Tables

```bash
lsblk  # Print Devices
gdisk /dev/<os-device>

n, 128, -200M, Enter, ef00  # Create EFI Partition
n, 1, Enter, Enter, 8e00  # Create Linux LVM Partition
w

# If you are using a separate home drive, use this to create an encrypted home partition (LVM, luks added beneath)
gdisk /dev/<home-device>
n, 1, Enter, Enter, 8e00  # Create Linux Filesystem Partition for the entire driveNow copy your Backups to the Home directory (sudo recommended)
```

#### Encrypt Root Partition

```bash
cryptsetup luksFormat /dev/<os-device>1  # Root Partition Device
Confirm and Enter Passphrase.

cryptsetup open /dev/<os-device>1 cryptlvm  # Open Created LVM Device
Enter Passphrase

pvcreate /dev/mapper/cryptlvm  # create Physical Volume

vgcreate vg1 /dev/mapper/cryptlvm  # create Volume Group

# For the Home Partition Encryption (if created above)
cryptsetup luksFormat /dev/<home-device>
cryptsetup open /dev/<home-device> crypt_home
```

#### Create Logical Volumes

Note: for *<size-of-ram>* use the amount of RAW + 1G for swap (e.g. 16 GB RAM => 17GB SWAP), if you wish to use hibernate-mode (load current session from RAM in SWAP on Suspend)

```bash
lvcreate -L <size-of-ram>G vg1 -n swap
lvcreate -l 100%FREE vg1 -n root
```

Check your progress with *lsblk*

#### Format Logical Volumes and Partitions

```bash
mkfs.fat -F32 /dev/<os-device>128
mkfs.ext4 /dev/vg1/root
mkswap /dev/vg1/swap

# If you are using the above created encrypted home partition
mkfs.ext4 /dev/mapper/crypt_home
```

##### Mount Partitions

```bash
mount /dev/vg1/root /mnt
mkdir /mnt/home
mkdir /mnt/boot
mount /dev/<os-device>128 /mnt/boot
swapon /dev/vg1/swap

# If you are using the above created encrypted home partition
mount /dev/mapper/crypt_home /home
```

### Create Local Mirror List

*Note: This is for Server in Germany, feel free to tweak this command with your location*

```bash
reflector --verbose --country 'Germany' -l 200 -p https --sort rate --save /etc/pacman.d/mirrorlist

# Alternative
nano /etc/pacman.d/mirrorlist
```

### Install Base Packages

```bash
pacstrap /mnt base base-devel linux-lts linux linux-headers linux-lts-headers  linux-firmware nano dhcpcd lvm2 reflector git  
```

====