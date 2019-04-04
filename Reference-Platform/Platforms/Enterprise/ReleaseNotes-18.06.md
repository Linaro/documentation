# 18.06 Release Notes - Linaro Enterprise Reference Platform

The goal of the Linaro Enterprise Reference Platform is to provide a fully tested, end to end, documented, open source implementation for ARM based Enterprise servers. The Reference Platform includes kernel, a community supported userspace and additional relevant open source projects, and is validated against existing firmware releases. The Linaro Enterprise Reference Platform is built and tested on Linaro Enterprise Group members hardware and the Linaro Developer Cloud. It is intended to be a reference example for use as a foundation for members and partners for their products based on open source technologies. The members and partners to include distribution, hyperscaler or OEM/ODM vendors, can leverage the reference for ARM in the datacenter.

## Reference Platform Kernel

- 4.16 based, including backports of already upstreamed patches to fix issues discovered during QA.
- Unified tree, used by both the CentOS and Debian Reference Platforms
- Focused on ACPI and UEFI use-cases.
- Single kernel config and binary (package) for all hardware platforms
- kdump partially functional on limited platforms (see bugs below)

## Debian

- Network Installer based on Debian 9.2 "Stretch"
- Unified Reference Platform Kernel based on 4.16

## Enterprise Components
- Apache Bigtop upstream (Hadoop v2.8.4, Zookeeper v3.4.6 Spark v2.2.1, Hive v2.3.3 HBase v1.3.2 Ambari v2.6.1)
- ElasticStack / ELK v5.6.9 (from upstream)
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
- [3778](https://bugs.linaro.org/show_bug.cgi?id=3778) [Thunderx] Libvirt failure on cavium/thunder-nicvf


### HP Moonshot (m400)

### All
- [3619](https://bugs.linaro.org/show_bug.cgi?id=3619) No support virt_type=kvm and cpu_mode=host-passthrough
- [3876](https://bugs.linaro.org/show_bug.cgi?id=3876) Command exec on kolla_toolbox container fails sporadically
- [3901](https://bugs.linaro.org/show_bug.cgi?id=3901) OpenStack: cannot login/ping Cirros VM via floating ip (multi-node deployment)
