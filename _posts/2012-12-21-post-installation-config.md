---
title: 'Post Installation Config'
layout: null
---
###Bash
```bash
/etc/profile.d/editor.sh  # set default global editor
nano /etc/profile.d/alias.sh
```
use the the folowing rows:
``` 
alias <left>="<right">
```

The (n) notation means, that there are n ways of signing this alias. Do not use this (n) e.g. (1) in the alias notation!

Edit the ~/.bashrc file to add alias permanently for your user.

[https://github.com/n0raitor/archlinux-alias](My ArchLinux Alias Repo) 


###Firewall
```bash
sudo pacman -S ufw gufw
sudo ufw enable
sudo ufw status verbose
sudo systemctl enable ufw.service
```

### Systemd

#### Enable installed services
Enable or Disable Services
```bash
systemctl enable NetworkManager
systemctl enable cups.service # previously installed
systemctl enable sshd.service # ssh Service
```
#### Set up important Services
```bash
pacman -S acpid dbus avahi
```
Enable the Services:
```
systemctl enable acpid
systemctl enable avahi-daemon
```

#### Timedatectl  (optional)
The hardware clock can be queried and set with the _timedatectl_ command
```bash
# Enable  # timedatectl set-ntp true
# Edit  # /etc/systemd/timesyncd.conf
systemctl enable --now systemd-timesyncd.service  # Sync System Time with atomic clock
```

### Highlighting
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

### Install other optional Packages
```bash
sudo pacman -S bash-completion
sudo pacman -S xdg-utils xdg-user-dirs 
```