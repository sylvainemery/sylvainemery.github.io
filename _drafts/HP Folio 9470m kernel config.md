# X

Verify that evdev is activated
```
Device Drivers --->
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
   			[*] Enable legacy fbdev support for the modesettting intel driver
```


# webcam

In the kernel, choose v4l2 and usb cam /!\ need more explaination


# make.conf

The HP Folio 9470m has an Intel Core i7-3687U CPU. It's an IvyBridge and as of gcc-4.7, its `march` is `core-avx-i`.
Modify the following info in `/etc/portage/make.conf`:

```
CHOST="x86_64-pc-linux-gnu"
CFLAGS="-march=core-avx-i -O2 -pipe"
CXXFLAGS="${CFLAGS}"

MAKEOPTS="-j3" # 2 cores

VIDEO_CARDS="intel i915" # despite being an IvyBridge, the video card is still an i915, not an i965
INPUT_DEVICES="synaptics evdev"
ALSA_CARDS="hda_intel"
```
