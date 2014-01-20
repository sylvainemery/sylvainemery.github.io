---
layout: post
title: PiBeacon: Making an iBeacon from a Raspberry Pi
---

When I first heard about iBeacons, I thought it would be a great idea to make one. It seemed doable for an ex-developer-would-like-to-code-again-but-doesn-t-have-a-lot-of-spare-time like me.

So I grabbed my Rapsberry Pi, found a couple of articles describing how to build an iBeacon out of it, but none would tell exactly what I wanted: a simple service launched at boot, easy enough to configure, with no complicated C code.

The [first article](http://developer.radiusnetworks.com/2013/10/09/how-to-make-an-ibeacon-out-of-a-raspberry-pi.html) I read is from Radius Networks. They seem to have developped a comprehensive platform enabling marketers to do a lot of location-based things in their apps. The post is almost exactly what I was looking for, except that for some reason their Bluetooth commands didn't work with my adapter.

In the [second article](http://www.ioncannon.net/programming/1603/turn-a-raspberry-pi-into-an-ibeacon/) from Carson McDonald, the iBeacon part is a C program. It worked instantly with my adapter, but I didn't want to dive into C code to maintain it, should I need/want to. So I kind of reverse-engineered the C program with a bluetooth debugger, to see exactly what commands were transmitted to the adapter.

Combining thes two articles, I've been able to determine the commands compatible with my adapter and put together a fully autonomous PiBeacon.

This is what worked for me.


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


### up/down script : reverse engineered from small c program


## Test it

Android or iPhone


## Making it a service


- service


## Reference

- [iBeacons - Dave Addey](http://daveaddey.com/?p=1252)
