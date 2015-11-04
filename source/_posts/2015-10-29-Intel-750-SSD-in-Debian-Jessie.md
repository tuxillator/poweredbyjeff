title: Installing and booting to Intel 750 SSD in Debian 8
date: 2015-10-29 21:39:36
tags:
- linux
- serveradmin
- kernel
- intel
---

The [Intel 750 series flash storage](http://www.intel.com/content/www/us/en/solid-state-drives/solid-state-drives-750-series.html) is a beast, without doubt one of the top tier consumer storage platforms on the market today. This can deliver up to 2500 MB/s reads... for comparison the best SSDs on the market today are limited to ~550 MB/s due to the SATA interface.

## Installing
To use this storage in Debian Jessie, a little black magic is involved particularly if you want to use it as a boot drive.
<!-- more -->
- Boot to the Debian net installer, choose Advanced options... graphical expert install.
- Go through setup, ensure backports are enabled when configuring package manager. Leave anything you're not sure of at defaults. **Stop before installing boot loader, it will fail (see next step).**
- [NVME support](https://communities.intel.com/community/itpeernetwork/blog/2014/10/03/upgrade-a-nvme-capable-linux-kernel) is much better from 3.19 kernels, but Debian 8 only ships with 3.16. Additionally the release version of grub2 has an install bug where it doesn't properly recognise NVMe devices. Let's pull a more recent kernel and grub-efi from jessie-backports to resolve - choose "execute a shell" and type:
```
cd /target
mount -t proc proc proc/
mount --rbind /sys sys/
mount --rbind /dev dev/
mount --rbind /run run/
chroot . /bin/bash

```
Standard stuff... chrooted to the install, we can now run commmands like we had actually booted.
- Grab the backported kernel and grub2 UEFI bootloader - remember to install the kernel matching your architecture:
```
apt-get update
apt-get -t jessie-backports install linux-image-amd64
apt-get -t jessie-backports install grub-efi
update-grub
grub-install
```
If all went to plan you've got a > 3.19 kernel and grub installed a UEFI bootloader in the ESP partition.
- Type `exit` to return to the graphical installer. Choose "continue without bootloader" as we installed it via the shell, and finish the install. After it reboots you should be looking at your shiny and super fast Debian install. A quick check if you're getting decent speed from the card:
```
sudo hdparm -Tt --direct /dev/nvme0n1
```
Enjoy!