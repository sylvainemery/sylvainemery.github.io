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
```


# webcam

In the kernel, choose v4l2 and usb cam /!\ need more explaination


