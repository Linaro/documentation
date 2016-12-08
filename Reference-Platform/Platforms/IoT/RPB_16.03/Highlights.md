### Highlights for 16.03 release:

***

###### Consumer and Enterprise Edition:
#### Kernel
- Unified tree shared between the CE and EE builds. Supports Hikey, Dragonboard, D02, APM X-Gene, HP Proliant m400 and AMD Overdrive.
- 4.4.0 based, including under-review topic branches to extend the hardware support for the platforms available.
- Device-Tree support for CE; ARM ACPI and PCIe support for Enterprise.
- Single kernel config for all platforms in arch/arm64/configs/distro.config
- Single kernel binary (package) for all platforms

***

###### Consumer Edition:

#### CE Debian RPB (common):
- Upgrade to Debian 8.3 "Jessie"
- Upgrade to the unified 4.4.0 Linux Kernel
- Upgrade graphics components: Mesa 11.1.2 and Xserver 1.17.3
- Rootfs automatically resized during the first boot

#### CE Debian RPB for DragonBoard™ 410:
- Freedreno X11 video driver included by default (1.4.0)
- Analog audio playback and record support
- Upgrade Qualcomm Firmware Package to 1.2

#### CE Debian RPB for HiKey (CircuitCo & LeMaker):
- Default Grub 2 boot configuration updated, now supporting kernel package upgrades
- xserver-xorg-video-armsoc now included by default (still using software rendering, Mali integration expected as part of the next release) 

#### CE AOSP RPB (common):
- AOSP Android Marshmallow 6.0 (android-6.0.1_r16) 

#### CE AOSP RPB for DragonBoard™ 410:
- Initial build, available as Developer Preview (not suitable for end users).
- Mesa and Freedreno support
- Kernel 4.4.0  

#### CE AOSP RPB for HiKey (CircuitCo & LeMaker):
- AOSP Android Marshmallow 6.0 (android-6.0.1_r16)
- 4.1 based kernel 

#### CE OE/Yocto RPB:
- Included the unified 4.4.0 kernel
- meta-backports created, to contain backported recipes used by the reference platform

***

###### Enterprise Edition

#### Supported platforms:

- AMD Overdrive A0 (new) and B0
- D02 
- APM X-Gene Mustang (new)
- HP ProLiant m400 (new) 

#### Overall platform features, validated as part of the release:

- UEFI with ACPI
- KVM
- PCIe 

#### Firmware:

- Updated UEFI/EDK2 for D02, including support for PCIe and SAS 

#### Network Installers:

- Debian:
   - Upgrade to Debian 8.3 "Jessie"
   - Using the unified 4.4.0 kernel
- CentOS (Now officially supported):
   - Based on CentOS 7.2 15.11
   - Using the consolidated 4.4 kernel

#### Enterprise Components:

- Docker 1.9.1
- OpenStack Liberty for Debian Jessie
   - CentOS to be supported as part of the next cycle
- ODPi based Hadoop
- Spark 1.6
- OpenJDK 8 (Linaro 16.03)

***
