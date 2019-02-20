### Overdrive

***

### Boot Firmware

The [UEFI/EDK2 guide for EE](../../../../EECommon/UEFI-EDK2-Guide-EE.md) provides information on how to flash the boot firmware for Overdrive (AMI Bios).

### Reference Platform Kernel

The Reference Platform kernel used by the enterprise release can be found on [github.com/96boards/linux](https://github.com/96boards/linux/tree/96b/releases/2016.06)

Since we use the same kernel config with all our builds and distributions, it is also available as part of the same kernel tree, and can be found at [arch/arm64/configs/distro.config](https://github.com/96boards/linux/blob/96b/releases/2016.03/arch/arm64/configs/distro.config).

At the time of the 16.06 release, the kernel is based on *4.4.11*.

### Quick Start

#### AMD Overdrive

UEFI/EDK2 is supported by Overdrive Rev B (with build from source instructions available as part of the [UEFI EDK2 Guide](../../../../EECommon/UEFI-EDK2-Guide-EE.md#building), and since ACPI support is new, please make sure you are using the latest firmware available at [https://builds.96boards.org/releases/reference-platform/components/uefi/16.06/release/overdrive/](https://builds.96boards.org/releases/reference-platform/components/uefi/16.06/release/overdrive) before proceeding with kernel testing or installing your favorite distribution (and please make sure to report your firmware version when reporting issues and bugs).

**NOTE:** 16.06 kernel **requires** the 16.06 UEFI/EDK2 firmware release!


##### Flashing the firmware

Follow the instructions available as part of the [UEFI EDK2 Guide](../../../../EECommon/UEFI-EDK2-Guide-EE.md#amd-overdrive) in order to flash your AMD Overdrive. The tested flashing process requires [DediProg SF100](https://www.dediprog.com/pd/spi-flash-solution/SF100) or [SPI Hook](https://www.tincantools.com/spi-hook/).

### Network Installers

In order to install a distribution from network, PXE (DCHP/TFTP) booting is required. Since we require UEFI for the Enterprise Edition, the setup is usually easier since all you need is to load GRUB 2 (and its configuration). Check [this link](../../../../EECommon/DHCP-TFTP-Server-UEFI.md) for instructions on how to quickly setup your own PXE server (using *dnsmasq*).

Install instructions for the tested/supported distributions:
* [Debian 8.x 'Jessie'](../../../../EECommon/Install-Debian-Jessie.md)
* [CentOS 7](../../../../EECommon/Install-CentOS-7.md)

#### Other distributions

Only Debian and CentOS are officially released and validated as part of the reference software platform project, but other distributions can be easily supported as well (just need kernel and installer changes).

Extra resources for other distributions:
* [Fedora 23](../../../../EECommon/Install-Fedora-23.md)

### Enterprise Software Components

#### OpenStack

Follow the [instructions](../../../../EECommon/OpenStack-Liberty.md) on how to install and run OpenStack Liberty on Debian Jessie.

#### Hadoop (ODPi BigTop)

##### Installation

Follow the [instructions](../../../../EECommon/ODPi-Hadoop-Installation.md) to install ODPi BigTop Hadoop

##### Setup and Running Hadoop

Follow the [instructions](../../../../EECommon/ODPi-BigTop-Hadoop-Config-Run.md) to configure and install Hadoop
