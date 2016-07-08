### Highlights for 16.06 release:

***

###### Consumer and Enterprise Edition:

#### Kernel

- Unified tree shared between the CE and EE builds.
- 4.4.11-based, including some under-review topic branches to extend the features and platform hardware support.
- Device-Tree support for CE; ARM ACPI and PCIe support for Enterprise.
- Added OP-TEE support
   - Enabled on HiKey and Juno-r1
- Supports Reference HW platforms HiKey and Cello
   - Other Test Platforms include: Dragonboard 410c, Hisilicon D02 and D03, APM X-Gene, HP Proliant m400, AMD Overdrive, Qualcomm Q2432:ZB, and Cavium ThunderX.
- Single kernel config for all platforms in arch/arm64/configs/distro.config
- Single kernel binary (package) for all platforms

#### Bootloader

- UEFI OpenPlatformPkg (upstream) now contains reference implementations for Huawei D02/D03, AMD Overdrive and LeMaker Cello
- U-boot support in DB410c images to allow easier handling of images

***

###### Consumer Edition:

#### Reference hardware platform:
- LeMaker Hikey

#### Other supported test platforms:
- Dragonboard 410c

#### Overall CE Debian platform features, validated as part of the release:
- UEFI with DT
- Upgrade to Debian 8.5 "Jessie"
- Upgrade to the unified 4.4.11 Linux Kernel
- Upgrade graphics components: Mesa 11.1.2 and XServer 1.17.3a
- Rootfs automatically resized during the first boot

#### CE Debian RPB for HiKey:
- OP-TEE integrated by default
- UEFI updated to use the latest development trees based on Tianocore 
   - OpenPlatformPkg

#### CE Debian build for DragonBoard™ 410c:
- U-boot chain-loaded from LK

#### CE OE/Yocto RPB:
- First OpenEmbedded-based RPB, including several changes and components merged from the LHG OE layers
- Dragonboard 410c and HiKey support
- HiKey features:
   - OP-TEE initial support
   - Mali support for HiKey
- Dragonboard 410c features:
   - GPU, WLAN, BT, audio, LS I/O, camera and  GPS

***

###### Enterprise Edition

#### Reference hardware platform:
- LeMaker Cello

#### Other supported test platforms:
- AMD Overdrive A0 and B0
- Hisilicon D02 
- Hisilicon D03 (new)
- APM X-Gene Mustang
- HP ProLiant m400
- Qualcomm Q2432LZB (new)
- Cavium ThunderX (new)

#### Overall platform features, validated as part of the release:
- UEFI with ACPI
- KVM
- PCIe

#### Firmware:
- UEFI OpenPlatformPkg (upstream) now contains reference implementation for Huawei D02/D03, AMD Overdrive and LeMaker Cello

#### Network Installers:
- Debian:
   - Upgrade to Debian 8.5 "Jessie"
   - Use the unified 4.4.11 kernel
- CentOS
   - Based on CentOS 7.2 16.03
   - Use the unified 4.4.11 kernel

#### Enterprise Components:
- Docker 1.9.1
- OpenStack Liberty for Debian Jessie and CentOS
- ODPi 1.0.0 based Hadoop
- Spark 1.3.1
- OpenJDK 8
- QEMU 2.6
