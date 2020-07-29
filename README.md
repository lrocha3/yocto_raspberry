# Build a Linux Image for Raspberry Pi 2 using Yocto

## Initialize the repository
```
cd /home/luigi/Documents/
mkdir repo_yocto
cd repo_yocto
repo init -u https://github.com/lrocha3/yocto_raspberry.git
repo sync
```

## Source the environment and set the build folder
```
cd /home/luigi/Documents/repo_yocto/poky
source oe-init-build-env ../build
```

## Configure the local.conf
```
gedit /home/luigi/Documents/repo/build/conf/local.conf
Set the target:
  - MACHINE ?= "raspberrypi2"
Set the number of threads and parallel make parameters
  - BB_NUMBER_THREADS = "4"
  - PARALLEL_MAKE = "-j 8"
```

## Configure the bblayers.conf
```
gedit /home/luigi/Documents/repo/build/conf/bblayers.conf
Set the BBLAYERS:
BBLAYERS ?= " \
	/home/luigi/Documents/repo/poky/meta \
	/home/luigi/Documents/repo/poky/meta-poky \
	/home/luigi/Documents/repo/poky/meta-yocto-bsp \
	/home/luigi/Documents/repo/meta-openembedded/meta-oe \
	/home/luigi/Documents/repo/meta-openembedded/meta-multimedia \
	/home/luigi/Documents/repo/meta-openembedded/meta-networking \
	/home/luigi/Documents/repo/meta-openembedded/meta-python \
	/home/luigi/Documents/repo/meta-raspberrypi "
```

## Bitbake the image
```
bitbake -k core-image-minimal
```
Note: If any error we can cleansstate a specific recipe
```
bitbake -c cleansstate RECIPE_NAME
```

## Flash the image generated in Raspberry Pi 2
```
1. Discover the USB identifier:
   - Before connecting the sd card clean all messages in dmesg:
     - sudo dmesg -c
   - Connect SDCard in Virtual Box (check the message):
     - dmesg
     - Find the line that states: Attached SCSI removable disk --> (e.g, sdb)
2. cd /home/luigi/Documents/repo/build 
3. Copy the image to the SDCard with dd:
   - sudo umount /dev/sdb*
   - sudo dd if=tmp/deploy/images/raspberrypi2/core-image-minimal-raspberrypi2.rpi-sdimg of=/dev/sdb bs=1M
   - sudo umount /dev/sdb*
```
