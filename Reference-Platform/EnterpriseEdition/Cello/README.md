### LeMaker Cello

***

### Critical Bug List

As both USB and the PCIe slot are not yet functional (hardware issues), the only way to currently start the installer is via SATA (CD-ROM, or flashed in a SATA disk). Once the Realtek UEFI driver gets integrated as part of OpenPlatformPkg, it will also be possible to PXE boot the installer.

Please also check bugs [2194](https://bugs.linaro.org/show_bug.cgi?id=2194), [2195](https://bugs.linaro.org/show_bug.cgi?id=2195) and [2196](https://bugs.linaro.org/show_bug.cgi?id=2196) for the known issues.

### Boot Firmware

The [UEFI/EDK2 guide for EE](../UEFI-EDK2-Guide-EE.md) provides information on how to flash the boot firmware for Cello (Tianocore EDK2).

### Reference Platform Kernel

The Reference Platform kernel used by the enterprise release can be found on [github.com/96boards/linux](https://github.com/96boards/linux/tree/96b/releases/2016.06)

Since we use the same kernel config with all our builds and distributions, it is also available as part of the same kernel tree, and can be found at [arch/arm64/configs/distro.config](https://github.com/96boards/linux/blob/96b/releases/2016.06/arch/arm64/configs/distro.config).

At the time of the 16.06 release, the kernel is based on *4.4.11*.

### Quick Start

Booting from the network is not yet supported due lack of a binary UEFI driver for RTL8111GS, so installing from a physical medium is required (CD-ROM, SATA disk). USB and micro SD is not yet recognized by the UEFI firmware.

##### Flashing the firmware

Follow the instructions available as part of the [UEFI EDK2 Guide](../UEFI-EDK2-Guide-EE.md#amd-overdrive) in order to flash your LeMaker Cello. The tested flashing process requires [DediProg SF100](http://www.dediprog.com/pd/spi-flash-solution/SF100) or [SPI Hook](http://www.tincantools.com/SPI_Hook.html).

### Distro Installers

Install instructions for the tested/supported distributions:
* [Debian 8.x 'Jessie'](../Install-Debian-Jessie.md#loading-debian-installer-from-the-minimal-cd) - Using the minimum ISO
* [CentOS 7](../Install-CentOS-7.md)

#### Other distributions

Only Debian and CentOS are officially released and validated as part of the reference software platform project, but other distributions can be easily supported as well (just need kernel and installer changes).

Extra resources for other distributions:
* [Fedora 23](../Install-Fedora-23.md)

### Enterprise Software Components

#### OpenStack

Follow the [instructions](../OpenStack-Liberty.md) on how to install and run OpenStack Liberty on Debian Jessie.

#### Hadoop (ODPi BigTop)

##### Installation

Follow the [instructions](../ODPi-Hadoop-Installation.md) to install ODPi BigTop Hadoop

##### Setup and Running Hadoop

Follow the [instructions](../ODPi-BigTop-Hadoop-Config-Run.md) to configure and install Hadoop
