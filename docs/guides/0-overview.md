# Basics  

Things to know when starting out with using Linux on your pipa.
!!! warning "Use a UNIX-like OS"
	Fastboot is terminally broken on Windows.  
	If you have issues flashing, be sure to use a device running Linux or MacOS. (Potentially other UNIXoids?)

## Unlocking the Bootloader
This Wiki will not go into details of unlocking the Bootloader as the hurdles and insanities you have to go through are well known at this point.  

For this step only, you will need a Machine running Windows, or a VM/Container with (very!) reliable USB passthrough.  

For up-to-date and accurate information, visit: [the Bootloader Unlock Wall of Shame](https://github.com/zenfyrdev/bootloader-unlock-wall-of-
shame/blob/main/brands/xiaomi/README.md).

## Installing the latest Firmware
Before installing Linux on your pipa, you should first flash the latest official HyperOS Firmware for your region's device variant. If you're on HyperOS 1 or MIUI, unlock the bootloader first as this makes it significantly easier.  

You can get the latest Firmware directly from Xiaomi's servers on [miuirom.org](https://miuirom.org/tablets/xiaomi-pad-6).  
Be sure to download the Fastboot ROM for your device.  

??? note "Getting your device region:"
	Check the Model Number on the back of your device, it should be:  

	|Region|Model Number|
	|--------------------|--------------------|
	|Global/EEA|23043RP34G|
	|China|23043RP34C|
	|India|23043RP34I|
	If you have a China model on HyperOS 2 you cannot unlock without first rolling back to HyperOS 1/MIUI using EDL!


!!! note "In the extracted Firmware folder, with pipa in Fastboot:"
	``` bash
	./flash_all.sh
	```
	DO NOT run `flash_all_lock.sh` as this will relock your Bootloader.

## Installing Linux  
??? warning "Always use the specific instruction that come with the Images you want to install!"
	If there are none and you are provided with a boot image (likely named boot.img) and a root image (likely named root.img, rootfs.img or userdata.img), you can try these generic instructions:  
	Flash the boot image:
	```bash
	fastboot flash boot_ab <boot image>
	```
	Flash the rootfs image:
	```bash
	fastboot flash userdata <root image>
	```
	Clear the dtbo partition:
	```bash
	fastboot erase dtbo	
	```
	Exit fastboot and reboot:
	```bash
	fastboot reboot
	```


