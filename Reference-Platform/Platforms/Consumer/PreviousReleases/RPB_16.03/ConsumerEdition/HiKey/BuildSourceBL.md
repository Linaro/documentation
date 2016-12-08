## Building from source

The source code is available from:
- [**l-loader**](https://github.com/96boards-hikey/l-loader)
- [**ARM Trusted Firmware**](https://github.com/96boards-hikey/arm-trusted-firmware)
- [**Tianocore EDK2 – UEFI**](https://github.com/96boards-hikey/edk2) and [**OpenPlatformPkg**](https://github.com/96boards-hikey/OpenPlatformPkg)

Since GRUB2 is currently consumed directly from the Debian package, debian package rebuild instructions applies.

### Build instructions

Prerequisites:
- GCC 5.3 cross-toolchain for Aarch64 available in your PATH
   - You can download and use the Linaro GCC binary (Linaro GCC 5.3-2016.02), available at [http://releases.linaro.org/components/toolchain/binaries/5.3-2016.02/aarch64-linux-gnu/gcc-linaro-5.3-2016.02-x86_64_aarch64-linux-gnu.tar.xz](http://releases.linaro.org/components/toolchain/binaries/5.3-2016.02/aarch64-linux-gnu/gcc-linaro-5.3-2016.02-x86_64_aarch64-linux-gnu.tar.xz)
- GCC 5.3 cross-toolchain for gnueabihf available in your PATH
   - You can download and use the Linaro GCC binary (Linaro GCC 5.3-2016.02), available at [http://releases.linaro.org/components/toolchain/binaries/5.3-2016.02/arm-linux-gnueabihf/gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf.tar.xz](http://releases.linaro.org/components/toolchain/binaries/5.3-2016.02/arm-linux-gnueabihf/gcc-linaro-5.3-2016.02-x86_64_arm-linux-gnueabihf.tar.xz)
- GPT fdisk (gdisk package from your favorite distribution).

#### Installing pre-built toolchain(s)

```shell
mkdir arm-tc arm64-tc
tar --strip-components=1 -C ${PWD}/arm-tc -xf gcc-linaro-5.3-*arm-linux-gnueabihf.tar.xz
tar --strip-components=1 -C ${PWD}/arm64-tc -xf gcc-linaro-5.3-*aarch64-linux-gnu.tar.xz
export PATH="${PWD}/arm-tc/bin:${PWD}/arm64-tc/bin:$PATH"
```

#### Getting the source code

```shell
git clone -b hikey-aosp https://github.com/96boards-hikey/edk2.git
git clone -b hikey-aosp https://github.com/96boards-hikey/OpenPlatformPkg.git
git clone -b hikey https://github.com/96boards-hikey/arm-trusted-firmware.git
git clone https://github.com/96boards-hikey/l-loader.git
git clone git://git.linaro.org/uefi/uefi-tools.git
```

#### Building EDK2/UEFI for HiKey

Building EDK2/UEFI is simple if built with the _uefi-tools.sh_ script, since it already incorporates the platform specific configs and binaries.

To build EDK2/UEFI (use **-b** to select **RELEASE** or **DEBUG** build):

```shell
export AARCH64_TOOLCHAIN=GCC49
export EDK2_DIR=${PWD}/edk2
export ATF_DIR=${PWD}/arm-trusted-firmware
export UEFI_TOOLS_DIR=${PWD}/uefi-tools
cd ${EDK2_DIR}
rmdir OpenPlatformPkg; ln -s ../OpenPlatformPkg
${UEFI_TOOLS_DIR}/uefi-build.sh -b RELEASE -a ${ATF_DIR} hikey
```

And bl1.bin with l-loader (ptable files are also created as part of the l-loader Makefile):

```shell
cd ../l-loader
ln -s ${EDK2_DIR}/Build/HiKey/RELEASE_GCC49/FV/bl1.bin
ln -s ${EDK2_DIR}/Build/HiKey/RELEASE_GCC49/FV/fip.bin
make # requires sudo for creating the partition tables
```

The files 'fip.bin', 'l-loader.bin' and 'ptable-linux-8g.img' are now built. All the image files are in _$BUILD/l-loader_ directory. The Fastboot App is at _edk2/Build/HiKey/RELEASE_GCC49/AARCH64/AndroidFastbootApp.efi_.

#### EFI boot partition

The boot partition is a 64MB FAT partition only contains fastboot.efi and GRUB2, since the grub.cfg, kernel, initrd and device tree are all loaded from the root file system (grubaa64.efi searches for rootfs label/boot/grub/grub.cfg).

```shell
wget https://builds.96boards.org/snapshots/reference-platform/components/grub/latest/grubaa64.efi
mkdir boot-fat
dd if=/dev/zero of=boot-fat.uefi.img bs=512 count=131072
sudo mkfs.fat -n "boot" boot-fat.uefi.img
sudo mount -o loop,rw,sync boot-fat.uefi.img boot-fat
sudo mkdir -p boot-fat/EFI/BOOT
sudo cp ${EDK2_DIR}/Build/HiKey/RELEASE_GCC49/AARCH64/AndroidFastbootApp.efi boot-fat/EFI/BOOT/fastboot.efi
sudo cp grubaa64.efi boot-fat/EFI/BOOT/grubaa64.efi
sudo umount boot-fat
sudo mv boot-fat.uefi.img hikey-boot-linux-VERSION.uefi.img
rm -rf boot-fat
```

Now just flash the recently created 'hikey-boot-linux-VERSION.uefi.img' with the same instructions as used with the pre-built binaries.
