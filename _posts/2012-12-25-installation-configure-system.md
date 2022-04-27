---
title: 'Configure System'
category: Installation
layout: null
---
### Gererate fstab
```bash
genfstab -U /mnt >> /mnt/etc/fstab  # (alt. change -U to -L to use Label instead of UUID
cat /mnt/etc/fstab  # check gen
```

### Switch to Arch-Chroot
```bash
arch-chroot /mnt  # Change to Root Directory
```

### Set Computer Hostname
```
echo myhost > /etc/hostname
```

### Set Keyboard Layout
```
echo KEYMAP=de-latin1 > /etc/vconsole.conf
```

### Set Font (optional)
```
echo FONT=lat9w-16 >> /etc/vconsole.conf
```

### Set Locale
```
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
```
locale-gen
```

### Set Time
```
ln -sf /usr/share/zoneinfo/Europe/Berlin /etc/localtime  # Set Timezone
hwclock --systohc --utc # Sync Hardware-Clock
```

### Set Root Password
```
passwd root
```

### Edit Hosts file
```bash
cat /etc/hosts  # set Hosts file
```
Add:
```
#<ip-address>	<hostname.domain.org>	<hostname>
127.0.0.1		localhost.localdomain	localhost
::1				localhost.localdomain	localhost
127.0.0.1		<hostname>.localdomain	<hostname>
```

### Edit Mirror List
```bash
reflector --verbose --country 'Germany' -l 200 -p https --sort rate --save /etc/pacman.d/mirrorlist

# Alternative
nano /etc/pacman.d/mirrorlist
```

### Edit mkinitcpio.conf and generate
```bash
nano /etc/mkinitcpio.conf
```
Add between *block* and *filesystem*:
* encrypt
* lvm2

#### Generate mkinitcpio
```
mkinitcpio -p linux-lts
mkinitcpio -p linux
```

### Bootloader
```bash
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```
#### Method 1.
blkid  # BlockID
-> Copy or note down UUID of root partition (encrypted)
```
nano /etc/default/grub
```
--> Edit:
...
```
GRUB_CMDLINE_LINUX="cryptdevice=UUID="<UUID-copied>":cryptlvm root=/dev/vg1/root"
```

#### Method 2.
``` 
nano /etc/default/grub
```
1. Remove the *#* in front of *GRUB_ENABLE_CRYPTODISK=y*
2. On *GRUB_CMDLINE_LINUX_DEFAULT* add this line between *"=* and *log.level=3 quiet*:
cryptdevice=/dev/<device-with-luks>:<volgroupe-id>:allow-discards 

#### Generate Grub Configuration
```
grub-mkconfig -o /boot/grub/grub.cfg  # Generate Grub config file
```
