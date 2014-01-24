---
layout: post
title: PiBeacon: Making an iBeacon from a Raspberry Pi
---

When I first heard about iBeacons, I thought it would be a great idea to build one. It seemed doable for an ex-developer-would-like-to-code-again-but-doesn-t-have-a-lot-of-spare-time like me.

So I grabbed my Rapsberry Pi, found a couple of articles describing how to make an iBeacon out of it, but none would tell exactly what I wanted: a simple service launched at boot, easy enough to configure, with no complicated C code.

The [first article](http://developer.radiusnetworks.com/2013/10/09/how-to-make-an-ibeacon-out-of-a-raspberry-pi.html) I read is from Radius Networks. They seem to have developped a comprehensive platform enabling marketers to do a lot of location-based things in their apps. The post is almost exactly what I was looking for, except that for some reason their Bluetooth commands didn't work with my adapter.

In the [second article](http://www.ioncannon.net/programming/1603/turn-a-raspberry-pi-into-an-ibeacon/) from Carson McDonald, the iBeacon part is a C program. It worked instantly with my adapter, but I didn't want to dive into C code to maintain it, should I need/want to. So I kind of reverse-engineered the C program with a bluetooth debugger, to see exactly what commands were transmitted to the adapter.

Combining thes two articles, I've been able to determine the commands compatible with my adapter and put together a fully autonomous PiBeacon.

This is what worked for me. Everything is on my [github repo](https://github.com/sylvainemery/pibeacon).


## Shopping list

- a [Raspberry Pi](http://www.raspberrypi.org/). I had the B model in hand, but the A model should work just the same.
- an SD Card for the system. Any will do as long as it's at least 4GB.
- a Bluetooth 4.0 dongle. I chose the [Inatek BTA-CSR4B5](http://www.inateck.com/inateck-bta-csr4b5-usb-bluetooth-4-0-adapter/) because it's tiny, affordable and its chipset (CSR8510) is known to [work well with Linux](http://swiesmann.de/?p=36). You can find a list of tested Bluetooth adapters at [eLinux.org](http://elinux.org/RPi_USB_Bluetooth_adapters).


## Installation

### Install raspbian

Nothing fancy here, download the [official Wheezy Raspbian](http://www.raspberrypi.org/downloads), and write it on the SD Card.

Plug the Bluetooth dongle, boot the raspi and log in.

Because you don't want outdated software on your system, upgrade everything that needs to:

    sudo apt-get update && sudo apt-get upgrade -y

(Also want a fresh firmware/kernel ? See [rpi-update](https://github.com/Hexxeh/rpi-update))

### Install BlueZ stack

Go to the [Bluez site](http://www.bluez.org/) to know what the latest version is and replace it in the following commands on your raspi:

    wget https://www.kernel.org/pub/linux/bluetooth/bluez-5.13.tar.xz
    tar xvJf bluez-5.13.tar.xz
    cd bluez-5.13
    ./configure --disable-systemd --enable-library
    make
    make install


## Scripts

Now that our Raspi is fully installed, we can transform it in an iBeacon.

We only use programs that are part of the BlueZ stack: `hciconfig` and `hcitool`.

There are three parts:

- a start script, to start the iBeacon advertising
- a stop script, to stop advertising
- a config file, used by both scripts

### the config file

Since you could have several iBeacons in the same place, this file contains the values enabling us to identify an iBeacon.

Name it `ibeacon.conf`

	#all values must be in hex form separated by spaces between every two hex digits
	export BLUETOOTH_DEVICE=hci0
	export UUID="e2 c5 6d b5 df fb 48 d2 b0 60 d0 f5 a7 10 96 e0"
	export MAJOR="00 16"
	export MINOR="00 08"
	export POWER="c5"

You can change major/minor values at your linking.

### the start script

Name it `ibeacon_start`

	#!/bin/sh
	. ./ibeacon.conf
	echo "Launching virtual iBeacon..."
	sudo hciconfig $BLUETOOTH_DEVICE up
	sudo hciconfig $BLUETOOTH_DEVICE noleadv
	sudo hciconfig $BLUETOOTH_DEVICE leadv 0
	sudo hcitool -i hci0 cmd 0x08 0x0008 1e 02 01 1a 1a ff 4c 00 02 15 $UUID $MAJOR $MINOR $POWER 00
	echo "Complete"

### the stop script

Name it `ibeacon_stop`

	#!/bin/sh
	. ./ibeacon.conf
	echo "Disabling virtual iBeacon..."
	sudo hciconfig $BLUETOOTH_DEVICE noleadv
	echo "Complete"


## Test it

### start

### Android/iPhone apps

### stop


## Making it a service

Put this script in `ibeacon_install_service.sh`:

	#! /bin/bash
	# check root permissions
	if [[ $UID != 0 ]]; then
	  echo "Please start the script as root or sudo!"
	  exit 1
	fi

	apt-get install bluez -y

	sed "s:/home/pi/iBeacon:$(dirname $(readlink -f $0)):" > /etc/init.d/ibeacon << "EOF"
	#!/bin/bash
	PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:$PATH

	DESC="iBeacon Application Software"
	PIDFILE=/var/run/ibeacon.pid
	SCRIPTNAME=/etc/init.d/ibeacon

	case "$1" in
	  start)
	    printf "%-50s" "Starting ibeacon..."
	    cd /home/pi/iBeacon
	    ./ibeacon_start
	    ;;
	  stop)
	    printf "%-50s" "Stopping ibeacon..."
	    cd /home/pi/iBeacon
	    ./ibeacon_stop
	    ;;
	  restart)
	    $0 stop
	    $0 start
	    ;;
	  *)
	    echo "Usage: $0 {start|stop|restart}"
	    exit 1
	esac
	EOF

	chmod +x /etc/init.d/ibeacon

	update-rc.d -f ibeacon start 80 2 3 4 5 . stop 30 0 1 6 .

Now `sudo ibeacon_install_service.sh` to install the ibeacon service.

The ibeacon service starts automatically when the raspi boots. You can start/stop it manually by typing `sudo /etc/init.d/ibeacon start` and `sudo /etc/init.d/ibeacon stop`.

## Reference

- [iBeacons - Dave Addey](http://daveaddey.com/?p=1252)
