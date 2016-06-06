## Reference Platform Builds - 15.12

The *15.12* release is the second release for the Reference Software Platform project, and for the first time also including support for the Enterprise Edition. Since there is still no availability for the 96Boards HuskyBoard, the first EE RPB was produced using the current enterprise development boards that are available in Linaro, such as HiSilicon D02 and AMD Overdrive (same SoC from HuskyBoard, known as Seattle). Once HuskBoard is available, the work for making it supported by the EE RPB should be minimal.

A lot of work was put in place for the EE RPB, covering firmware (UEFI/EDK2), Linux 4.4 (with ACPI), Debian Jessie/CentOS 7 network installers, OpenStack Liberty, Hadoop, Spark and a few others, consolidating the work from several other Linaro groups and teams as well as from community and members.

For the Consumer Edition the CE AOSP RPB for Hikey is now using a 4.1 based kernel, closer to what is provided directly by AOSP. We decided to not push major updates and rebases for the CE Debian RPB kernel since we want the changes to follow the same [kernel policy](../../KernelPolicy.md) as used by the EE kernel. The goal of having one single tree for both CE and EE, with a strict upstream-based policy will continue, and we hope to have more news on that during the upcoming weeks.

The work for the CE OE/Yocto RPB was also started, but unfortunately not yet covering a single machine (due lack of a single kernel). Please check [https://github.com/96boards/meta-rpb](https://github.com/96boards/meta-rpb) and https://github.com/96boards/oe-rpb-manifest to see what was already done for OpenEmbedded.

##### Highlights for this release:

###### Enterprise Edition

- Firmware:
   - UEFI/EDK2 support for D02, provided by OpenPlatformPkg
- Linux:
   - 4.4-rc4 based, including support for D02 and Overdrive
   - ACPI support for D02 and Overdrive (mandatory for the enterprise edition)
- Distributions:
   - Debian Jessie network installer (using latest kernel)
   - CentOS 7 network installer (alpha state)
- Enterprise Components:
   - Docker 1.9.1
   - OpenStack Liberty
   - ODPi BigTop (Hadoop, Spark, etc)
   - OpenJDK 8

###### Consumer Edition

- CE Debian RPB for DragonBoard410 and HiKey (including support for the LeMaker version):
   - Debian 8.2 "Jessie"
   - 4.3 kernel (with additional patches)
   - OpenJDK 8 included by default
   - 96Boards artwork and default settings
- CE AOSP RPB for HiKey (including support for the LeMaker version):
   - AOSP Android Marshmallow 6.0
   - 4.1 based kernel

The complete list of known issues for this release: [Known Issues](Known=-Issues.md)

##### Enterprise

- [UEFI/EDK2](https://builds.96boards.org/releases/reference-platform/components/uefi/15.12/) for HiSilicon D02
- [Kernel 4.4-rc4](https://builds.96boards.org/releases/reference-platform/components/linux/enterprise/15.12/) tested with D02 and Overdrive
- [Debian (Jessie) Installer](https://builds.96boards.org/releases/reference-platform/components/debian-installer/15.12/) tested with D02 and Overdrive (shipping kernel 4.4-rc4 by default)
- [OpenStack Liberty]() for Debian Jessie
- [ODPi Hadoop]() for Debian Jessie
- [EE Debian Test Report](https://builds.96boards.org/releases/reference-platform/components/linux/enterprise/15.12/EE-Debian-RPB-15.12-TestReport.pdf)
