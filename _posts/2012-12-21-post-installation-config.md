---
title: 'Post Installation Config'
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
# Setup OpenVPN
sudo pacman -S openvpn networkmanager-openvpn
systemctl restart networkmanager
# import openvpn config with:
openvpn import --config <config-incl-path>
nmcli connection import type openvpn file <file-to-ovpn>
```