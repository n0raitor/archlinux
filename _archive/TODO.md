To Insert

*pkgstats*

[https://pkgstats.archlinux.de/](https://pkgstats.archlinux.de/)

*pacman-contrib*

```bash
sudo pacman -S pacman-contrib
```

This package contains contributed scripts to pacman.

Scripts:

- **checkupdates** - print a list of pending updates without touching the system
  sync databases (for safety on rolling release distributions).

- **paccache** - a flexible package cache cleaning utility that allows greater
  control over which packages are removed.

- **pacdiff** - a simple pacnew/pacsave updater for /etc/.

- **paclist** - list all packages installed from a given repository. Useful for seeing
  which packages you may have installed from the testing repository,
  for instance.

- **paclog-pkglist** - lists currently installs packages based pacman's log.

- **pacscripts** - tries to print out the {pre,post}_{install,remove,upgrade}
  scripts of a given package.

- **pacsearch** - a colorized search combining both -Ss and -Qs output. Installed
  packages are easily identified with a `[installed]`, and
  local-only packages are also listed.

- **rankmirrors** - ranks pacman mirrors by their connection and opening speed.

- **updpkgsums** - performs an in-place update of the checksums in a PKGBUILD.
