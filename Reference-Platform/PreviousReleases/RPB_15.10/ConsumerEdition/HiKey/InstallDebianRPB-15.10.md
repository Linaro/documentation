h2. Install Instructions - CE Debian RPB 15.10 - HiKey

This guide describes how to get started with the CE Debian Reference Platform Build, release 15.10, for HiKey.

For more information about the HiKey development board, please check "https://www.96boards.org/products/ce/hikey/":https://www.96boards.org/products/ce/hikey/

h3. Image Components

The CE Debian RPB 15.10 - HiKey build is composed of the following artifacts:

* Bootloader:
** ARM Trusted Firmware, EDK2/UEFI and Grub2
** For more information about the reference bootloader used by HiKey, please check "Reference-Bootloader-Hikey":https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey
** Pre-built files: "http://builds.96boards.org/releases/reference-platform/debian/hikey/15.10/bootloader":http://builds.96boards.org/releases/reference-platform/debian/hikey/15.10/bootloader
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

bc. wget http://builds.96boards.org/releases/reference-platform/debian/hikey/15.10/bootloader/l-loader.bin
wget http://builds.96boards.org/releases/reference-platform/debian/hikey/15.10/bootloader/fip.bin
wget http://builds.96boards.org/releases/reference-platform/debian/hikey/15.10/bootloader/ptable-linux.img
wget http://builds.96boards.org/releases/reference-platform/debian/hikey/15.10/bootloader/hisi-idt.py

*CE Debian RPB image:*

bc. wget https://builds.96boards.org/releases/reference-platform/debian/hikey/15.10/hikey-boot-linux-20151106-31.uefi.img.gz
wget https://builds.96boards.org/releases/reference-platform/debian/hikey/15.10/hikey-rootfs-debian-jessie-alip-20151106-31.emmc.img.gz
gunzip hikey-*

h3. Flashing

h4. Bootloader

To flash the bootloader the recovery mode is required. For more information about the recovery mode, how to enable and use, please check "https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey#enabling-recovery-mode":https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey#enabling-recovery-mode

On recovery mode, flash the bootloader with the following command:

bc. sudo python hisi-idt.py --img1=l-loader.bin -d /dev/ttyUSB0
sudo fastboot flash ptable ptable-linux.img
sudo fastboot flash fastboot fip.bin

Change @ttyUSB0@ to the right interface name that gets exported to your host system.

Make sure to reboot the board after updating the partition table (@ptable-linux.img@), otherwise flashing the rootfs might fail.

h4. Boot and Rootfs

Fastboot is required to flash both the boot and rootfs images.

Due bug "117 (UEFI fastboot uploads hangs for large images)":https://bugs.96boards.org/show_bug.cgi?id=117, it's recommended to flash both the boot and rootfs images via recovery mode (after running @hisi-idt.py@).

*Flashing boot and rootfs:*

bc. sudo fastboot flash boot hikey-boot-linux-20151106-31.uefi.img
sudo fastboot flash system hikey-rootfs-debian-jessie-alip-20151106-31.emmc.img

Once flashed, make sure recovery mode is not enabled (pin3-pin4 on J15), then just reboot the board and enjoy :-)

h3. Additional resources

For known issues and more information about this release, please check "https://github.com/96boards/documentation/wiki/ReferenceSoftware":https://github.com/96boards/documentation/wiki/ReferenceSoftware
