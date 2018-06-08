# 18.06 Release Notes - Linaro Enterprise Reference Platform

The goal of the Linaro Enterprise Reference Platform is to provide a fully tested, end to end, documented, open source implementation for ARM based Enterprise servers. The Reference Platform includes kernel, a community supported userspace and additional relevant open source projects, and is validated against existing firmware releases. The Linaro Enterprise Reference Platform is built and tested on Linaro Enterprise Group members hardware and the Linaro Developer Cloud. It is intended to be a reference example for use as a foundation for members and partners for their products based on open source technologies. The members and partners to include distribution, hyperscaler or OEM/ODM vendors, can leverage the reference for ARM in the datacenter.

## Reference Platform Kernel

- 4.14 based, including backports of already upstreamed patches to fix issues discovered during QA.
- Unified tree, used by both the CentOS and Debian Reference Platforms
- Focused on ACPI and UEFI use-cases.
- Single kernel config and binary (package) for all hardware platforms

## Debian

- Network Installer based on Debian 9.2 "Stretch"
- Unified Reference Platform Kernel based on 4.14

## Enterprise Components
- Docker 1.12.6
- Bigtop 1.2 stack (Hadoop 2.7.3, Spark 2.1, Hive 1.1.3)
- ELK 5.4.1
- OpenJDK 8
- Libvirt 3.8.0
- QEMU 2.10
- DPDK 17.11

## Supported Hardware Platforms

- Cavium ThunderX
- HiSilicon D03
- HiSilicon D05
- HP Proliant m400
- Qualcomm Centriq 2400

## Known Issues

### Bootloader
- [3495](https://bugs.linaro.org/show_bug.cgi?id=3495) Unable to use Device-Tree with GRUB

### RPK:
- [3451](https://bugs.linaro.org/show_bug.cgi?id=3451) LTP: keyctl03 fails with "Failed to add key"
- [3469](https://bugs.linaro.org/show_bug.cgi?id=3469) LTP syscalls/madvise09 fails with "Found corrupted page"
- [3174](https://bugs.linaro.org/show_bug.cgi?id=3174) Docker triggered problem causes crash in tty subsystem.

### Qualcomm Centriq 2400:
- [3492](https://bugs.linaro.org/show_bug.cgi?id=3492) QDF2400: DMI information missing

### D05:
- [2657](https://bugs.linaro.org/show_bug.cgi?id=2657) [D03] [D05] Confusing Ethernet port sequence
- [3206](https://bugs.linaro.org/show_bug.cgi?id=3206) lscpu shows wrong cpu layout on D05
- [3450](https://bugs.linaro.org/show_bug.cgi?id=3450) Need for HPM File Generation for Firmware 17.10
- [3169](https://bugs.linaro.org/show_bug.cgi?id=3169) can't enable sr-iov on D05 with Intel 82599. D05 does not support SR-IOV and IOMMU features for this release, which influence DPDK functionality on D05 platform.

### Cavium (ThunderX)
- [3049](https://bugs.linaro.org/show_bug.cgi?id=3049) [Thunderx] BMC ignores bootdev
- [3399](https://bugs.linaro.org/show_bug.cgi?id=3399) [ThunderX] thunderx_zip driver is crashing during boot (workaround applied)

### HP Moonshot (m400)
- [3499](https://bugs.linaro.org/show_bug.cgi?id=3499) [HP-m400] CPU\*: failed to boot: -6

### DPDK is released with the following known bugs:
- [3158](https://bugs.linaro.org/show_bug.cgi?id=3158) [ThunderX] DPDK PMD driver has non complete implementation for VLAN API

#### DPDK notes
- The DPDK release notes are available at [DPDK site](http://dpdk.org/doc/guides/rel_notes/release_17_11.html)
- Only platform specific bugs have been found, please check the 'known bugs' section above.

