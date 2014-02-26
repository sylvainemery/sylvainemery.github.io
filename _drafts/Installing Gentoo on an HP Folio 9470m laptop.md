---
layout: post
title: Installing Gentoo on an HP Folio 9470m laptop
---

# Prepare the USB drive

- get the latest minimal install CD ISO from http://distfiles.gentoo.org/releases/amd64/autobuilds/current-iso/
- put it on an usb drive with the help of [UNetbootin](http://unetbootin.sourceforge.net/)


# Boot on the LiveCD *well, Live USB drive*

- boot on the USB drive (don't forget to strike Esc at BIOS and then F9 to choose our boot medium)
- UNetbootin has a loader that allows you to choose what to boot, choose `gentoo`
- you briefly have the option to change your keyboard layout, type the number associated with the layout of your laptop before the default one (US) is loaded

Tip: if the default was loaded before you could change it, and want to get (e.g.) the french layout, type `loadkeys fr` once you are in the shell.


# Configure Wifi

Note: only possible if wpa_supplicant is included in your gentoo LiveCD

To verify that you have wpa_supplicant, type `/etc/init.d/wpa_supplicant start`

If you get this kind of error:
```
 * Starting WPA Supplicant Daemon
Successfully initialized wpa_supplicant
Failed to open config file 'etc/wpa_supplicant/wpa_supplicant.conf', error: No such file or directory
Failed to read or parse configuration '/etc/wpa_supplicant/wpa_supplicant.conf'.
 * start-stop-daemon: failed to start '/usr/sbin/wpa_supplicant'
 * Failed to start wpa_supplicant
 * ERROR: wpa_supplicant failed to start
```
It indicates that wpa_supplicant is installed and that we need to provide it with a valid config file.

Taking info from the [gentoo handbook](http://www.gentoo.org/doc/en/handbook/handbook-x86.xml?part=4&chap=4), edit the following file `/etc/wpa_supplicant/wpa_supplicant.conf`:

```
ctrl_interface=/var/run/wpa_supplicant
ctrl_interface_group=0
ap_scan=1

# Simple case: WPA-PSK, PSK as an ASCII passphrase, allow all valid ciphers
network={
  ssid="simple"
  psk="very secret passphrase"
}
```

Now retype `/etc/init.d/wpa_supplicant start` and you should see:

```
 * Starting WPA Supplicant Daemon
Successfully initialized wpa_supplicant	[ok]
```

An `iwconfig` should show that you are connected to your SSID, and an `ifconfig` should show your IP address.

To verify that you really have Internet access, type `ping -c 3 www.google.com` to see the pings.


# Partition the disk

To enter the partition utility, type :

```
cfdisk /dev/sda
```

- First, delete all existing partitions, should you have any.
- Create the first partition, it will be named `/dev/sda1` and be used as the boot partition:
	- Primary
	- Size: 512 MB
	- Add partition at beginning of free space
	- Make this partition Bootable
- Create the second partition, it will be named `/dev/sda2` and will be the swap:
	- Primary
	- Size: equal to the size of your RAM (e.g. 8192 MB)
	- Add partition at beginning of free space
	- Change the type to be `Linux swap / Solaris` (82)
- Create the last partition, it will be named `/dev/sda3` and contain your system:
	- Primary
	- Size: the rest, let the default size cfdisk gives you
	- Add partition at beginning of free space
- Write the partition table to disk
- Quit


# Init the partitions

- Format the partitions

```
mkfs.ext2 /dev/sda1
mkfs.ext4 /dev/sda3
```

- Init and activate the swap

```
mkswap /dev/sda2
swapon /dev/sda2
```

- Now mount the partitions

```
mount /dev/sda3 /mnt/gentoo
mkdir /mnt/gentoo/boot
mount /dev/sda1 /mnt/gentoo/boot
```

All set, you'll now start to configure your final gentoo system.


# Download stage3

You can get the current stage3 here: http://distfiles.gentoo.org/releases/amd64/autobuilds/current-stage3-amd64/

Note the URL for the `stage3-amd64-YYYMMDD.tar.bz2` file

Tip: You are better off **not** choosing a nomultilib file if you want maximum compatibility.

Tip: You can choose a [closer mirror](http://www.gentoo.org/main/en/mirrors2.xml) to download the stage3.

```
cd /mnt/gentoo
wget URL_of_your_stage3_file
tar xvjpf stage3-*.tar.bz2
```

# Download portage

The latest portage snapshot is found here: http://distfiles.gentoo.org/snapshots/portage-latest.tar.bz2

```
wget http://distfiles.gentoo.org/snapshots/portage-latest.tar.bz2
tar -xvjf portage-latest.tar.bz2 -C /mnt/gentoo/usr
```


# Prepare your gentoo system

Before you chroot to your system, you need to prepare it first:

- copy the DNS info

```
cp -L /etc/resolv.conf /mnt/gentoo/etc/
```

- mount the filesystems

```
mount -t proc proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
```

- now chroot in your new environment

```
chroot /mnt/gentoo /bin/bash
```

- adjust a few settings

```
env-update && source /etc/profile
export PS1="(chroot) $PS1"
```

*From now on, everything you do is in your final system, not on the LiveCD.*


# Portage profile

The portage profile aims to pre-fill USE flags. Since this is a laptop and you want some graphical UI, choose the desktop profile.
First, type `eselect profile list` to see the list of available profiles.
Then type `eselect profile set X` where X is the desktop profile.


# make.conf

The HP Folio 9470m has an Intel Core i7-3687U CPU. It's an IvyBridge and as of gcc-4.7, its `march` is `core-avx-i`.
So here is the content of `/etc/portage/make.conf`:

```
CHOST="x86_64-pc-linux-gnu"
CFLAGS="-march=core-avx-i -O2 -pipe" # IvyBridge
CXXFLAGS="${CFLAGS}"

MAKEOPTS="-j3" # 2 cores

USE="bindist mmx sse sse2 -ipv6"

PORTDIR="/usr/portage"
DISTDIR="${PORTDIR}/distfiles"
PKGDIR="${PORTDIR}/packages"

VIDEO_CARDS="intel i965"
INPUT_DEVICES="synaptics evdev"

EMERGE_DEFAULT_OPTS="--keep-going=y"
```

If you need to know which CFLAGS you should use with a different processor, you can go to the [gentoo wiki](http://wiki.gentoo.org/wiki/Safe_CFLAGS).


# Locales

You need to specify which locales you want on your system.
Here is the content of `/etc/locale.gen` enabling the use of US english and french locales:

```
en_US ISO-8859-1
en_US.UTF-8 UTF-8
fr_FR ISO-8859-1
fr_FR@euro ISO-8859-15
fr_FR.UTF-8 UTF-8
```

You then have to run `locale-gen` to generate all the specified locales.

Finaly, edit your `/etc/env.d/02locale` file to match your preferences. Here is mine, all french, apart from messages:

```
LANG="fr_FR.UTF-8"
LC_CTYPE="fr_FR.UTF-8"
LC_NUMERIC="fr_FR.UTF-8"
LC_TIME="fr_FR.UTF-8"
LC_COLLATE="fr_FR.UTF-8"
LC_MONETARY="fr_FR.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_PAPER="fr_FR.UTF-8"
LC_NAME="fr_FR.UTF-8"
LC_ADDRESS="fr_FR.UTF-8"
LC_TELEPHONE="fr_FR.UTF-8"
LC_MEASUREMENT="fr_FR.UTF-8"
LC_IDENTIFICATION="fr_FR.UTF-8"
LC_ALL=""
```

Don't forget to set your timezone. Type these commands for Paris, France:

```
echo "Europe/Paris" > /etc/timezone
emerge --config sys-libs/timezone-data
```

If you live somewhere else, check `/usr/share/zoneinfo` for a list of all the supported timezones.


# Build your kernel

*You know you want to! That's why Linux exists, after all!*

There is a very involving process where you can choose every single piece of code that goes into your kernel, and there is a simple one, where you don't have to decide anything.
This is what you'll choose here, and because the `genkernel` tool takes its config from what was detected at the LiveCD boot, your kernel will still be optimized for your laptop.

- First, grab the kernel sources and the `genkernel` util

```
emerge -q gentoo-sources genkernel
```

- Then copy the config used during the LiveCD boot to the file used by `genkernel`

```
zcat /proc/config.gz > /usr/share/genkernel/arch/x86_64/kernel-config
```

- You can now launch the kernel building

```
genkernel all
```

If all succeeds, you'll see 2 files in `/boot`: `kernel*` and `initramfs*`


# Configure the filesystem

Your partitions ar listed in `/etc/fstab`. Gentoo provides a default file that is not valid and must be modified.
Based on the partitions we made earlier, here is what it should look like:

```
/dev/sda1   /boot        ext2    defaults,noatime     0 2
/dev/sda2   none         swap    sw                   0 0
/dev/sda3   /            ext4    noatime,discard      0 1

shm         /dev/shm     tmpfs   nodev,nosuid         0 0
```

Note the `discard` option for our root partition: since it's on an SSD and the fs is ext4, this will enable the TRIM command.
Also: there is no cdrom drive on this laptop, so no need to write an entry in the fstab.


# Configure the network

First, define your hostname in `/etc/conf.d/hostname`

While you are at it, modify your `/etc/hosts` to fill your host name:

```
127.0.0.1    *your_host_name* localhost
```

No, we will install and configure wpa_supplicant to enable wireless networking.

- install wpa_supplicant (with some other wireless utilities) by typing `emerge net-wireless/wpa_supplicant net-wireless/wireless-tools net-wireless/iw`

- as we did previously, edit the `/etc/wpa_supplicant/wpa_supplicant.conf` file

```
ctrl_interface=/var/run/wpa_supplicant
ctrl_interface_group=0
ap_scan=1

# Simple case: WPA-PSK, PSK as an ASCII passphrase, allow all valid ciphers
network={
  ssid="simple"
  psk="very secret passphrase"
}
```

- now edit the `/etc/conf.d/net` file

```
config_wlo1="dhcp" # wireless interface for this particular laptop
config_enp0s25="dhcp" # lan interface for this particular laptop
```

- and to automatically start networking at boot, type the following commands

```
cd /etc/init.d
ln -s net.lo net.wlo1
ln -s net.lo net.enp0s25
rc-update add net.wlo1 default
rc-update add net.enp0s25 default
```

- for linux to be able to start the wireless interface, you need to install a microcode for it. This laptop needs the `sys-firmware/iwl6005-ucode` ebuild, but doing a straight emerge won't do because it is not considered as stable (as of the time of writing). So you need to add a line to `/etc/portage/package.accept_keywords` before the emerge

```
echo "sys-firmware/iwl6005-ucode" >> /etc/portage/package.accept_keywords
?? dispatch_conf
emerge sys-firmware/iwl6005-ucode
```

- finally, don't forget to install a dhcp client by typing `emerge dhcpdc`


# Configure keymap and hwclock

Some misc config now:

- edit the `/etc/conf.d/keymaps` file to reflect your keyboard
- edit the `/etc/conf.d/hwclock` file and set it to "UTC" "or "local" depending on your BIOS setting. If you came from or dual-boot with Windows, it will be "local".


# Change the root password

Just type `passwd`. This is the root password. You know what it means.


# Install some necessary system tools

- `metalog` will be your system logger. To install it, type `emerge metalog && rc-update add metalog default`.
- the cron daemon will be `cronie`. Type `emerge cronie && rc-update add cronie default`
- to run SSH at boot: `rc-update add sshd default`
- install some useful utils: `emerge gentoolkit portage-utils iproute2`


# ACPI (power button, fans, battery...)

Install the daemon:: `emerge acpid`
Then start it: `/etc/init.d/acpid start`
And add it at boot: `rc-update add acpid default`


# Install a bootloader

- install GRUB2: `emerge sys-boot/grub`
- now install the necessary GRUB2 files in your boot disk: `grub2-install /dev/sda`
- generate the GRUB2 configuration based on genkernel: `grub2-mkconfig -o /boot/grub/grub.cfg`

# Reboot

Before you can reboot, you have to do some cleaning:

- type `exit` to exit the chroot
- get back to root by typing `cd`
- delete the stage3 and portage archives: `rm /mnt/gentoo/stage3-* && rm /mnt/gentoo/portage-latest*` 
- unmount all the partitions

```
umount -l /mnt/gentoo/dev{/shm,/pts,}
umount -l /mnt/gentoo{/boot,/proc,/sys,}
```

- and now, unplug your USB drive and type `reboot`


# Deeply update your system

Why would you need to update your system since you just installed it from a recent stage3?
You would be surprised to see how many packages are outdated even after a fresh install.

```
emerge --update --deep --with-bdeps=y --newuse @world
```

Now that your system is updated, you can remove obsolete and unused packages

```
emerge --depclean
```


# Install X

(re)Configure your kernel
```
genkernel all --menuconfig
```

Verify that evdev is activated
```
Device dDrivers --->
	Input device support --->
	<*> Event interface
```

Disable Framebuffer drivers
```
Device Drivers --->
	Graphics support --->
		-*- Support for frame buffer devices --->
			## unset everything
		Console display driver support --->
			-*- Framebuffer Console support
```

Setup Intel video card
```
Device Drivers --->
	Graphics support --->
		<*> /dev/agpgart (AGP Support) --->
			## unset everything other than:
			<*> Intel 440LX/BX/GX, I8xx and E7x05 chipset support
		<*> Direct Rendering Manager (XFree86 4.1.0 and higher DRI support) --->
			<*> Intel 8xx/9xx/G3x/G4x/HD Graphics
   			[*] Enable modesetting on intel by default
```

When you have finished the configuration, save it and exit menuconfig. genkernel will compile the new kernel and initramfs.

Reboot when it's done.


Install the drivers
```
emerge xorg-drivers --autounmask-write
dispatch-conf
emerge xorg-drivers
```

Install the X11 server

```
echo "x11-base/xorg-server udev" >> /etc/portage/package.use
emerge xorg-server
```

Install i3

```
emerge x11-wm/i3
```

Install a terminal

```
emerge xterm
```
