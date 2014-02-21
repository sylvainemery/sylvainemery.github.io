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
	- if you didn't make attention and want to get (e.g.) the french layout, type `loadkeys fr` once you are in the shell


# Configure Wifi

Note: only possible if wpa_supplicant is included in your gentoo LiveCD

To verify that you have wpa_supplicant, type `/etc/init.d/wpa_supplicant start`

If you get this kind of error:

	 * Starting WPA Supplicant Daemon
	Successfully initialized wpa_supplicant
	Failed to open config file 'etc/wpa_supplicant/wpa_supplicant.conf', error: No such file or directory
	Failed to read or parse configuration '/etc/wpa_supplicant/wpa_supplicant.conf'.
	 * start-stop-daemon: failed to start '/usr/sbin/wpa_supplicant'
	 * Failed to start wpa_supplicant
	 * ERROR: wpa_supplicant failed to start

It indicates that wpa_supplicant is installed and that we need to provide it with a valid config file.

Taking info from the [gentoo handbook](http://www.gentoo.org/doc/en/handbook/handbook-x86.xml?part=4&chap=4), edit the following file `/etc/wpa_supplicant/wpa_supplicant.conf` :

	ctrl_interface=/var/run/wpa_supplicant
	ctrl_interface_group=0
	ap_scan=1

	# Simple case: WPA-PSK, PSK as an ASCII passphrase, allow all valid ciphers
	network={
	  ssid="simple"
	  psk="very secret passphrase"
	}

Now retype `/etc/init.d/wpa_supplicant start` and you should see:

	 * Starting WPA Supplicant Daemon
	Successfully initialized wpa_supplicant	[ok]

An `iwconfig` should show that you are connected to your SSID, and an `ifconfig` should show your IP address.

To verify that you really have Internet access, type `ping -c 3 www.google.com` to see the pings.

/*
# Prepare to ssh

By booting on this LiveCD, you are logged in automatically as root, but with a scrambled and unknown password. You won't be able to ssh into this system without knowing your password.

So, type `passwd` to change it to your liking.

Now you can start the SSH daemon:

	/etc/init.d/sshd start

From now on, you'll be doing everything from an SSH session in an another PC
*/

# Partition the disk

	cfdisk /dev/sda

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

Format the partitions:

	mkfs.ext2 /dev/sda1
	mkfs.ext4 /dev/sda3

Init and activate the swap:

	mkswap /dev/sda2
	swapon /dev/sda2

Now mount the partitions:

	mount /dev/sda3 /mnt/gentoo
	mkdir /mnt/gentoo/boot
	mount /dev/sda1 /mnt/gentoo/boot

All set, you'll now start to configure your final gentoo system.


# Download stage3

You can get the current stage3 here: http://distfiles.gentoo.org/releases/amd64/autobuilds/current-stage3-amd64/

Note the URL for the stage3-amd64-YYYMMDD.tar.bz2 file

Tip: You are better off **not** choosing a nomultilib file if you want maximum compatibility.

Tip: You can choose a [closer mirror](http://www.gentoo.org/main/en/mirrors2.xml) to download the stage3.

	cd /mnt/gentoo
	wget URL_of_your_stage3_file
	tar xvjpf stage3-*.tar.bz2

# Download portage

The latest portage snapshot is found here: http://distfiles.gentoo.org/snapshots/portage-latest.tar.bz2

	wget http://distfiles.gentoo.org/snapshots/portage-latest.tar.bz2
	tar -xvjf portage-latest.tar.bz2 -C /mnt/gentoo/usr

