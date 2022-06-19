---
title: 'Final Steps'
layout: null
---

### Bash

*use the dotfiles!*

#### OR

```bash
sudo nano /etc/profile.d/editor.sh  # set default global editor
```

**Man Page - German Edition**

```bash
sudo pacman -S man-pages-de
alias man="LANG=de_DE.UTF-8 man"  # Write to ~/.bashrc or ~/.zshrc
```

**Most**

```bash
#Install Most for Manual Page highlighting

sudo pacman -S most
export PAGER=most

# See results on "man mv"
```

### Use Reflector as above

```bash
reflector --verbose --country 'Germany' -l 200 -p https --sort rate --save /etc/pacman.d/mirrorlist
```

###### Generate SSH-Key

```bash
ssh-keygen -t rsa -b 4096 -o -a 100
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
sudo pacman -S openvpn networkmanager-openvpn
sudo systemctl restart NetworkManager
```

**OpenConnect**

```bash
sudo pacman -S openconnect networkmanager-openconnect
sudo systemctl restart NetworkManager
```

**Thumbnailer**

```bash
# Lightweight video thumbnailer that can be used by file managers.
sudo pacman -S ffmpegthumbnailer  

```

**System Monitoring**

```bash
sudo pacman -S neofetch
sudo pacman -S nano-syntax-highlighting
sudo pacman -S hwinfo
sudo pacman -S gnome-disk-utility gparted
sudo pacman -S glances
sudo pacman -S htop
yay -S nvidia-system-monitor-git

```

**Setup Useful Shortcuts**

- CTRL + ALT + T: Terminal
- CTRL + ALT + E: File Manager
- CTRL + ALT + ESC: xkill
