## Install Instructions - CE Debian RPB 15.10 - Dragonboard410c

This guide describes how to get started with the CE Debian Reference Platform Build, release 15.10, for Dragonboard410c.

For more information about the Dragonboard410c development board, please check https://www.96boards.org/products/ce/dragonboard410c/

### Image Components

The CE Debian RPB 15.10 - Dragonboard410c build is composed of the following artifacts:

- Bootloader:
   - [Qualcomm proprietary first stage bootloader](https://developer.qualcomm.com/download/linux-ubuntu-board-support-package-v1.zip)
   - [Little Kernel](https://git.linaro.org/landing-teams/working/qualcomm/lk.git) as second stage boot loader
- Linux Kernel:
   - Upstream plus extra changes for a better hardware support
   - Git: https://github.com/rsalveti/linux.git
   - Branch: *reference-qcom-rebase*
- Debian "Jessie"
   - ALIP (LXDE based)
   - Custom 96Boards artworks and default settings
   - Additional packages provided by [linaro-overlay](https://repo.linaro.org/ubuntu/linaro-overlay)

#### Closed source binaries

This release contains proprietary firmware. You can also download the proprietary firmware separately, from [here](https://developer.qualcomm.com/download/db410c/firmware-410c-1.1.0.bin). All the required firmware files are pre-installed, and the image is bound to the following [license agreement](https://git.linaro.org/landing-teams/working/qualcomm/lt-docs.git/blob_plain/HEAD:/license/license.txt).

### Downloading the pre-built binaries

The build is mainly composed by two image files (boot and rootfs), but to avoid incompatibilities issues with older bootloaders, or different partition tables, it's also recommended to flash the bootloader.

Flashing and booting from the external SD Card is not supported by this release.

*Bootloader files:*

Download the latest bootloader zip from [https://builds.96boards.org/releases/dragonboard410c/linaro/rescue/latest](https://builds.96boards.org/releases/dragonboard410c/linaro/rescue/latest/dragonboard410c_bootloader_emmc_linux*.zip)

*CE Debian RPB image:*

```
wget https://builds.96boards.org/releases/reference-platform/debian/dragonboard410c/15.10/dragonboard410c-boot-linux-20151106-31.img.gz
wget https://builds.96boards.org/releases/reference-platform/debian/dragonboard410c/15.10/dragonboard410c-rootfs-debian-jessie-alip-20151106-31.emmc.img.gz
gunzip dragonboard410c-*
```

### Flashing

#### Bootloader

Flash the eMMC with the bootloader:

- Unzip the bootloader that was downloaded in the previous step. Note the directory that is it located in.
- Assure that a micro USB cable is connected from the micro-USB port on the DB410c to the host PC
- Assure micro SD Card slot is empty on the DB410c
- Set the S6 switch on the DB410c to: 0-0-0-0 {SD Boot set to off}
- Power on the DB410c into fastboot mode
   - Press and hold the Vol (-) button on the DB410c (S4)
   - While pressing S4 button, power up the DB410c. It will come up in fastboot mode
- From the host PC terminal window, run the following commands:

```
# Check to make sure fastboot device connected.  If not resolve
sudo fastboot devices
# cd to the directory the bootloader zip file was extracted
cd <extraction directory>
sudo ./flashall
```

The bootloader is now installed on the DB410c.

#### Boot and Rootfs

Fastboot is required to flash both the boot and rootfs images.

*Flashing boot and rootfs:*

```
sudo fastboot flash boot dragonboard410c-boot-linux-20151106-31.img
sudo fastboot flash rootfs dragonboard410c-rootfs-debian-jessie-alip-20151106-31.emmc.img
```

Once flashed just reboot the board and enjoy :-)
