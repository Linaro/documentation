# 17.12 Release Notes - Linaro Enterprise Reference Platform

The goal of the Linaro Enterprise Reference Platform is to provide a fully tested, end to end, documented, open source implementation for ARM based Enterprise servers. The Reference Platform includes boot firmware, kernel, a community supported userspace and additional relevant open source projects. The Linaro Enterprise Reference Platform is built and tested on Linaro Enterprise Group members hardware and the Linaro Developer Cloud. It is intended to be a reference example for use as a foundation for members and partners for their products based on open source technologies. The members and partners to include distribution, hyperscaler or OEM/ODM vendors, can leverage the reference for ARM in the datacenter.

## Reference Platform Kernel

- 4.14 based, including under-review topic branches to extend hardware platform support
- Unified tree, used by both the CentOS and Debian Reference Platforms
- ACPI and PCIe support
- Single kernel config and binary (package) for all hardware platforms

## UEFI

- Tianocore EDK II and OpenPlatformPkg containing reference implementations for Huawei D03/D05

## Debian

- Network Installer based on Debian 9.2 "Stretch"
- Unified Reference Platform Kernel based on 4.14

## Enterprise Components
- Docker 1.12.6
- Ceph 12.2
- Bigtop 1.2 stack (Hadoop 2.7.3, Spark 2.1, Hive 1.1.3)
- ELK 5.4.1
- OpenJDK 8
- Libvirt 3.8.0
- QEMU 2.10
- DPDK 17.08

## Supported Hardware Platforms

- Cavium ThunderX
- HiSilicon D03
- HiSilicon D05
- HP Proliant m400
- Qualcomm Centriq 2400

## Known Issues

### Debian:
- [3174](https://bugs.linaro.org/show_bug.cgi?id=3174) Docker triggered problem causes crash in tty subsystem. Also present on x86_64.

### Qualcomm Centriq 2400:
- [3188](https://bugs.linaro.org/show_bug.cgi?id=3188) [Centriq 2400] Unable to handle kernel NULL pointer dereference at virtual address 00000020

### D05:
- [3169](https://bugs.linaro.org/show_bug.cgi?id=3169) DPDK: can't enable sr-iov on D05 with Intel 82599

### Cavium (ThunderX)
- [3049](https://bugs.linaro.org/show_bug.cgi?id=3049) [Thunderx] BMC ignores bootdev
- [3172](https://bugs.linaro.org/show_bug.cgi?id=3172) [Inventec Cavium ThunderX] Host CPU stuck when running VM
- [3158](https://bugs.linaro.org/show_bug.cgi?id=3158) [ThunderX] DPDK PMD driver has non complete implementation for VLAN API

### The SDI (OpenStack) components are released with the following known bugs:

- [2819](https://bugs.linaro.org/show_bug.cgi?id=2819) Hotplugging network or storage does not work in VM
  This is a problem that will likely result on changes in libvirt and/or nova. Upstream conversations ongoing.
- [3157](https://bugs.linaro.org/show_bug.cgi?id=3157) OpenStack RefStack fail with Ceph rgw

### DPDK Technical Preview is released with the following known bugs:

#### DTS issues

- [3149](https://bugs.linaro.org/show_bug.cgi?id=3149) [DPDK DTS] To fix issues in ip_pipeline (DPDK 17.05)
- [3150](https://bugs.linaro.org/show_bug.cgi?id=3150) [DPDK DTS] To fix issues in kni (DPDK 17.05)
- [3154](https://bugs.linaro.org/show_bug.cgi?id=3154) [DPDK DTS] To fix issues in unit_tests_pmd_perf (DPDK 17.05)

#### DPDK issues

- [3136](https://bugs.linaro.org/show_bug.cgi?id=3136) dpdk: can't use intel x710 for dpdk testing



