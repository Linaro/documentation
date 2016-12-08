
h2. Install Instructions - CE AOSP RPB 15.12 - HiKey

This guide describes how to get started with the CE AOSP Reference Platform Build, release 15.12, for HiKey.

For more information about the HiKey development board, please check "https://www.96boards.org/products/ce/hikey/":https://www.96boards.org/products/ce/hikey/

h3. Image Components

The CE AOSP RPB 15.12 - HiKey build is composed of the following artifacts:

* Bootloader:
** ARM Trusted Firmware, EDK2/UEFI and Grub2
** For more information about the reference bootloader used by HiKey, please check "Reference-Bootloader-Hikey":https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey
** Pre-built files: "http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/bootloader":http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/bootloader
* Linux Kernel:
** Derived from Linux 4.1 kernel
** Git: "https://github.com/96boards/linux.git":https://github.com/96boards/linux.git
** Branch: *android-hikey-linaro-4.1*
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

bc. wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/bootloader/l-loader.bin
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/bootloader/nvme.img
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/bootloader/fip.bin
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/bootloader/ptable-aosp-4g.img
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/bootloader/ptable-aosp-8g.img
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/bootloader/hisi-idt.py

*CE AOSP RPB image:*

bc. wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/boot_fat.uefi.img.tar.xz
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/cache.img.tar.xz
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/userdata.img.tar.xz
wget http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/userdata-8gb.img.tar.xz

Since @system.img@ requires the user to accept an End User License Agreement covering the rights to download and use the proprietary Mali userspace driver, it needs to be manually downloaded via browser. Please go to "http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/system.img.tar.xz":http://builds.96boards.org/releases/reference-platform/aosp/hikey/15.12/system.img.tar.xz and follow the instructions to download the file.

Uncompress the .tar.xz files using your operating system file manager, or with the following command, for each file:

bc. xz --decompress [filename].tar.xz; tar -xvf [filename].tar

h3. Flashing

h4. Bootloader

To flash the bootloader the recovery mode is required. For more information about the recovery mode, how to enable and use, please go to "https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey#enabling-recovery-mode":https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey#enabling-recovery-mode

Now you need to identify if your device contains 4G or 8G of eMMC (LeMaker produces 2 HiKey versions, one with 4G and another with 8G of storage). The @ptable-aosp@ and @userdata@ files will differ depending on the board you have.

On recovery mode, flash the bootloader with the following command:

bc. sudo python hisi-idt.py --img1=l-loader.bin -d /dev/ttyUSB0

Then on a 4G compatible device:

bc. sudo fastboot flash ptable ptable-aosp-4g.img

Or the following on a 8G compatible device:

bc. sudo fastboot flash ptable ptable-aosp-8g.img

Then flash UEFI:

bc. sudo fastboot flash fastboot fip.bin

Change @ttyUSB0@ to the right interface name that gets exported to your host system.

Make sure to reboot the board after updating the partition table (@ptable-aosp@), otherwise flashing the system image might fail.

h4. Boot, System, Cache and Userdata

Fastboot is required to flash boot, system, cache and userdata.

*Flashing boot, cache, system and userdata:*

Enable fastboot (either via recovery or by changing the boot jumpers), and then just flash the required files:

bc. sudo fastboot flash boot boot_fat.uefi.img
sudo fastboot flash cache cache.img
sudo fastboot flash system system.img
sudo fastboot flash nvme nvme.img

Then on a 4G compatible device:

bc. sudo fastboot flash userdata userdata.img

Or the following on a 8G compatible device:

bc. sudo fastboot flash userdata userdata-8gb.img


Once flashed, make sure recovery mode is not enabled (pin3-pin4 on J15), that you don't have any sd card in place (since it first tries to boot from sd card, boot order can be changed with @sudo fastboot oem bootdevice [emmc|sd]@), then just reboot the board and enjoy :-)

h3. Additional resources

For known issues and more information about this release, please check "https://github.com/96boards/documentation/wiki/ReferencePlatform":https://github.com/96boards/documentation/wiki/ReferencePlatform
