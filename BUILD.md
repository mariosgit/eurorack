Environment Variables for build...
==================================

This is what mariosgit uses to build the firmwares.
Instead of changing makefiles, set the following env vars...

```
export TOOLCHAIN_PATH=/Volumes/OS_MBP_108/Applications/Arduino_1_8_1.app/Contents/Java/hardware/tools/arm/
export PGM_INTERFACE=stlink-v2-1

export PGM_SERIAL_PORT=/dev/cu.usbserial-A5XK3RJT
```

Programming via the serial might require root/wheel access to the dev.
```
sudo PGM_SERIAL_PORT=/dev/cu.usbserial-A5XK3RJT make -f clouds/makefile upload_combo_serial
```

```
export AVRLIB_TOOLS_PATH=/Volumes/OS_MBP_108/Applications/Arduino_1_6_0.app/Contents/Java/hardware/tools/avr/bin/
export AVRDUDE=$(AVRLIB_TOOLS_PATH)avrdude -C $(AVRLIB_TOOLS_PATH)../etc/avrdude.conf 
```
The latter is not really checked in the makefile (avrlib/makefile.mk:70)

Then follow instructions on webpage.