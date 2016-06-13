### Highlights for 16.06 release:

***

###### Consumer and Enterprise Edition:
#### Kernel
- 1
- 2
- 3

***

###### Consumer Edition:

#### CE Debian RPB (common):
- Upgrade to Debian x.x "Jessie"
- Upgrade to the unified x.x.x Linux Kernel
- Upgrade graphics components: Mesa x.x.x and Xserver x.x.x
- Rootfs automatically resized during the first boot

#### CE Debian RPB for DragonBoard™ 410:
- Freedreno X11 video driver included by default (x.x.x)
- Analog audio playback and record support
- Upgrade Qualcomm Firmware Package to x.x

#### CE Debian RPB for HiKey (CircuitCo & LeMaker):
- Default Grub 2 boot configuration updated, now supporting kernel package upgrades
- xserver-xorg-video-armsoc now included by default (still using software rendering, Mali integration expected as part of the next release) 

#### CE AOSP RPB (common):
- AOSP Android Marshmallow x.x (android-x.x.x_rXX) 

#### CE AOSP RPB for DragonBoard™ 410:
- Initial build, available as Developer Preview (not suitable for end users).
- Mesa and Freedreno support
- Kernel x.x.x  

#### CE AOSP RPB for HiKey (CircuitCo & LeMaker):
- AOSP Android Marshmallow 6.0 (android-x.x.x_rXX)
- x.x based kernel 

#### CE OE/Yocto RPB:
- Included the unified x.x.x kernel
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
   - Upgrade to Debian x.x "Jessie"
   - Using the unified x.x.x kernel
- CentOS (Now officially supported):
   - Based on CentOS x.x xx.xx
   - Using the consolidated x.x kernel

#### Enterprise Components:

- Docker x.x.x
- OpenStack Liberty for Debian Jessie
   - CentOS to be supported as part of the next cycle
- ODPi based Hadoop
- Spark x.x
- OpenJDK x (Linaro xx.xx)

***
