# 16.12 Release Notes - Enterprise Reference Platform

## Reference Platform Kernel

- 4.9 based, including under-review topic branches to extend hardware platform support
- Unified tree, used by both the CentOS and Debian Reference Platforms
- ACPI and PCIe support
- Single kernel config and binary (package) for all hardware platforms

## UEFI

- Tianocore EDK II and OpenPlatformPkg containing reference implementations for Huawei D03/D05, AMD Overdrive and LeMaker Cello

## Debian

- Network Installer based on Debian 8.6 "Jessie"
- Unified Reference Platform Kernel based on 4.9

## CentOS

- Network Installer based on CentOS 7.2 16.03
- Unified Reference Platform Kernel based on 4.9

## Enterprise Components
- Docker 1.10.3
- OpenStack
   - Not ready
- Ceph 10.2.3
   - Not ready
- Spark 2.0
- OpenJDK 8
- QEMU 2.7

## Supported Hardware Platforms

- AMD Overdrive
- LeMaker Cello
- HiSilicon D03
- HiSilicon D05
- APM X-Gene Mustang
- HP Proliant m400
- Qualcomm Q2432
- Cavium Thunder X

## Known Issues

### CentOS:

- Using Docker with XFS and Overlay Storage Driver:
   - Note that XFS file systems must be created with the -n ftype=1 option enabled for use as an overlay. With the rootfs and any file systems created during system installation, set the --mkfsoptions=-n ftype=1 parameters in the Anaconda kickstart. When creating a new file system after the installation, run the # mkfs -t xfs -n ftype=1 /PATH/TO/DEVICE command. To determine whether an existing file system is eligible for use as an overlay, run the # xfs_info /PATH/TO/DEVICE | grep ftype command to see if the ftype=1 option is enabled.
   - https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/7.2_Release_Notes/technology-preview-file_systems.html
- Installation via USB fails - https://bugs.linaro.org/show_bug.cgi?id=2655

### QDF2432:

- No support for on-board ethernet (emac) - https://bugs.linaro.org/show_bug.cgi?id=2603

### D03:

- Only 16 cores enabled (instead of 32) - https://bugs.linaro.org/show_bug.cgi?id=2635
- Fails to install/boot Debian/CentOS with the default boot argument - https://bugs.linaro.org/show_bug.cgi?id=2661
- Earlycon not working (no console output) - https://bugs.linaro.org/show_bug.cgi?id=2699

### HP-m400 Moonshot:

- Boot medium is not updated after network installer - https://bugs.linaro.org/show_bug.cgi?id=2703

### Cavium (ThunderX)

- UEFI boot entry not created after Debian installer finished - https://bugs.linaro.org/show_bug.cgi?id=2736

