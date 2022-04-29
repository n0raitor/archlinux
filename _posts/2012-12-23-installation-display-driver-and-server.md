---
title: 'Display Driver and Server'
category: Installation
layout: null
---

### XOrg
#### Print GPU Info
```bash
lspci -k | grep -A 2 -E "(VGA|3D)"
```

#### Install XORG
```bash
pacman -S --needed xorg
```

### video drivers
#### Proprietary
```bash
sudo pacman -S nvidia nvidia-lts nvidia-settings
# optional #
# virtualbox-guest-utils  # (For Virtualbox)
# nvidia-390xx  # (AUR) End of life
# nvidia-dkms  # (For custom kernel)
# nvidia-390xx-dkms  # (AUR) (for custom kernel) -> for my older laptop
```

#### Opensource
```bash
# xf86-video-*
# e.g. nouveau
```
#### ! Only use one Proprietary or Opensource Video Driver !
also check out this [link](https://wiki.archlinux.org/index.php/NVIDIA)