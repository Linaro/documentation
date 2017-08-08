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
- Docker 1.10.3
- OpenStack Mitaka
- Ceph 10.2.3
- Spark 2.0
- OpenJDK 8
- QEMU 2.7

## Supported Hardware Platforms

- HiSilicon D03
- HiSilicon D05
- Qualcomm Amberwing
- Cavium Thunder X

## Known Issues

### Debian:

- [2748](https://bugs.linaro.org/show_bug.cgi?id=2748) LTP: containers sub test case netns_breakns and netns_comm failed
- [2747](https://bugs.linaro.org/show_bug.cgi?id=2747) LTP test case isofs failed on D03, Thunderx and moonshot running Debian
- [2745](https://bugs.linaro.org/show_bug.cgi?id=2745) LTP: quota_remount_test01 failed on Debian Running on D03, moonshot and Thunderx platforms.
- [2744](https://bugs.linaro.org/show_bug.cgi?id=2744) LTP: writev01, writev03, writev04 test cases failed 
- [2720](https://bugs.linaro.org/show_bug.cgi?id=2720) CentOS/Debian: CONFIG_ARM64_VA_BITS=48 breaks userspace
- [2351](https://bugs.linaro.org/show_bug.cgi?id=2351) LTP: openat03 TBROK errno=EINVAL(22) Invalid argument

### Qualcomm Amberwing:

- [2663](https://bugs.linaro.org/show_bug.cgi?id=2663) Debian: 'qemu-sys -  tem-aarch64 -cpu host' causes host reboot
- [2646](https://bugs.linaro.org/show_bug.cgi?id=2646) LTP: Kernel crashes in syscalls/ipc/msgctl10 test
- [2603](https://bugs.linaro.org/show_bug.cgi?id=2603) No support for on-board ethernet (emac)

### D05:

- [2788](https://bugs.linaro.org/show_bug.cgi?id=2788) kernel oops during hns_dsaf probe
- [2786](https://bugs.linaro.org/show_bug.cgi?id=2786) raid0 performance worse on debian comparing to centos
- [2770](https://bugs.linaro.org/show_bug.cgi?id=2770) UEFI occasionally hangs
- [2715](https://bugs.linaro.org/show_bug.cgi?id=2715) CentOS: ngingx-apache-bench reports request failures
- [2712](https://bugs.linaro.org/show_bug.cgi?id=2712) network port is slow when using encrypted connection (SSH)
- [2657](https://bugs.linaro.org/show_bug.cgi?id=2657) Confusing Ethernet port sequence

### D03:

- [2792](https://bugs.linaro.org/show_bug.cgi?id=2792) BMC bootdevice doesn't work with UEFI
- [2788](https://bugs.linaro.org/show_bug.cgi?id=2788) kernel oops during hns_dsaf probe
- [2774](https://bugs.linaro.org/show_bug.cgi?id=2774) System stucked when manually enabled function graph tracer and echo function_graph > /sys/kernel/debug/tracing/current_tracer
- [2732](https://bugs.linaro.org/show_bug.cgi?id=2732) LuvOS next doesn't boot on d03
- [2699](https://bugs.linaro.org/show_bug.cgi?id=2699) Earlycon not working (no console output)
- [2661](https://bugs.linaro.org/show_bug.cgi?id=2661) Fails to install/boot Debian/CentOS with the default boot argument
- [2657](https://bugs.linaro.org/show_bug.cgi?id=2657) Confusing Ethernet port sequence
- [2635](https://bugs.linaro.org/show_bug.cgi?id=2635) Only 16 cores enabled (instead of 32)

### Cavium (ThunderX)

- [2773](https://bugs.linaro.org/show_bug.cgi?id=2773) UEFI Runtime regions are not aligned to 64 KB - buggy firmware?
- [2736](https://bugs.linaro.org/show_bug.cgi?id=2736) UEFI boot entry not created after Debian installer finished
- [2700](https://bugs.linaro.org/show_bug.cgi?id=2700) LTP test case oom01 restarted serial-getty and NetworkManager.service

### The SDI (OpenStack) components are released with the following known bugs:

- [2819](https://bugs.linaro.org/show_bug.cgi?id=2819) "Volume hot plug not working in 17.08" - This is a known issue with this release that hasn't been fixed.
- [2805](https://bugs.linaro.org/show_bug.cgi?id=2805) "dhcp lease is not freed after instance deleted" - this was a regression on the kernel that is being fixed. A new validated kernel will land next week with this fix in it.
- [2549](https://bugs.linaro.org/show_bug.cgi?id=2549) "Ethernet hotplug fails on 16.06" - this is a known bug that hasn't been fixed for this release.

### Big Data components are released with the following known bugs:

- [3174](https://bugs.linaro.org/show_bug.cgi?id=3174) Memory corruption on Moonshot cartridge
