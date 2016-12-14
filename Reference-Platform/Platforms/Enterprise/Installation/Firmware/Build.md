<table align="center">
<tr>
    <td align="center">UEFI/EDK2<br><a href="../README.md">Go Back</a></td>
    <td align="center"><a href="">Download</a><br>Get the latest pre-built firmware images</td>
    <th align="center"><a href="Build.md">Build</a><br>Instructions for building latest firmware images</td>
    <td align="center"><a href="Install.md">Install</a><br>Instructions on how to install firmware</td>
    <td align="center"><a href="README.md">Read more</a><br>Learn more about UEFI/EDK2</td>
</tr>
</table>

## Pre-Requisites

Make sure the build dependencies are available at your host machine.

On Debian/Ubuntu:

```shell
sudo apt-get install uuid-dev build-essential aisle
```

On RHEL/CentOS/Fedora:

```shell
sudo yum install uuid-devel libuuid-devel aisle
```

Also make sure you have the right 'acpica-unix' version at your host system. The current one required by the 16.03/16.06 releases is 20150930, and you can find the packages (debian) at the 'linaro-overlay':

```shell
wget http://repo.linaro.org/ubuntu/linaro-overlay/pool/main/a/acpica-unix/acpica-tools_20150930-1.linarojessie.1_amd64.deb
wget http://repo.linaro.org/ubuntu/linaro-overlay/pool/main/a/acpica-unix/acpidump_20150930-1.linarojessie.1_all.deb
wget http://repo.linaro.org/ubuntu/linaro-overlay/pool/main/a/acpica-unix/iasl_20150930-1.linarojessie.1_all.deb
sudo dpkg -i --force-all *.deb
```

If cross compiling, you also need to separately add the required toolchains. Ubuntu has a prebuilt arm-linux-gnueabihf toolchain, but not an aarch64-linux-gnu one.

Download Linaro's GCC 4.9 cross-toolchain for Aarch64, and make it available in your 'PATH'. You can download and use the Linaro GCC binary (Linaro GCC 4.9-2015.02), available at [http://releases.linaro.org/15.02/components/toolchain/binaries/aarch64-linux-gnu/gcc-linaro-4.9-2015.02-3-x86_64_aarch64-linux-gnu.tar.xz](http://releases.linaro.org/15.02/components/toolchain/binaries/aarch64-linux-gnu/gcc-linaro-4.9-2015.02-3-x86_64_aarch64-linux-gnu.tar.xz)

```shell
mkdir arm-tc arm64-tc
tar --strip-components=1 -C ${PWD}/arm-tc -xf gcc-linaro-arm-linux-gnueabihf-4.9-*_linux.tar.xz
tar --strip-components=1 -C ${PWD}/arm64-tc -xf gcc-linaro-aarch64-linux-gnu-4.9-*_linux.tar.xz
export PATH="${PWD}/arm-tc/bin:${PWD}/arm64-tc/bin:$PATH"
```

## Getting the source code

UEFI/EDK2:

```shell
git clone https://github.com/tianocore/edk2.git
git clone https://git.linaro.org/uefi/OpenPlatformPkg.git
cd edk2
git checkout -b stable-baseline 469e1e1e4203b5d369fdce790883cb0aa035a744 # revision provided by https://git.linaro.org/uefi/linaro-edk2.git
ln -s ../OpenPlatformPkg
```

ARM Trusted Firmware (in case it is supported by your target hardware, only used by Juno at this point):

```shell
git clone https://github.com/ARM-software/arm-trusted-firmware.git
cd arm-trusted-firmware
git checkout -b stable-baseline v1.2 # suggested latest stable release
```

UEFI Tools (helpers and scripts to make the build process easy):

```shell
git clone git://git.linaro.org/uefi/uefi-tools.git
```

## Building UEFI/EDK2 for Juno R0/R1

```shell
export AARCH64_TOOLCHAIN=GCC49
export LINARO_EDK2_DIR=${PWD}/edk2
export UEFI_TOOLS_DIR=${PWD}/uefi-tools
export ARMTF_DIR=${PWD}/arm-trusted-firmware
cd ${LINARO_EDK2_DIR}
${UEFI_TOOLS_DIR}/uefi-build.sh -b DEBUG -a $ARMTF_DIR juno
```

The output files:

- `Build/ArmJuno/DEBUG_GCC49/FV/bl1.bin`
- `Build/ArmJuno/DEBUG_GCC49/FV/fip.bin`

### Proceed to [Installation](Install.md) page

***

## Building UEFI/EDK2 for D02

```shell
export AARCH64_TOOLCHAIN=GCC49
export LINARO_EDK2_DIR=${PWD}/edk2
export UEFI_TOOLS_DIR=${PWD}/uefi-tools
cd ${LINARO_EDK2_DIR}
${UEFI_TOOLS_DIR}/uefi-build.sh -b DEBUG d02
```

The output file:

- `Build/Pv660D02/DEBUG_GCC49/FV/PV660D02.fd`

### Proceed to [Installation](Install.md) page

***

## Building UEFI/EDK2 for D03

```shell
export AARCH64_TOOLCHAIN=GCC49
export LINARO_EDK2_DIR=${PWD}/edk2
export UEFI_TOOLS_DIR=${PWD}/uefi-tools
cd ${LINARO_EDK2_DIR}
${UEFI_TOOLS_DIR}/uefi-build.sh -b DEBUG d03
```

The output file:

- `Build/D03/DEBUG_GCC49/FV/D03.fd`

### Proceed to [Installation](Install.md) page

***

## Building UEFI/EDK2 for Overdrive

```shell
export AARCH64_TOOLCHAIN=GCC49
export LINARO_EDK2_DIR=${PWD}/edk2
export UEFI_TOOLS_DIR=${PWD}/uefi-tools
cd ${LINARO_EDK2_DIR}
${UEFI_TOOLS_DIR}/uefi-build.sh -b DEBUG overdrive
```

The output file:

- `Build/Overdrive/DEBUG_GCC49/FV/STYX_ROM.fd`

### Proceed to [Installation](Install.md) page

***

## Building UEFI/EDK2 for HuskyBoard / Cello

```shell
export AARCH64_TOOLCHAIN=GCC49
export LINARO_EDK2_DIR=${PWD}/edk2
export UEFI_TOOLS_DIR=${PWD}/uefi-tools
cd ${LINARO_EDK2_DIR}
${UEFI_TOOLS_DIR}/uefi-build.sh -b DEBUG cello
```

The output file:

- `Build/Cello/DEBUG_GCC49/FV/STYX_ROM.fd`

### Proceed to [Installation](Install.md) page

***
