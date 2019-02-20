### How to get and customize the kernel source code

#### Building the Linux kernel from source

The Linux kernel used in this release is available via tags in the git [repository](https://github.com/rsalveti/linux)

```
git: https://github.com/rsalveti/linux
tag: 96boards-rpb-debian-15.10-qcom
defconfig: arch/arm64/defconfig kernel/configs/distro.config
```

The kernel image (`Image`) is located in the `boot` image and partition and the kernel modules are installed in the root file system. It is possible for a user to rebuild the kernel and run a custom kernel image instead of the released kernel. You can build the kernel using any recent GCC release using the git tree, tag and defconfig mentioned above. This release only supports booting with device tree, as such both the device tree blobs need to be built as well.

The DragonBoard 410c is an ARMv8 platform, and the kernel is compiled for the Aarch64 target. Even though it is possible to build natively, on the target board, It is recommended to build the Linux kernel on a PC development host. In which case you need to install a cross compiler for the ARM architecture. It is recommended to download the [Linaro GCC cross compiler (Aarch64 little-endian)](https://releases.linaro.org/14.11/components/toolchain/binaries/aarch64-linux-gnu/gcc-linaro-4.9-2014.11-x86_64_aarch64-linux-gnu.tar.xz).

To build the Linux kernel, you can use the following instructions:

```
git clone -n https://github.com/rsalveti/linux.git
cd linux
git checkout -b kernel-rpb-15.10 96boards-rpb-debian-15.10-qcom
export ARCH=arm64
export CROSS_COMPILE=<path to your GCC cross compiler>/aarch64-linux-gnu-
make defconfig distro.config
make -j4 Image dtbs KERNELRELEASE=4.3.0-your-custom-release
```

#### Building a boot image

You now need to create a valid boot image with your own kernel build.

On your host PC, we need to install the following tools:

```
sudo apt-get install device-tree-compiler
git clone git://codeaurora.org/quic/kernel/skales
```

The boot image consists of the table of device tree (`dt.img`), the kernel image (`Image`) and an init ramdisk image.

The `dtbTool` is a standalone application that will process the DTBs generated during the kernel build, to create the table of device tree image. This tool is included in the @skales@ git tree cloned above.

`./skales/dtbTool -o dt.img -s 2048 arch/arm64/boot/dts/qcom/`

To create the boot image, you also need a ramdisk image, and you can use the one available at _/boot_ from the rootfs.

The tool `mkbootimg` (also in the git tree previously cloned) is a standalone application that will process all files and create the boot image that can then be booted on the target board, or flash into the on-board eMMC. The boot image also contains the kernel bootargs, which can be changed as needed in the next command:

```
./skales/mkbootimg --kernel arch/arm64/boot/Image \
                   --ramdisk initrd.img \
                   --output boot-db410c.img \
                   --dt dt.img \
                   --pagesize 2048 \
                   --base 0x80000000 \
                   --cmdline "root=/dev/disk/by-partlabel/rootfs rw rootwait console=ttyMSM0,115200n8"
```

#### Booting a custom boot image

Assuming you have now built a valid boot image called `boot-db410c.img`, you can run the following `fastboot` command to boot it on the board:

`sudo fastboot boot boot-db410c.img`

If you want to permanently use a custom kernel image, you can update the boot image and reflash it into the `boot` partition:

`sudo fastboot flash boot boot-db410c.img`

#### How to get and customize the bootloader

While the first stage bootloader is proprietary and released as firmware blob available on [Qualcomm Developer Network](https://developer.qualcomm.com/download/linux-ubuntu-board-support-package-v1.zip), the second stage bootloader is `LK` and is open source.

The original LK source code is available on "CodeAurora.org":https://www.codeaurora.org/cgit/quic/la/kernel/lk/, and the source code which is used in this release can be found in the [Linaro Qualcomm Landing Team git repository](https://git.linaro.org/landing-teams/working/qualcomm/lk.git):

```
git: https://git.linaro.org/landing-teams/working/qualcomm/lk.git
tag: ubuntu-qcom-dragonboard410c-LA.BR.1.2.4-00310-8x16.0-linaro1
```

To build the LK bootloader, you can use the following instructions:

```
git clone git://codeaurora.org/platform/prebuilts/gcc/linux-x86/arm/arm-eabi-4.8.git -b LA.BR.1.1.3.c4-01000-8x16.0
git clone https://git.linaro.org/landing-teams/working/qualcomm/lk.git -b ubuntu-qcom-dragonboard410c-LA.BR.1.2.4-00310-8x16.0-linaro1
cd lk
make -j4 msm8916 EMMC_BOOT=1 TOOLCHAIN_PREFIX=<path to arm-eabi-4.8 tree>/bin/arm-eabi-
```

The second stage bootloader is flashed on the `aboot` partition, you can now flash your board with:

`sudo fastboot aboot ./build-msm8916/emmc_appsboot.mbn`

#### How to get and customize Debian packages source code

This release is based on Debian 8.2 "Jessie".

Since all packages installed in Linaro Debian-based images are maintained either in Debian archives or in Linaro repositories, it is possible for users to update their environment with commands such as:

```
sudo apt-get update
sudo apt-get upgrade
```

All user space software is packaged using Debian packaging process. As such you can find extensive information about using, patching and building packages in The Debian New Maintainers Guide. If you quickly want to rebuild any package, you can run the following commands to fetch the package source code and install all build dependencies:

```
sudo apt-get update
sudo apt-get build-dep <pkg>
apt-get source <pkg>
```

Then you can rebuild the package locally with:

```
cd <pkg-version>
dpkg-buildpackage -b -us -uc
```

#### TODO

- Explain how to build the rootfs from source
