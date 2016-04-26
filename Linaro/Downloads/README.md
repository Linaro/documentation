## Downloads

This page contains direct links to the latest versions of the most popular downloads from Linaro. These include a selection of builds including Android, the LAVA test framework and key toolchains.

## Linaro Member Builds

LMBs are full system builds of popular open-source products set up at the request of a Linaro Core/Club [Member](https://www.linaro.org/members/) company.

|     |     |     |
|:---|:---|:---|
|ARM| Juno, Fixed Virtual Platforms (FVP), Versatile Express | [Platform release notes](http://community.arm.com/groups/arm-development-platforms)|
|Qualcomm| Download for Snapdragon 600 processor | [Snapdragon 600 Linux Platform](https://releases.linaro.org/debian/boards/snapdragon/latest/)|

***

## Linaro Stable Kernel (LSK)

The LSK is a version of kernel.org’s Long-Term Stable (LTS) release with new Linaro developed optimizations and ARM support integrated. There are two versions: a “Core” version for generic Linux and an “Android” version. Click right for the latest downloads.

- [linux-linaro-stable (LSK), Source, Git](https://wiki.linaro.org/LSK)

***

## Linaro Confectionary Release (LCR)

R-LCR is a build of the Android Open Source Project (AOSP) from a stable “L” branch that includes platform support and other features. R-LCR includes the Android flavour of Linaro Stable Kernel (LSK) for all machine configurations.

***

## LAVA

The Linaro Automated Validation Architecture (LAVA) is a test and continuous integration framework that Linaro uses to validate its releases. The source is open so that members and others can create their own instantiations and run proprietary tests within this standard framework. [Click here for the latest downloads](https://releases.linaro.org/components/lava/latest/).

***

## Linaro Networking

Based on the Linaro Stable Kernel (LSK) and upstream, these kernels add features currently being developed by LNG and not upstreamed yet.

Release notes https://git.linaro.org/lng/releases-instructions.git

Repo https://git.linaro.org/kernel/linux-linaro-lng.git

- Latest LSK kernel for which a preempt-rt patch set has been released, plus patches that have not yet been accepted upstream and are relevant to LNG ([linux-linaro-lng-4.1](http://releases.linaro.org/components/kernel/linux-linaro-lng/16.03/linux-linaro-lng-4.1.14-2016.03.tar.bz2))
- Same as linux-linaro-lng-v4.1 but with the preempt-rt patches applied. ([linux-linaro-lng-preempt-rt-4.1](http://releases.linaro.org/components/kernel/linux-linaro-lng/16.03/linux-linaro-lng-preempt-rt-4.1.14-2016.03.tar.bz2))

***

#### OpenDataPlane

The [OpenDataPlane](http://www.opendataplane.org/) API has three implementations supported directly by LNG

- Functional reference model that runs on any linux implementation ([odp-linux-generic](https://git.linaro.org/lng/odp.git))
- Reusing odp-linux-generic and adding packet_io acceleration via Netmap ([odp-netmap](https://git.linaro.org/lng/odp-netmap.git))
- Performance implementation build for x86  using the DPDK SDK. ([odp-dpdk](https://git.linaro.org/lng/odp-dpdk.git))

*** 

## Linaro Toolchain

Linaro offers monthly updates to QEMU, GDB, toolchain components and various versions of GCC. You can access source and pre-built binaries. Click below for the latest downloads.

- linaro-toolchain-binaries (little-endian) - ([Linux](https://releases.linaro.org/components/toolchain/binaries/latest-5/arm-linux-gnueabihf/) / [Windows Archive](https://releases.linaro.org/components/toolchain/binaries/latest-5/arm-linux-gnueabihf/) / [Bare Metal](https://releases.linaro.org/components/toolchain/binaries/latest-5/arm-eabi/) / [Source](https://releases.linaro.org/components/toolchain/gcc-linaro/latest-5/) / [Sysroot](https://releases.linaro.org/components/toolchain/binaries/latest-5/arm-linux-gnueabihf/))
- linaro-toolchain-binaries (big-endian) - ([Linux](https://releases.linaro.org/components/toolchain/binaries/latest-5/armeb-linux-gnueabihf/) / [Bare Metal](https://releases.linaro.org/components/toolchain/binaries/latest-5/armeb-eabi/) / [Source](https://releases.linaro.org/components/toolchain/gcc-linaro/latest-5/) / [Sysroot](https://releases.linaro.org/components/toolchain/binaries/latest-5/armeb-linux-gnueabihf/))
- linaro-toolchain-binaries (Aarch64 little-endian) - ([Linux](https://releases.linaro.org/components/toolchain/binaries/latest-5/aarch64-linux-gnu/) / [Windows Archive](https://releases.linaro.org/components/toolchain/binaries/latest-5/aarch64-linux-gnu/) / [Bare Metal](https://releases.linaro.org/components/toolchain/binaries/latest-5/aarch64-elf/) / [Source](https://releases.linaro.org/components/toolchain/gcc-linaro/latest-5/) / [Sysroot](https://releases.linaro.org/components/toolchain/binaries/latest-5/aarch64-linux-gnu/))
- linaro-toolchain-binaries (Aarch64 big-endian) - ([Linux](https://releases.linaro.org/components/toolchain/binaries/latest-5/aarch64_be-linux-gnu/) / [Bare Metal](https://releases.linaro.org/components/toolchain/binaries/latest-5/aarch64_be-elf/) / [Source](https://releases.linaro.org/components/toolchain/gcc-linaro/latest-5/) / [Sysroot](https://releases.linaro.org/components/toolchain/binaries/latest-5/aarch64_be-linux-gnu/))

***

More interested in bare-metal and long-term maintained [releases](https://launchpad.net/gcc-arm-embedded) for ARM embedded processors? We’re working with ARM to also supply a Cortex-R and Cortex-M bare-metal build. Major releases will be made once a year with quarterly update releases. Releases will be maintained for two years. Get these from Launchpad: https://launchpad.net/gcc-arm-embedded
