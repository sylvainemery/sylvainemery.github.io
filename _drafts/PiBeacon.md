---
layout: post
title: PiBeacon: Making an iBeacon from a Raspberry Pi
---

What I used
-----------

- a [Raspberry Pi](http://www.raspberrypi.org/). I had the B model in hand, but the A model should work just the same.
- an SD Card for the system. Any will do as long as it's at least 4GB.
- a Bluetooth 4.0 dongle. I chose the [Inatek BTA-CSR4B5](http://www.inateck.com/inateck-bta-csr4b5-usb-bluetooth-4-0-adapter/) because it's tiny, affordable and its chipset (CSR8510) is known to [work well with Linux](http://swiesmann.de/?p=36).


Installation
------------

### Install raspbian

Nothing fancy here, I downloaded the [official Wheezy Raspbian](http://www.raspberrypi.org/downloads), and wrote it on the SD Card.

Because I don't want to plug the raspi in my tv and I don't have a spare USB keyboard, I just use SSH from my laptop.


### install BlueZ stack

### up/down script : reverse engineered from small c program


Making it a service
-------------------

- service


Reference
---------

- [iBeacons - Dave Addey](http://daveaddey.com/?p=1252)
