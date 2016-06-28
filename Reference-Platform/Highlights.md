### Highlights for 16.06 release:

***

###### Consumer and Enterprise Edition:
#### Kernel
- Unified tree shared between the CE and EE builds. Supports Hikey, Dragonboard410c, D02, D03, APM X-Gene, HP Proliant m400, LeMaker Cello, Q2432LZB and ThunderX.
- 4.4.11 based, including under-review topic branches to extend the hardware support for the platforms available.
- Device-Tree support for CE; ARM ACPI and PCIe support for Enterprise.
- Single kernel config for all platforms in arch/arm64/configs/distro.config
- Single kernel binary (package) for all platforms

***

###### Consumer Edition:

#### CE Debian RPB (common):
- Upgrade to Debian 8.5 "Jessie"
- Upgrade to the unified 4.4.11 Linux Kernel
- SDCard installer and images

#### CE Debian RPB for HiKey:
- OP-TEE Integrated by default
- UEFI updated to use the latest development trees based on Tianocore + OpenPlatformPkg

#### CE Debian RPB for DragonBoard™ 410:
- U-Boot is now integrated by default, allowing kernel package updates without reflashing the device

#### CE OE/Yocto RPB:
- Included the unified 4.4.11 kernel
- Weston, desktop and console images supported for both HiKey and Dragonboard410c

***

###### Enterprise Edition

#### Supported platforms:

- AMD Overdrive A0 and B
- D02
- D03 (new)
- APM X-Gene Mustang
- HP ProLiant m400
- LeMaker Cello (new)
- Q2432LZB (new)
- ThunderX (new)

#### Overall platform features, validated as part of the release:

- UEFI with ACPI
- KVM
- PCIe

#### Firmware:

- Updated UEFI/EDK2 for D02, D03, Cello and Overdrive
- Support available via the upstream Tianocore and OpenPlatformPkg development trees

#### Network Installers:

- Debian:
   - Upgrade to Debian 8.5 "Jessie"
   - Using the unified 4.4.11 kernel
- CentOS:
   - Based on CentOS 7.2 16.03
   - Using the consolidated 4.4.11 kernel

#### Enterprise Components:

- Docker 1.9.1
- OpenStack Liberty for Debian Jessie
- ODPi based Hadoop
- Spark 1.6
- OpenJDK 8 (Linaro 16.03)

***
