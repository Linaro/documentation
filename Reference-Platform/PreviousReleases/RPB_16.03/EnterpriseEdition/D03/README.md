### D03

***

### Boot Firmware

The [UEFI/EDK2 guide for EE](../UEFI-EDK2-Guide-EE.md) provides information about building and flashing the boot firmware for D03.

### Reference Platform Kernel

The Reference Platform kernel used by the enterprise release can be found on [github.com/96boards/linux](https://github.com/96boards/linux/tree/96b/releases/2016.06)

Since we use the same kernel config with all our builds and distributions, it is also available as part of the same kernel tree, and can be found at [arch/arm64/configs/distro.config](https://github.com/96boards/linux/blob/96b/releases/2016.06/arch/arm64/configs/distro.config).

At the time of the 16.03 release, the kernel is based on *4.4.11*.

For future releases we will also have kernel config fragments for key functionality that will make it easier for other projects and distributions to consume.

The Reference Platform kernel will act as an integration point (very similar to linux-next) for various upstream-targeted features and platform-enablement code on the latest kernel. Please read the [kernel policy](../../KernelPolicy.md) on how this kernel will be maintained. It is not meant to be a stable kernel - the [LSK](https://wiki.linaro.org/LSK) is already available for that.

### Quick Start

#### D03 - QuickStart

UEFI/EDK2 is supported by D03 (with build from source instructions available as part of the [UEFI EDK2 Guide](../UEFI-EDK2-Guide-EE.md#building), and since ACPI support is new, please make sure you are using the latest firmware available at [https://builds.96boards.org/snapshots/reference-platform/components/uefi/latest/release/d03/](https://builds.96boards.org/snapshots/reference-platform/components/uefi/latest/release/d03/) before proceeding with kernel testing or installing your favorite distribution (and please make sure to report your firmware version when reporting issues and bugs).

**NOTE:** 16.06 kernel **requires** the 16.06 UEFI/EDK2 firmware release!

##### Flashing the firmware

Follow the instructions available as part of the [UEFI EDK2 Guide](../UEFI-EDK2-Guide-EE.md#d03) in order to flash your D03. The tested flashing process only requires access to a TFTP server, since the firmware supports fetching the firmware from the network.

### Network Installers

In order to install a distribution from network, PXE (DCHP/TFTP) booting is required. Since we require UEFI for the Enterprise Edition, the setup is usually easier since all you need is to load GRUB 2 (and its configuration). Check [this link](../DHCP-TFTP-Server-UEFI.md) for instructions on how to quickly setup your own PXE server (using *dnsmasq*).

Install instructions for the tested/supported distributions:
* [Debian 8.x 'Jessie'](../Install-Debian-Jessie.md)
* [CentOS 7](../Install-CentOS-7.md)

Enterprise Test Reports: ([Debian](https://builds.96boards.org/releases/reference-platform/components/debian-installer/16.03/EE-Debian-RPB-16.06-TestReport.pdf) / [CentOS](https://builds.96boards.org/releases/reference-platform/components/centos-installer/16.03/EE-CentOS-RPB-16.06-TestReport.pdf))

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
