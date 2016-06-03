### LeMaker Cello

***

### Critical Bug List

As both USB and the PCIe slot are not yet functional (hardware issues), the only way to currently start the installer is via SATA (CD-ROM, or flashed in a SATA disk). Once we get a functional Realtek UEFI driver, it will also be possible to PXE boot the installer.

Please also check bugs [2194](https://bugs.linaro.org/show_bug.cgi?id=2194), [2195](https://bugs.linaro.org/show_bug.cgi?id=2195) and  [2196](https://bugs.linaro.org/show_bug.cgi?id=2196) for the known issues.

### Boot Firmware

The [UEFI/EDK2 guide for EE](https://github.com/linaro/documentation/wiki/UEFI-EDK2-Guide-for-EE) provides information on how to flash the boot firmware for Cello (EDK2).

Since the EDK2 based firmware is not yet public (work in progress), internal access to the tree/binary is required. Email your board point of contact for further information on how to download the required firmware.

### Reference Platform Kernel

The Reference Platform kernel used by the enterprise release can be found on [github.com/96boards/linux](https://github.com/96boards/linux/tree/96b/releases/2016.03)

Since we use the same kernel config with all our builds and distributions, it is also available as part of the same kernel tree, and can be found at [arch/arm64/configs/distro.config](https://github.com/96boards/linux/blob/96b/releases/2016.03/arch/arm64/configs/distro.config).

At the time of the 16.03 release, the kernel is based on *4.4.0*.

### Quick Start

Booting from the network is not yet supported due lack of a binary UEFI driver for RTL8111GS, so installing from a physical medium is required (CD-ROM, SATA disk). USB and micro SD is not yet recognized by the UEFI firmware.

##### Flashing the firmware

Follow the instructions available as part of the [UEFI EDK2 Guide](https://github.com/linaro/documentation/wiki/UEFI-EDK2-Guide-for-EE#amd-overdrive) in order to flash your LeMaker Cello. The tested flashing process requires [DediProg SF100](http://www.dediprog.com/pd/spi-flash-solution/SF100) or [SPI Hook](http://www.tincantools.com/SPI_Hook.html).

### Distro Installers

Install instructions for the tested/supported distributions:
* [Debian 8.x 'Jessie'](https://github.com/Linaro/documentation/wiki/Reference-Platform-EE-Installing-Debian-Jessie#loading-debian-installer-from-the-minimal-cd) - Using the minimum ISO
* [CentOS 7](https://github.com/linaro/documentation/wiki/Reference-Platform-EE-Installing-CentOS-7)

#### Other distributions

Only Debian and CentOS are officially released and validated as part of the reference software platform project, but other distributions can be easily supported as well (just need kernel and installer changes).

Extra resources for other distributions:
* [Fedora 23](https://github.com/linaro/documentation/wiki/Reference-Platform-EE-Installing-Fedora-23)

### Enterprise Software Components

#### OpenStack

Follow the [instructions](https://github.com/linaro/documentation/wiki/Openstack-Liberty) on how to install and run OpenStack Liberty on Debian Jessie.

#### Hadoop (ODPi BigTop)

##### Installation

Follow the [instructions](https://github.com/linaro/documentation/wiki/ODPi-Hadoop-Installation) to install ODPi BigTop Hadoop

##### Setup and Running Hadoop

Follow the [instructions](https://github.com/linaro/documentation/wiki/ODPi-BigTop-Hadoop-configuration-and-Running) to configure and install Hadoop
