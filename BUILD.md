# Environment Variables for build...

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



# Using WSL2 and docker on Windoof

Need old ubuntu because of python issues etc... 
 * could try to use ubuntu-14 in wsl ??? needs a tar? [any distro](https://learn.microsoft.com/en-us/windows/wsl/use-custom-distro) Yeah.. they get the tar from a running docker uhh ahh pfff. To stressful.
 * could try to fix issues ???

Start with my development dir mapped to /DevelSelf. This contains clones of
 * eurorack
 * mutable-dev-environment

In the mutable-dev-environment/0_... you can comment out
 * Install some additional drivers, including support for FTDI dongles

and change
 * openocd to version 0.12.0 (5 times)

and add missing packages...
 * ```sudo apt-get -y --force-yes python-serial wget``` somethere
 * also changed ```apt-get -y``` into ```apt-get -y --force-yes ``` for reasons.

Then change into the docker, run ```0_.sh```

```
sudo docker run -ti -v /mnt/c/Users/mario/DevelSelf:/DevelSelf  ubuntu:trusty-20151021 /bin/bash
mkdir /home/vagrant
cd /DevelSelf
cd mutable-dev-environment
./0_install-toolchain.sh
```

Save container state... Before exiting the docker, use a second terminal!
```
sudo docker ps
sudo docker commit -p 1636 mutabledevenv
```

## Run programmer with /dev/ttyUSB0...

I wanted to use the STM32CubeProgrammer tool to burn the firmware but you might be able to use the command line tools (openocd) as well. This requires access to USB devices in the docker.

WSL can pass USB devices to the linux kernel, and docker can allow access as well, great!

Please check the details there: ```github.com/dorssel/usbipd-win``` you need to install the [msi from that page](https://github.com/dorssel/usbipd-win/tags) and follow the [wiki](https://github.com/dorssel/usbipd-win/wiki/WSL-support) instructions.

Then done, go into the docker like this:
```
sudo docker run -ti --device=/dev/ttyUSB0  -v /mnt/c/Users/mario/DevelSelf:/DevelSelf mutabledevenv /bin/bash
export PGM_SERIAL_PORT=/dev/ttyUSB0
make -f warps/makefile upload_combo_serial
```
