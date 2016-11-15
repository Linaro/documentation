h3. Boot Loader

Please see "https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey#building-from-source":https://github.com/96boards/documentation/wiki/Reference-Bootloader-Hikey#building-from-source for instructions on how to built the boot loader from source.

h3. How to get and customize the kernel source code

h4. Building the Linux kernel from source

The Linux kernel used in this release is available via tags in the git "repository":https://github.com/rsalveti/linux

bc. git: https://github.com/rsalveti/linux
tag: 96boards-rpb-debian-15.12-hikey
defconfig: arch/arm64/defconfig kernel/configs/distro.config

The kernel image (@Image@) is located in the @/boot@ directory from the system partition (rootfs), with the modules also installed in the root file system. It is possible for a user to rebuild the kernel and run a custom kernel image instead of the released kernel. You can build the kernel using any recent GCC release using the git tree, tag and defconfig mentioned above. This release only supports booting with device tree, as such both the device tree blobs need to be built as well.

The HiKey is an ARMv8 platform, and the kernel is compiled for the Aarch64 target. Even though it is possible to build natively, on the target board, It is recommended to build the Linux kernel on a PC development host. In which case you need to install a cross compiler for the ARM architecture. It is recommended to download the "Linaro GCC cross compiler (Aarch64 little-endian)":http://releases.linaro.org/14.11/components/toolchain/binaries/aarch64-linux-gnu/gcc-linaro-4.9-2014.11-x86_64_aarch64-linux-gnu.tar.xz.

To build the Linux kernel, you can use the following instructions:

bc. git clone -n https://github.com/rsalveti/linux.git
cd linux
git checkout -b kernel-rpb-15.12 96boards-rpb-debian-15.12-hikey
export ARCH=arm64
export CROSS_COMPILE=<path to your GCC cross compiler>/aarch64-linux-gnu-
make defconfig distro.config
make -j4 Image dtbs KERNELRELEASE=4.3.0-your-custom-release

To boot using your own kernel, simply copy the kernel, modules and device tree to the root file system (similar to desktops), and create your own grub entry at @/boot/grub/grub.cfg@.

h4. How to get and customize Debian packages source code

This release is based on Debian 8.2 "Jessie".

Since all packages installed in Linaro Debian-based images are maintained either in Debian archives or in Linaro repositories, it is possible for users to update their environment with commands such as:

bc. sudo apt-get update
sudo apt-get upgrade

All user space software is packaged using Debian packaging process. As such you can find extensive information about using, patching and building packages in The Debian New Maintainers Guide. If you quickly want to rebuild any package, you can run the following commands to fetch the package source code and install all build dependencies:

bc. sudo apt-get update
sudo apt-get build-dep <pkg>
apt-get source <pkg>

Then you can rebuild the package locally with:

bc. cd <pkg-version>
dpkg-buildpackage -b -us -uc

h4. TODO

* Explain how to build the rootfs from source