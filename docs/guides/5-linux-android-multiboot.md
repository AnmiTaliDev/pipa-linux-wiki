# Multibooting Linux and Android

!!! warning
    All data on your tablet will be erased. Back up anything important before continuing.

    If you think you made a mistake, do not reboot the tablet. Ask for help in the [support group](https://t.me/pipa_mainline) first.

    Run the commands one by one, in order.

## What you need

- Xiaomi Pad 6 (pipa) with an unlocked bootloader.
- A computer with [`adb` and `fastboot`](https://developer.android.com/tools/releases/platform-tools) installed.
- A USB cable to connect the tablet to your computer.
- [`twrp.img`](https://t.me/pipa_mainline/27559), used to boot into TWRP temporarily.
- [`parted`](https://t.me/pipa_mainline/25847), used to manage the storage partition layout.
- A Linux image file for the distro you want to install.

## Before you start

1. Backup your important Android data.
2. Make sure Android can boot normally with USB debugging enabled.
3. Make sure `adb` and `fastboot` can detect the device.
4. Put `twrp.img`, `parted`, and the Linux image in the same folder on your computer for easier access.
5. Keep your [Fastboot Stock ROM](https://mifirm.net/model/pipa.ttt#cn-fastboot-stable) nearby in case you need to restore the device.


## Basic idea

This guide uses the tablet's A/B slot system to keep Android and Linux separate.

- Slot A is used for Android.
- Slot B is used for Linux.

With this setup, you can switch between Android and Linux by changing the active slot. Android stays on one slot, while Linux is installed and booted from the other slot.

To switch from Android to Linux, rooted Android users can use [Boot Control](https://github.com/capntrips/BootControl/releases) to change the active slot.

To switch from Linux back to Android, use:

```bash
qbootctl -s a
```

You can also use the [qbootctl GUI](https://github.com/khairul169/qbootctrl-gui).

## Step 1: Prepare Android

1. Make sure USB debugging is enabled.
2. Reboot into bootloader mode by holding **Volume Down + Power**.
3. Confirm the device is detected:

```bash
fastboot devices
```

## Step 2: Repartition storage

This step prepares space for Linux on the tablet storage.

1. Boot TWRP temporarily:

```bash
fastboot boot twrp.img
```

2. After TWRP boots, push `parted` to the tablet:

```bash
adb push parted /tmp/parted
```

3. Start an adb shell:

```bash
adb shell
```

4. Make `parted` executable:

```bash
chmod +x /tmp/parted
```

5. Use `parted` to check the current partition layout before making changes:

```bash
/tmp/parted /dev/block/sda print
```

6. Create the Linux partition using the layout you want to use.

!!! warning
    Double-check the partition numbers and sizes before applying any change. A wrong command can erase Android or make the tablet unbootable.

## Step 3: Install the Linux rootfs

Extract the distro rootfs into the Linux target location.

Example outline:

```bash
sudo mount /dev/YOUR_LINUX_PARTITION /mnt
sudo tar -xpf rootfs.tar -C /mnt
sync
sudo umount /mnt
```

Replace `/dev/YOUR_LINUX_PARTITION` and `rootfs.tar` with the actual device and file name.

## Step 4: Prepare kernel and boot files

You need a kernel that supports pipa hardware.

Prepare the required boot files:

- Kernel image
- Device tree
- Initramfs, if needed
- Kernel command line
- UEFI files, if using UEFI

Make sure the kernel command line points to the correct Linux root location.

Example root arguments:

```text
root=/dev/YOUR_LINUX_PARTITION rw
```

or, for image based setups:

```text
root=/dev/loop0 rw
```

## Step 5: Add a boot method

Choose one boot method and document the exact command here.

### Temporary boot with fastboot

This is useful for testing because it does not permanently flash the boot image.

```bash
fastboot boot linux-boot.img
```

### Flash Linux boot image

Only do this if you know how to restore the Android boot image.

```bash
fastboot flash boot linux-boot.img
```

### UEFI boot menu

If using UEFI, place the Linux boot entry in the EFI partition and select it from the boot menu.

Document the exact boot entry name here:

```text
Linux on pipa
```

## Step 6: First Linux boot

1. Boot Linux using your chosen method.
2. Wait for the first boot to finish.
3. Login with the default distro credentials.
4. Resize the filesystem if needed.
5. Configure Wi-Fi, users, timezone, and packages.

Useful first checks:

```bash
uname -a
lsblk
df -h
ip addr
```

## Step 7: Boot back to Android

Reboot the device and confirm Android still works.

If Android does not boot, restore the Android boot image or select the Android boot entry from UEFI.

Example:

```bash
fastboot flash boot android-boot.img
fastboot reboot
```

## Troubleshooting

### Device boots only Android

- Check that the Linux boot image was actually used.
- Check the UEFI boot order, if using UEFI.
- Check that the kernel command line points to the Linux rootfs.

### Linux kernel boots but rootfs is missing

- Check the root partition or image path.
- Check filesystem support in the kernel or initramfs.
- Check `root=` kernel argument.

### Android no longer boots

- Restore the original Android boot image.
- Reflash the Android ROM if needed.
- Do not erase user data unless you already have a backup or are prepared to lose it.

### Touchscreen, Wi-Fi, audio, or GPU does not work

- Check the kernel status page.
- Try a newer kernel build.
- Check whether the distro needs extra firmware packages.

## Notes

Add device-specific notes here:

- Android ROM version:
- Linux distro:
- Kernel build:
- Storage method:
- Boot method:
- Known working features:
- Known broken features:
