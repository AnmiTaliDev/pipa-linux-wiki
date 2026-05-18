# Basics  

Things to know when starting out with using Linux on your pipa.


## Installing Linux  
Only use these instructions, if:
- The images you're trying to install don't come with installation instructions  
- You only have boot and userdata/root images

From fastboot:

#### Flash the boot image
``` bash
fastboot flash boot_ab boot.img
```
#### Flash the rootfs image
``` bash
fastboot flash userdata root.img
```
#### Clear the dtbo partition
``` bash
fastboot erase dtbo
```
#### Exit the bootloader
``` bash
fastboot reboot
```
