---
layout: post
title: PiBeacon: Making an iBeacon from a Raspberry Pi
---

Hardware
--------

- a [Raspberry Pi](http://www.raspberrypi.org/). I had the B model in hand, but the A model should work just the same.
- an SD Card for the system. Any will do as long as it's at least 4GB.
- a Bluetooth 4.0 dongle. I chose the [Inatek BTA-CSR4B5](http://www.inateck.com/inateck-bta-csr4b5-usb-bluetooth-4-0-adapter/) because it's tiny, affordable and its chipset (CSR8510) is known to [work well with Linux](http://swiesmann.de/?p=36). You can find a list of tested Bluetooth adapters at [eLinux.org](http://elinux.org/RPi_USB_Bluetooth_adapters).


Installation
------------

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


### up/down script : reverse engineered from small c program


Making it a service
-------------------

- service


Reference
---------

- [iBeacons - Dave Addey](http://daveaddey.com/?p=1252)
