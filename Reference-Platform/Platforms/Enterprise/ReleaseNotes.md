# 17.08 Release Notes - Enterprise Reference Platform

## Reference Platform Kernel

- 4.12 based, including under-review topic branches to extend hardware platform support
- Unified tree, used by both the CentOS and Debian Reference Platforms
- ACPI and PCIe support
- Single kernel config and binary (package) for all hardware platforms

## UEFI

- Tianocore EDK II and OpenPlatformPkg containing reference implementations for Huawei D03/D05

## Debian

- Network Installer based on Debian 8.6 "Jessie"
- Unified Reference Platform Kernel based on 4.12

## Enterprise Components
- Docker 1.12.6
- ERP 17.08 OpenStack Newton
  - New Swift package released
- Ceph 10.2.7
- Spark 2.0
- OpenJDK 8
- Libvirt 3.4.0
- QEMU 2.9
- DPDK 17.05

## Supported Hardware Platforms

- HiSilicon D03
- HiSilicon D05
- Qualcomm Amberwing
- Cavium Thunder X

## Known Issues

### Debian:

### Qualcomm Amberwing:
- [3174](https://bugs.linaro.org/show_bug.cgi?id=3174) Memory corruption.
  This is trivially triggered and prevents any real use of Docker on the platform.
### D05:

- [3174](https://bugs.linaro.org/show_bug.cgi?id=3174) Memory corruption.
  This is trivially triggered and prevents any real use of Docker on the platform.
- [3169](https://bugs.linaro.org/show_bug.cgi?id=3169) DPDK: can't enable sr-iov on D05 with Intel 82599

### D03:
- [3174](https://bugs.linaro.org/show_bug.cgi?id=3174) Memory corruption.
  This is trivially triggered and prevents any real use of Docker on the platform.

### Cavium (ThunderX)
- [3174](https://bugs.linaro.org/show_bug.cgi?id=3174) Memory corruption.
  This is trivially triggered and prevents any real use of Docker on the platform.
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

### Big Data components are released with the following known bugs:

