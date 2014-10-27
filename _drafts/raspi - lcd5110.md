raspi - lcd5110

prerequisite:
The program we will use require spidev to be activated. The kernel module should then be activated. To do so, comment the line blacklist spi-bcm2708 by adding a heading # in the file /etc/modprobe.d/raspi-blacklist.conf then reboot the Raspberry Pi to activate this module.


First, install wiringpi2

	git clone git://git.drogon.net/wiringPi
	cd wiringPi
	./build

try it :
	gpio readall

!! different raspi revisions -> different gpio results but wiringpi acts as a middleware so you don't have to deal with this



Now, install the python binding of wiringpi:

	sudo apt-get install python-dev python-imaging python-imaging-tk python-pip
	sudo pip install wiringpi2

Finally install spidev python library:

sudo pip install spidev
