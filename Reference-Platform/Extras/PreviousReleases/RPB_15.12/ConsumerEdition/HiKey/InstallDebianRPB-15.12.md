h2. Install Instructions - CE Debian RPB 15.12 - HiKey

This guide describes how to get started with the CE Debian Reference Platform Build, release 15.12, for HiKey.

For more information about the HiKey development board, please check "https://www.96boards.org/products/ce/hikey/":https://www.96boards.org/products/ce/hikey/

h3. Image Components

The CE Debian RPB 15.12 - HiKey build is composed of the following artifacts:

* Bootloader:
** ARM Trusted Firmware, EDK2/UEFI and Grub2
** For more information about the reference bootloader used by HiKey, please check "Reference-Bootloader-Hikey":https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey
** Pre-built files: "http://builds.96boards.org/releases/reference-platform/debian/hikey/15.12/bootloader":http://builds.96boards.org/releases/reference-platform/debian/hikey/15.12/bootloader
* Linux Kernel:
** Upstream plus extra changes for a better hardware support
** Git: "https://github.com/rsalveti/linux.git":https://github.com/rsalveti/linux.git
** Branch: *reference-hikey-rebase*
* Debian "Jessie"
** ALIP (LXDE based)
** Custom 96Boards artworks and default settings
** Additional packages provided by "linaro-overlay":http://repo.linaro.org/ubuntu/linaro-overlay
** Kernel and initrd loaded from the rootfs (under /boot)

h4. Closed source binaries

The following components requires a closed source binary for better hardware support:

* TI wlan firmware (@wl18xx@)
** Git: "http://git.ti.com/wilink8-wlan/wl18xx_fw":http://git.ti.com/wilink8-wlan/wl18xx_fw
** Branch: *R8.6*
* Extra firmware files available from firmware-linux
* Mali (not yet included by default)

h3. Downloading the pre-built binaries

The build is mainly composed by two image files (boot and rootfs), but to avoid incompatibilities issues with older bootloaders, or different partition tables, it's also recommended to flash the bootloader.

Flashing and booting from the external SD Card is not supported by this release.

*Bootloader files:*

bc. wget http://builds.96boards.org/releases/reference-platform/debian/hikey/15.12/bootloader/l-loader.bin
wget http://builds.96boards.org/releases/reference-platform/debian/hikey/15.12/bootloader/nvme.img
wget http://builds.96boards.org/releases/reference-platform/debian/hikey/15.12/bootloader/fip.bin
wget http://builds.96boards.org/releases/reference-platform/debian/hikey/15.12/bootloader/ptable-linux-4g.img
wget http://builds.96boards.org/releases/reference-platform/debian/hikey/15.12/bootloader/ptable-linux-8g.img
wget http://builds.96boards.org/releases/reference-platform/debian/hikey/15.12/bootloader/hisi-idt.py

*CE Debian RPB image:*

bc. wget https://builds.96boards.org/releases/reference-platform/debian/hikey/15.12/hikey-boot-linux-20151214-35.uefi.img.gz
wget https://builds.96boards.org/releases/reference-platform/debian/hikey/15.12/hikey-rootfs-debian-jessie-alip-20151214-35.emmc.img.gz
gunzip hikey-*

h3. Flashing

h4. Bootloader

To flash the bootloader the recovery mode is required. For more information about the recovery mode, how to enable and use, please check "https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey#enabling-recovery-mode":https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey#enabling-recovery-mode

Now you need to identify if your device contains 4G or 8G of eMMC (LeMaker produces 2 HiKey versions, one with 4G and another with 8G of storage). The @ptable-linux@ file will differ depending on the board you have.

On recovery mode, flash the bootloader with the following command:

bc. sudo python hisi-idt.py --img1=l-loader.bin

Then on a 4G compatible device:

bc. sudo fastboot flash ptable ptable-linux-4g.img

Or the following on a 8G compatible device:

bc. sudo fastboot flash ptable ptable-linux-8g.img

Then flash UEFI:

bc. sudo fastboot flash fastboot fip.bin

Make sure to reboot the board after updating the partition table (@ptable-linux@), otherwise flashing the rootfs might fail.

h4. Boot and Rootfs

Fastboot is required to flash both the boot and rootfs images.

To avoid bug "117 (UEFI fastboot uploads hangs for large images)":https://bugs.96boards.org/show_bug.cgi?id=117, it's recommended to flash both the boot and rootfs images via recovery mode (after running @hisi-idt.py@).

*Flashing boot and rootfs:*

bc. sudo fastboot flash boot hikey-boot-linux-20151214-35.uefi.img
sudo fastboot flash system hikey-rootfs-debian-jessie-alip-20151214-35.emmc.img

Once flashed, make sure recovery mode is not enabled (pin3-pin4 on J15), that you don’t have any sd card in place (since it first tries to boot from sd card, boot order can be changed with sudo fastboot oem bootdevice [emmc|sd]), then just reboot the board and enjoy :-)

h3. Additional resources

For known issues and more information about this release, please check "https://github.com/96boards/documentation/wiki/ReferencePlatform":https://github.com/96boards/documentation/wiki/ReferencePlatform
