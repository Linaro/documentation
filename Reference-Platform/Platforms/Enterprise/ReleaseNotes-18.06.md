# 18.06 Release Notes - Linaro Enterprise Reference Platform

The goal of the Linaro Enterprise Reference Platform is to provide a fully tested, end to end, documented, open source implementation for ARM based Enterprise servers. The Reference Platform includes kernel, a community supported userspace and additional relevant open source projects, and is validated against existing firmware releases. The Linaro Enterprise Reference Platform is built and tested on Linaro Enterprise Group members hardware and the Linaro Developer Cloud. It is intended to be a reference example for use as a foundation for members and partners for their products based on open source technologies. The members and partners to include distribution, hyperscaler or OEM/ODM vendors, can leverage the reference for ARM in the datacenter.

## Reference Platform Kernel

- 4.16 based, including backports of already upstreamed patches to fix issues discovered during QA.
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
- Libvirt 4.3.0
- QEMU 2.12

## Supported Hardware Platforms

- Cavium ThunderX
- HiSilicon D03
- HiSilicon D05
- HP Proliant m400
- Qualcomm Centriq 2400

## Known Issues

### Bootloader

### RPK:

### Qualcomm Centriq 2400:
- [3894](https://bugs.linaro.org/show_bug.cgi?id=3894) [centriq-2400] kdump fails to generate dump file
- [3759](https://bugs.linaro.org/show_bug.cgi?id=3759) [centriq-2400] lscpu shows unknown cache size


### D05:
- [3896](https://bugs.linaro.org/show_bug.cgi?id=3896) [d05] kdump doesn't work with lots of memory

### Cavium (ThunderX)
- [3049](https://bugs.linaro.org/show_bug.cgi?id=3049) [Thunderx] BMC ignores bootdev
- [3891](https://bugs.linaro.org/show_bug.cgi?id=3891) [Thunderx] lscpu shows unknown cache size


### HP Moonshot (m400)

### All
- [3880](https://bugs.linaro.org/show_bug.cgi?id=3880) kdump fails to load kdump kernel
- [3876](https://bugs.linaro.org/show_bug.cgi?id=3876) Command exec on kolla_toolbox container fails sporadically
- [3901](https://bugs.linaro.org/show_bug.cgi?id=3901) OpenStack: cannot login/ping Cirros VM via floating ip (multi-node deployment)
