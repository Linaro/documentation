# 16.12 Release Notes - Enterprise Reference Platform

## Reference Platform Kernel

- 4.9 based, including under-review topic branches to extend hardware platform support
- Unified tree, used by both the CentOS and Debian Reference Platforms
- ACPI and PCIe support
- Single kernel config and binary (package) for all hardware platforms

## UEFI

- Tianocore EDK II and OpenPlatformPkg containing reference implementations for Huawei D03/D05 and AMD Overdrive

## Debian

- Network Installer based on Debian 8.6 "Jessie"
- Unified Reference Platform Kernel based on 4.9

## CentOS

- Network Installer based on CentOS 7.2 16.03
- Unified Reference Platform Kernel based on 4.9

## Enterprise Components
- Docker 1.10.3
- OpenStack Mitaka
- Ceph 10.2.3
- Spark 2.0
- OpenJDK 8
- QEMU 2.7

## Supported Hardware Platforms

- AMD Overdrive
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

- [2801](https://bugs.linaro.org/show_bug.cgi?id=2801) sort out qemu/qemu-ev packaging in linaro-overlay for CentOS 7
- [2778](https://bugs.linaro.org/show_bug.cgi?id=2778) qemu-system-aarch64 doesn't pull ipxe-roms-qemu package
- [2743](https://bugs.linaro.org/show_bug.cgi?id=2743) LTP: growfiles test cases failed
- [2728](https://bugs.linaro.org/show_bug.cgi?id=2728) openssl test suite test_req and test_verify failed exit major All
- [2725](https://bugs.linaro.org/show_bug.cgi?id=2725) No source RPM found for openssh-6.6.1p1-25.el7.aarch64 major All
- [2720](https://bugs.linaro.org/show_bug.cgi?id=2720) CentOS/Debian: CONFIG_ARM64_VA_BITS=48 breaks userspace
- [2701](https://bugs.linaro.org/show_bug.cgi?id=2701) libhugetlbfs test cases counters.sh heap-overflow and heapshrink failed
- [2694](https://bugs.linaro.org/show_bug.cgi?id=2694) CentOS: overlayfs: upper fs needs to support d_type
- [2681](https://bugs.linaro.org/show_bug.cgi?id=2681) Install will fail if RTC date is incorrect due to https
- [2655](https://bugs.linaro.org/show_bug.cgi?id=2655) kickstart file not available in ISO image
- [2363](https://bugs.linaro.org/show_bug.cgi?id=2363) LTP: # profil01 profil failed to record anything
- [2356](https://bugs.linaro.org/show_bug.cgi?id=2356) LTP: open14.c check file permissions failed normal All
- [2351](https://bugs.linaro.org/show_bug.cgi?id=2351) LTP: openat03 TBROK errno=EINVAL(22) Invalid argument normal All

### QDF2432:

- [2663](https://bugs.linaro.org/show_bug.cgi?id=2663) Debian: 'qemu-sys -  tem-aarch64 -cpu host' causes host reboot
- [2646](https://bugs.linaro.org/show_bug.cgi?id=2646) LTP: Kernel crashes in syscalls/ipc/msgctl10 test
- [2603](https://bugs.linaro.org/show_bug.cgi?id=2603) No support for on-board ethernet (emac)

### D05:

- [2788](https://bugs.linaro.org/show_bug.cgi?id=2788) kernel oops during hns_dsaf probe
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

### HP-m400 Moonshot:

- [2703](https://bugs.linaro.org/show_bug.cgi?id=2703) Boot medium is not updated after network installer

### Cavium (ThunderX)

- [2773](https://bugs.linaro.org/show_bug.cgi?id=2773) UEFI Runtime regions are not aligned to 64 KB - buggy firmware?
- [2736](https://bugs.linaro.org/show_bug.cgi?id=2736) UEFI boot entry not created after Debian installer finished
- [2700](https://bugs.linaro.org/show_bug.cgi?id=2700) LTP test case oom01 restarted serial-getty and NetworkManager.service
- [2332](https://bugs.linaro.org/show_bug.cgi?id=2332) fork: Resource temporarily unavailable when building linux normal ThunderX

### The SDI (OpenStack) components are released with the following known bugs:

- [2819](https://bugs.linaro.org/show_bug.cgi?id=2819) "Volume hot plug not working in 16.12" - This is a known issue with this release that hasn't been fixed.
- [2805](https://bugs.linaro.org/show_bug.cgi?id=2805) "dhcp lease is not freed after instance deleted" - this was a regression on the kernel that is being fixed. A new validated kernel will land next week with this fix in it.
- [2549](https://bugs.linaro.org/show_bug.cgi?id=2549) "Ethernet hotplug fails on 16.06" - this is a known bug that hasn't been fixed for this release.

