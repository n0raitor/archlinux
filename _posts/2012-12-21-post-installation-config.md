---
title: 'Post Installation Config'
layout: null
---


#### Bash
```bash
/etc/profile.d/editor.sh  # set default global editor
nano /etc/profile.d/alias.sh
```
use the the folowing rows:
``` alias <left>="<right">```
The (n) notation means, that there are n ways of signing this alias. Do not use this (n) e.g. (1) in the alias notation!
![enter image description here](https://normannator.de/archlinux/IMG/Alias.PNG)

Edit the ~/.bashrc file to add alias permanently for your user.

Example Alias Config Section:
```bash
# Main ALIAS
alias ls="ls --color=auto"
alias l="ls --color=auto -lA --time-style long-iso"
alias ll="ls --color=auto -la --time-style long-iso"
alias cd..="cd .."
alias ..="cd .."
alias ...="cd ../../"
alias ....="cd ../../../"
alias .....="cd ../../../../"
alias ff="find / -name"
alias f="find . -name"
alias grep="grep --color=auto"
alias egrep="egrep --color=auto"
alias fgrep="fgrep --color=auto"
alias ip="alias ip='ip -c'"
alias pacman="pacman --color auto"
alias pactree="pactree --color"
alias vdir="vdir --color=auto"
alias watch="watch --color"
alias man="color function"
alias yay="sudo -u norman yay"
alias browser="midori &"

# Kali Undercover Mode
alias panic="kali-undercover"
alias ku="kali-undercover"
alias qwer="kali-undercover"

# CUSTOM Additional ALIAS
alias pgadmin="sudo /usr/pgadmin4/bin/pgadmin4"
alias haskel="ghci"
alias cyberghost="sudo cyberghostvpn --traffic sudo cyberghostvpn --traffic --connect --country-code"
alias cyberghostlist="cyberghostvpn --traffic --country-code"
alias spotifys="spotify --force-device-scale-factor=2.0 %U"
alias vi="vim"

# German Man Page
alias man="LANG=de_DE.UTF-8 man"

# Integrate Most in Man
export PAGER=most
```

**Optional**
```bash
nano /etc/profile.d/ps1.sh  # (optional)
# Minimal				/home #
# User					user:/home #
# User and Hostname		user@hostname:/home #

# Now update .bashrc  # (optional)
```

#### Firewall (optional)
```bash
# edit IPv4			/etc/iptables/iptables.rules
# edit IPv6			/etc/iptables/ip6tables.rules

# load Rules
# iptables-restore & ip6tables-restore

# Start At Boot
# systemctl enable iptables & systemctl enable ip6tables

# Generate Default Rules
# /etc/iptables/iptables.rules & /etc/iptables/ip6tables.rules
```
**!!! For a simple Firewall !!!**
```bash
sudo pacman -S ufw
sudo ufw enable
sudo ufw status verbose
sudo systemctl enable ufw.service
```

#### Systemd

##### Enable installed services
```bash
Enable or Disable Services
systemctl enable NetworkManager
systemctl enable cups.service # previously installed
systemctl enable sshd.service # ssh Service
```
##### Set up important Services
```bash
pacman -S acpid dbus avahi

systemctl enable acpid
systemctl enable avahi-daemon
```

##### Timedatectl  (optional)
The hardware clock can be queried and set with the _timedatectl_ command
```bash
# Enable  # timedatectl set-ntp true
# Edit  # /etc/systemd/timesyncd.conf
systemctl enable --now systemd-timesyncd.service  # Sync System Time with atomic clock
```

#### Boot (optional and redundant -> done above)
```bash
### initcpio
# edit /etc/mkinitcpio.conf
# mkinitcpio -p linux(-lts)
```


### Install other optional Packages
```bash
pacman -S  wireless_tools wpa_supplicant  mtools  linux-<lts->headers git bash-completion jre8/11-openjdk
xdg-utils  # Home Directories

pacman -S  gst-libav gst-plugins-good  # Multimedia graph framework
pacman -S icedtea-web  # Additional components for OpenJDK - Browser plug-in and Web Start implementation (optional)
```
