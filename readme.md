Repository moved to [codeberg.org/randogoth/bluefin-icons.git](http://codeberg.org/randogoth/bluefin-icons.git)

# Auto GRUB Class Setup for Bluefin

## Introduction

GRUB themes display icons according to each entry's class. On some rpm-ostree systems, including Bluefin, new boot entries are created without a class, which means no icon is shown in the menu. 

This setup fixes that by adding a dedicated Bluefin icon to your active GRUB theme and installing a helper script that ensures every BootLoaderSpec (BLS) entry includes `grub_class bluefin`. With this in place, GRUB automatically picks up the correct icon and your Bluefin deployments appear with their own distinctive symbol in the boot menu.

(Developed and tested on `bluefin-dx:latest-42.20251012.1`)

## Acknowledgements

This setup borrows the icon for the Bluefin GRUB [theme created by Raindrac](https://github.com/Raindrac/Banner/) and is inspired by and meant as a replacement for a previous [solution by Tomasz Drozdz](https://codeberg.org/TomaszDrozdz/Fedora-Silverblue-rpm-ostree-generate-grub-theme-icon-class).

## Installation

If you don't have a GRUB theme installed, [check these out](https://www.gnome-look.org/browse?cat=109&ord=latest). I am currently using [this one](https://www.gnome-look.org/p/2227016). The installer expects `/boot/grub2/user.cfg` to contain a `set theme=/boot/grub2/themes/<theme>/<file>.txt` entry so it can locate the theme's `icons` folder.

1) Run the installer from the repo root (it will sudo itself):

```bash
./install.sh
```

What it does:
- Reads your active theme path from `/boot/grub2/user.cfg` and copies `bluefin.png` into its `icons/` directory.
- Installs `bls-add-bluefin.sh` to `/usr/local/sbin/` and the rpm-ostree drop-in to `/etc/systemd/system/ostree-finalize-staged.service.d/bluefin-icons.conf`.
- Reloads systemd and immediately applies `grub_class bluefin` to existing BLS entries so icons show up right away.

2) (Optional) Verify:

```bash
grep -n '^grub_class' /boot/loader/entries/ostree*.conf
```
