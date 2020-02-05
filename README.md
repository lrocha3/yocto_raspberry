# yocto_raspberry

##Initialize the repo
1 - cd /home/luigi/Documents/
2 - mkdir repo
3 - cd repo
4 - Initialiye repo:
5 - repo init -u https://github.com/lrocha3/yocto_raspberry.git
6 - repo sync

##Initialize poky bblayers
1   - cd /home/luigi/Documents/repo/poky
2   - source oe-init-build-env ../build
3   - Open any File Explorer and navigate to /home/luigi/Documents/repo/build/conf
4   - change the local.conf and change the machine to:
4.1 - MACHINE ?= "raspberrypi2"
5   - change the bblayers.conf and add the following
4.1- BBLAYERS ?= " \
	/home/luigi/Documents/repo/poky/meta \
	/home/luigi/Documents/repo/poky/meta-poky \
	/home/luigi/Documents/repo/poky/meta-yocto-bsp \
	/home/luigi/Documents/repo/meta-openembedded/meta-oe \
	/home/luigi/Documents/repo/meta-openembedded/meta-multimedia \
	/home/luigi/Documents/repo/meta-openembedded/meta-networking \
	/home/luigi/Documents/repo/meta-openembedded/meta-python \
	/home/luigi/Documents/repo/meta-raspberrypi \
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
