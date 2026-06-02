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
- [`parted`](https://t.me/pipa_mainline/27563), used to manage the storage partition layout.
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
sudo qbootctl -s a
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

    The TWRP image file may have a different name depending on who packaged it. In the linked download above, the file is named `boot-rmux.img`, so that is the name used in the example below.

    ```bash
    fastboot boot boot-rmux.img
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

5. Start `parted`:

    ```bash
    /tmp/parted /dev/block/sda
    ```

    Before this, your shell prompt will usually look like `pipa:/ #`. If the command works, you will enter the interactive `parted` prompt, so the prompt changes from the normal shell to `(parted)`.

    You should see something like this:

    ```text
    GNU Parted 3.2
    Using /dev/block/sda
    Welcome to GNU Parted! Type 'help' to view a list of commands.
    (parted)
    ```

6. Use `print` to check the current partition layout before making changes:

    ```bash
    print
    ```

    This prints the partition table for the internal storage. You will see the disk size at the top, followed by a list of partitions with columns such as `Number`, `Start`, `End`, `Size`, and `Name`.

    You are looking for the partition named `userdata`. On pipa, this is usually the last partition in the table, often partition `34`.

    Example:

    ```text
    Model: SKhynix HN8T15DEHKX075 (scsi)
    Disk /dev/block/sda: 253GB
    Sector size (logical/physical): 4096B/4096B
    Partition Table: gpt
    Disk Flags: 

    Number  Start   End     Size    File system  Name             Flags
    ...
    34      11.1GB  253GB   242GB                userdata
    ```

    In this example, `userdata` is partition `34`, and it uses most of the remaining internal storage.

    Compare the `End` value of `userdata` with the total disk size shown at the top. Here, the disk size is `253GB`, and the `userdata` partition also ends at `253GB`, which means the layout is still close to the default one.

    If your `userdata` partition ends much earlier than the total disk size, or if the partition number and layout look very different from the example, the tablet was likely repartitioned before. Double-check the partition table carefully before making any changes.

    !!! warning
        Double-check the partition numbers and sizes before applying any change. A wrong command can erase Android or make the tablet unbootable.

7. Delete the existing `userdata` partition first.

    On a mostly stock pipa layout, `userdata` is usually partition `34`, so the command is often:

    ```text
    rm 34
    ```

    You may see a warning like this:

    ```text
    Warning: Partition /dev/block/sda34 is being used. Are you sure you want to continue?
    Yes/No? yes
    Error: Partition(s) 34 on /dev/block/sda have been written, but we have been unable to inform the kernel of the change,
    probably because it/they are in use. As a result, the old partition(s) will remain in use.
    before making further changes.
    Ignore/Cancel? ignore
    ```

    Read the warning first and make sure you are deleting the correct partition. In this case, if you are sure that partition `34` is `userdata`, it is usually safe to answer `yes` and then `ignore`. This happens because the partition is still in use by the running environment, even though the partition table change itself was written.

8. Create two new partitions in the free space:

    - one partition for Android
    - one partition for Linux

    Choose a split point. This value is used twice: as the end of the Android `userdata` partition, and as the start of the Linux partition.

    Examples:

    - `173GB` gives Linux about `80GB`
    - `153GB` gives Linux about `100GB`

    Then create the Android `userdata` partition first:

    ```text
    mkpart userdata ext4 11.1GB 153GB
    ```

    Then create the Linux partition:

    ```text
    mkpart linux ext4 153GB 253GB
    ```

    After making new partitions, the result will look something like this:
    ```text
    Number  Start   End     Size    File system  Name             Flags
    ...
    34      11.1GB  153GB   142GB   ext4   
    35      153GB   253GB   100GB   ext4   
    ```

    !!! warning
        Run the `mkpart` commands one by one in the `(parted)` prompt. Double-check the start and end values before pressing Enter. A wrong value here can overlap partitions or leave Android without enough space.

9. Name the new partitions.

    On a mostly stock layout, the recreated Android partition will usually be `34`, and the new Linux partition will usually be `35`.

    First, name partition `34` as `userdata` for Android

    ```text
    name 34 userdata
    ```

    Then name partition `35` for Linux. You can use `linux` or another simple name if you prefer.

    ```text
    name 35 linux
    ```

    After naming the partitions, the result will look something like this:

    ```text
    Number  Start   End     Size    File system  Name             Flags
    ...
    34      11.1GB  153GB   142GB   ext4   userdata
    35      153GB   253GB   100GB   ext4   linux
    ```

10. Exit `parted`, then leave the adb shell.

    First, quit the `(parted)` prompt:

    ```text
    quit
    ```

    This will return you to the normal shell prompt, which usually looks like `pipa:/ #`.

    Then exit the shell:

    ```text
    exit
    ```

    Reboot the tablet:

    ```bash
    adb reboot
    ```

    Let the tablet boot back into Android to make sure the new partition layout is still usable before continuing.

## Step 3: Install Linux

Before continuing, choose a Linux distro for pipa. Some available projects include:

- [postmarketOS](https://wiki.postmarketos.org/wiki/Xiaomi_Pad_6_(xiaomi-pipa)) - pmOS for pipa
- [void-linux-pipa](https://github.com/userg0d/void-linux-pipa) - Another Void Linux for pipa
- [pipa-alarm](https://t.me/pipa_mainline/32978) - alarm (Arch Linux ARM) for pipa
- [armtix-xiaomi-pipa](https://github.com/Neo10e/armtix-xiaomi-pipa) - ARMtix (Artix Linux ARM) for pipa
- [pipa-fedora-builder-43](https://github.com/rr1111/pipa-fedora-builder-43#related-projects) - Another Fedora for pipa

Choose one and download it. Make sure the downloaded files include `boot.img` and `root.img`.

1. Power off the tablet, then boot it into bootloader mode by holding **Volume Down + Power**.

2. Make sure the tablet is detected in fastboot mode.

    ```bash
    fastboot devices
    ```

3. Flash the boot image to slot `b`.

    ```bash
    fastboot flash boot_b boot.img
    ```

4. Flash the rootfs image to the Linux partition.

    ```bash
    fastboot flash linux_partition root.img
    ```

    Replace `linux_partition` with the Linux partition name you created earlier in [Step 2](#step-2-repartition-storage), such as `linux` or another name you chose in poin `9`.

5. Clear the `dtbo` partition in slot `b`.

    ```bash
    fastboot erase dtbo_b
    ```

6. Reboot the tablet out of bootloader mode.

    ```bash
    fastboot reboot
    ```

    After this, the tablet should boot into Linux.
    Check the distro link you chose earlier for details about its desktop environment, first boot experience, and any distro-specific setup steps.
    Distros with a desktop environment should boot straight into it after the first startup. Otherwise, you will likely land in a text console and need to continue setup from there.

!!! note
    Linux installation is now complete. Once you have confirmed that Linux boots correctly, use the Android boot instructions from [Basic idea](#basic-idea) to switch back to slot `a`.
