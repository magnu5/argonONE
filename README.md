# argonONE
Standalone daemon for the [Argon40 One Pi 4 case](https://www.argon40.com/argon-one-raspberry-pi-4-case.html)

This release is messy and carries the 'It works for me!' label. Use
at your own risk.

Note that this daemon will do two things:
* Turn on and speed up the fan when the CPU temperature goes up
* Watch the power button and do a clean shutdown on a double click

Holding the power button for 3 seconds or longer will do a hard (i.e. unclean) shutdown and can cause filesystem corruption. This is done in hardware and the daemon is unable to respond to it.

## Background 
The ArgonONE case comes with a Python script to respond to powerbutton presses
and to set the fan speed depending on the CPU temperature. The Python dependency
is not always easy to fix, so I recreated the functionailty in a C program.

These sources are compiled with 'musl' on a Raspbian distro. The result is
a ~25kb static executable that has no dependencies on any libraries. It should
run on basically any Pi4 distro.

## Installation on LibreElec (tested on nightly)
download all the files and place them in /storage/.config (no sub-folder)

The daemon has the following requirements:

`ls -l /dev/vcio /dev/i2c-1 /sys/class/gpio/export /sbin/shutdown`

You can get the i2c-1 device by editing the config.txt file:
```
mount -o remount,rw /flash
nano /flash/config.txt
# Enable I2C device
dtparam=i2c_arm=on
```

Install the daemon, make sure it starts at boot:
```
cd /storage/.config/

chmod a+x argonONE
```

Place the argonONE.service file in /storage/.config/system.d/
```
Then enable the service and start it:
```
systemctl daemon-reload

systemctl enable argonONE

systemctl start argonONE
```
