Environment Variables for build...
==================================

This is what mariosgit uses to build the firmwares.
Instead of changing makefiles, set the following env vars...

From the doc...
> If you want to set up your own environment to build Elements’ code, an ARM EABI toolchain must be installed. Because of tight CPU and code size limits, we recommend you to use the same compiler version as we do: 4.8-2013-q4-major. Various pre-compiled binaries and source packages are available [here](https://launchpad.net/gcc-arm-embedded/4.8/4.8-2013-q4-major/) .

```
export TOOLCHAIN_PATH=/Volumes/OS_MBP_108/Applications//Arduino_1_6_0.app/Contents/Java/hardware/tools/gcc-arm-none-eabi-4.8.3-2014q1/
# export TOOLCHAIN_PATH=/Volumes/OS_MBP_108/Applications/Arduino_1_8_1.app/Contents/Java/hardware/tools/arm/
export PGM_INTERFACE=stlink-v2-1
export PGM_SERIAL_PORT=/dev/tty.usbserial-A5XK3RJT
````

```
export AVRLIB_TOOLS_PATH=/Volumes/OS_MBP_108/Applications/Arduino_1_6_0.app/Contents/Java/hardware/tools/avr/bin/
export AVRDUDE=$(AVRLIB_TOOLS_PATH)avrdude -C $(AVRLIB_TOOLS_PATH)../etc/avrdude.conf 
```
The latter is not really checked in the makefile (avrlib/makefile.mk:70)

Then follow instructions on webpage.