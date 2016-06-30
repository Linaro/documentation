h2. Install Instructions - CE AOSP RPB 15.10 - HiKey

This guide describes how to get started with the CE AOSP Reference Platform Build, release 15.10, for HiKey.

For more information about the HiKey development board, please check "https://www.96boards.org/products/ce/hikey/":https://www.96boards.org/products/ce/hikey/

h3. Image Components

The CE AOSP RPB 15.10 - HiKey build is composed of the following artifacts:

* Bootloader:
** ARM Trusted Firmware, EDK2/UEFI and Grub2
** For more information about the reference bootloader used by HiKey, please check "Reference-Bootloader-Hikey":https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey
** Pre-built files: "http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.10/bootloader":http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.10/bootloader
* Linux Kernel:
** Derived from Linux 3.18 kernel
** Git: "https://github.com/96boards/linux.git":https://github.com/96boards/linux.git
** Branch: *hikey*
* AOSP Android Marshmallow 6.0

h4. Closed source binaries

The following components requires a closed source binary for better hardware support:

* TI wlan firmware (@wl18xx@)
** Git: "http://git.ti.com/wilink8-wlan/wl18xx_fw":http://git.ti.com/wilink8-wlan/wl18xx_fw
** Branch: *R8.5*
* Extra firmware files available from firmware-linux
* Mali (requires EULA)

h3. Downloading the pre-built binaries

The build is composed by the traditional Android image files (@boot@, @cache@, @system@ and @userdata@), but to avoid incompatibilities issues with older bootloaders, or different partition tables, it's also recommended to flash the bootloader.

*Bootloader files:*

bc. wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.10/bootloader/l-loader.bin
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.10/bootloader/fip.bin
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.10/bootloader/ptable-aosp.img
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.10/bootloader/hisi-idt.py

*CE AOSP RPB image:*

bc. wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.10/boot_fat.uefi.img.tar.xz
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.10/cache.img.tar.xz
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.10/userdata.img.tar.xz

Since @system.img@ requires the user to accept an End User License Agreement covering the rights to download and use the proprietary Mali userspace driver, it needs to be manually downloaded via browser. Please go to "http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.10/system.img.tar.xz":http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.10/system.img.tar.xz and follow the instructions to download the file.

Uncompress the .tar.xz files using your operating system file manager, or with the following command, for each file:

bc. xz --decompress [filename].tar.xz; tar -xvf [filename].tar

h3. Flashing

h4. Bootloader

To flash the bootloader the recovery mode is required. For more information about the recovery mode, how to enable and use, please go to "https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey#enabling-recovery-mode":https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey#enabling-recovery-mode

On recovery mode, flash the bootloader with the following command:

bc. sudo python hisi-idt.py --img1=l-loader.bin -d /dev/ttyUSB0
sudo fastboot flash ptable ptable-aosp.img
sudo fastboot flash fastboot fip.bin

Change @ttyUSB0@ to the right interface name that gets exported to your host system.

Make sure to reboot the board after updating the partition table (@ptable-aosp.img@), otherwise flashing the system image might fail.

h4. Boot, System, Cache and Userdata

Fastboot is required to flash boot, system, cache and userdata.

Due bug "117 (UEFI fastboot uploads hangs for large images)":https://bugs.96boards.org/show_bug.cgi?id=117, it's recommended to flash the images via recovery mode (after running @hisi-idt.py@).

*Flashing boot, cache, system and userdata:*

bc. sudo fastboot flash boot boot_fat.uefi.img
sudo fastboot flash cache cache.img
sudo fastboot flash system system.img
sudo fastboot flash userdata userdata.img

Once flashed, make sure recovery mode is not enabled (pin3-pin4 on J15), then just reboot the board and enjoy :-)

h3. Additional resources

For known issues and more information about this release, please check "https://github.com/96boards/documentation/wiki/ReferenceSoftware":https://github.com/96boards/documentation/wiki/ReferencePlatform

