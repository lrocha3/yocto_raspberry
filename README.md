# yocto_raspberry

##Initialize the repo
1 - repo init -u https://github.com/lrocha3/yocto_raspberry.git

##Initialize poky bblayers
1 - cd poky
2 - source oe-init-build-env ../build
3 - go to build/conf/local.conf and change the machine to MACHINE ?= "raspberrypi2"
4 - go to build/bblayers.conf/ and add the following
4.1- BBLAYERS ?= " \
	  /home/luigi/raspberry_yocto/poky/meta \
	  /home/luigi/raspberry_yocto/poky/meta-poky \
	  /home/luigi/raspberry_yocto/poky/meta-yocto-bsp \
	  /home/luigi/raspberry_yocto/poky/meta-openembedded/meta-oe \
	  /home/luigi/raspberry_yocto/poky/meta-openembedded/meta-multimedia \
	  /home/luigi/raspberry_yocto/poky/meta-openembedded/meta-networking \
	  /home/luigi/raspberry_yocto/poky/meta-openembedded/meta-python \
	  /home/luigi/raspberry_yocto/poky/meta-raspberrypi \
	  "

##Bitbake the image
1 -bitbake -k core-image-minimal
2 - if any error we can cleansstate of a specific recipe
2.1 bitbake -c cleansstate RECIPE_NAME


##FLASH THE IMAGE IN RASPBERRY PI
1 - Discover the USB identifier:
2 - Before connecting the sd card clean all messages in dmesg:
2 - sudo dmesg -c
3 - Connect SDCard in Virtual Box (check the message):
4 - dmesg
5 - find the line that states: Attached SCSI removable disk --> (e.g, sdb)
6 - Go to ~/raspberry_yocto/poky/build. 
7 - Copy the image to the SDCard with dd:
7.1 - sudo umount /dev/sdb*
7.2 - sudo dd if=tmp/deploy/images/raspberrypi2/core-image-minimal-raspberrypi2.rpi-sdimg of=/dev/sdb bs=1M
7.3 - sudo umount /dev/sdb*
