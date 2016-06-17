## UEFI/EDK2

EDK2 is a modern, feature-rich, cross-platform firmware development environment for the UEFI and PI specifications.

The reference UEFI/EDK2 tree used by the EE-RPB comes directly from [upstream](https://github.com/tianocore/edk2), based on a specific commit that gets validated and published as part of the Linaro EDK2 effort (which is available at [https://git.linaro.org/uefi/linaro-edk2.git](https://git.linaro.org/uefi/linaro-edk2.git)).

Since there is no hardware specific support as part of EDK2 upstream, an external module called [OpenPlatformPkg](https://git.linaro.org/uefi/OpenPlatformPkg.git) is also required as part of the build process.

EDK2 is currently used by 96boards LeMaker Cello, AMD Overdrive, ARM Juno r0/r1/r2, HiSilicon D02 and HiSilicon D03.

This guide provides enough information on how to build UEFI/EDK2 from scratch, but meant to be a quick guide. For further information please also check the official Linaro UEFI documentation, available at [https://wiki.linaro.org/ARM/UEFI](https://wiki.linaro.org/ARM/UEFI) and  [https://wiki.linaro.org/LEG/Engineering/Kernel/UEFI/build](https://wiki.linaro.org/LEG/Engineering/Kernel/UEFI/build)

### Building

#### Pre-Requisites

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

#### Getting the source code

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

#### Building UEFI/EDK2 for Juno R0/R1

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

#### Building UEFI/EDK2 for D02

```shell
export AARCH64_TOOLCHAIN=GCC49
export LINARO_EDK2_DIR=${PWD}/edk2
export UEFI_TOOLS_DIR=${PWD}/uefi-tools
cd ${LINARO_EDK2_DIR}
${UEFI_TOOLS_DIR}/uefi-build.sh -b DEBUG d02
```

The output file:

- `Build/Pv660D02/DEBUG_GCC49/FV/PV660D02.fd`

#### Building UEFI/EDK2 for D03

```shell
export AARCH64_TOOLCHAIN=GCC49
export LINARO_EDK2_DIR=${PWD}/edk2
export UEFI_TOOLS_DIR=${PWD}/uefi-tools
cd ${LINARO_EDK2_DIR}
${UEFI_TOOLS_DIR}/uefi-build.sh -b DEBUG d03
```

The output file:

- `Build/D03/DEBUG_GCC49/FV/D03.fd`

#### Building UEFI/EDK2 for Overdrive

```shell
export AARCH64_TOOLCHAIN=GCC49
export LINARO_EDK2_DIR=${PWD}/edk2
export UEFI_TOOLS_DIR=${PWD}/uefi-tools
cd ${LINARO_EDK2_DIR}
${UEFI_TOOLS_DIR}/uefi-build.sh -b DEBUG overdrive
```

The output file:

- `Build/Overdrive/DEBUG_GCC49/FV/STYX_ROM.fd`

#### Building UEFI/EDK2 for HuskyBoard / Cello

```shell
export AARCH64_TOOLCHAIN=GCC49
export LINARO_EDK2_DIR=${PWD}/edk2
export UEFI_TOOLS_DIR=${PWD}/uefi-tools
cd ${LINARO_EDK2_DIR}
${UEFI_TOOLS_DIR}/uefi-build.sh -b DEBUG cello
```

The output file:

- `Build/Cello/DEBUG_GCC49/FV/STYX_ROM.fd`

### Flashing

#### Juno R0/R1

##### Clean flash

Power on the board, and (if prompted) press Enter to stop auto boot. Once in Juno's boot monitor, use the following commands to erase Juno's flash and export it as an external storage:

```shell
Cmd> flash
Flash> eraseall
Flash> quit
Cmd> usb_on
```

This will delete any binaries and UEFI settings currently stored in the Juno's flash, then mount the Juno's MMC card as an external storage device on your host PC.

In order to do a clean flash on Juno, you will also need to flash the firmware provided by ARM, which can be downloaded from the Linaro ARM LT Versatile Express Firmware git tree:

```shell
git clone -b juno-0.11.6-linaro1 --depth 1 https://git.linaro.org/arm/vexpress-firmware.git
```

Then copy over the UEFI/EDK2 files that were built in the previous steps, making sure they get copied to the right firmware folder location:

```shell
cp $LINARO_EDK2_DIR/Build/ArmJuno/DEBUG_GCC49/FV/bl1.bin vexpress-firmware/SOFTWARE
cp $LINARO_EDK2_DIR/Build/ArmJuno/DEBUG_GCC49/FV/fip.bin vexpress-firmware/SOFTWARE
```

Now just copy all the files that are now available in the 'vexpress-firmware' folder into the mounted MMC card (which is provided as an external storage after calling 'usb_on'):

```shell
cp -rf vexpress-firmware/* /media/recovery
```

Be sure to issue a sync command on your host PC afterwards, which will guarantee that the copy has completed:

```shell
sync
```

Finally, power cycle the Juno. After it has finished copying the contents of the MMC card into Flash, the board will boot up and run the new firmware.

##### Upgrading UEFI/EDK2

If you already have a known working firmware available in your Juno, you simply need to update 'bl1.bin' and 'fip.bin', by mounting Juno's MMC over usb (as described in the procedure for clean flash).

Export Juno's MMC as a usb storage device on your host machine:

```shell
Cmd> usb_on
```

Then just copy over the UEFI/EDK2 files that were built in the previous steps:

```shell
cp $LINARO_EDK2_DIR/Build/ArmJuno/DEBUG_GCC49/FV/bl1.bin /media/recovery/SOFTWARE
cp $LINARO_EDK2_DIR/Build/ArmJuno/DEBUG_GCC49/FV/fip.bin /media/recovery/SOFTWARE
```

Be sure to issue a sync command on your host PC afterwards, which will guarantee that the copy has completed:

```shell
sync
```

Then just power cycle the Juno and the board should see and use the new firmware.

#### D02

Flashing D02 requires the board to have a working ethernet connection to the FTP server hosting the firmware (since the recovery UEFI image provides an update path via FTP fetch + flash). Flashing also requires entering the Embedded Boot Loader (EBL). This can be reached by typing 'exit' on the UEFI shell that will bring you to a bios-like menu. Goto 'Boot Manager' to find EBL.

##### Clean flash

First make sure the built firmware is available in your FTP server ('PV660D02.fd'):

```shell
cp PV660D02.fd /srv/tftp/
```

Now follow the steps below in order to fetch and flash the new firmware:

1. Power off the board and unplug the power supply.
2. Push the dial switch **3. CPU0_SPI_SEL** to **off** (check [http://open-estuary.com/d02-2/](http://open-estuary.com/d02-2/) for the board picture)
   - The board has two SPI flash chips, and this switch selects which one to boot from.
3. Power on the device, stop the boot from the serial console, and get into the the 'Embedded Boot Loader (EBL)' shell
4. Push the dial switch **3. CPU0_SPI_SEL** to **on**
   - **NOTE:** make sure to run the step above before running 'biosupdate' (as it modifies the flash), or else the backup BIOS will also be modified and there will be no way to unbrick the board (unless sending it back to Huawei).
5. Download and flash the firmware file from the FTP server:
'biosupdate <server ip> -u <user> -p <password> -f <UEFI image file name> master' like
'D02 > biosupdate 10.0.0.10 -u anonymous -p anonymous -f PV660D02.fd master'
6. Exit the EBL console and reboot the board

##### Upgrading firmware

There are 2 options for updating the firmware, first via network and the second via USB storage.

Network upgrade:

1. Make sure the built firmware is available in your FTP server ('PV660D02.fd')
2. Stop UEFI boot, select 'Boot Manager' then 'Embedded Boot Loader (EBL)'
3. Download and flash the firmware file from the FTP server:
'biosupdate <server ip> -u <user> -p <password> -f <UEFI image file name> master', like
'D02 > biosupdate 10.0.0.10 -u anonymous -p anonymous -f PV660D02.fd master'
4. Exit the EBL console and reboot the board

USB storage upgrade:
- Copy the '.fd' file to a FAT32 partition on USB (UEFI can only recognize FAT32 file system), then run the following command (from **EBL**):
'newbios fs1:\<file path to .fd file>'

On EBL fs1 is for USB first partition, while fs0 the ramdisk.

#### D03

Flashing D03 requires the board to have a working ethernet connection to the FTP server hosting the firmware (since the recovery UEFI image provides an update path via FTP fetch + flash). Flashing also requires entering the Embedded Boot Loader (EBL). This can be reached by typing 'exit' on the UEFI shell that will bring you to a bios-like menu. Goto 'Boot Manager' to find EBL.

##### Clean flash

First make sure the built firmware is available in your FTP server ('D03.fd'):

```shell
cp D03.fd /srv/tftp/
```

Now follow the steps below in order to fetch and flash the new firmware:

1. Power off the board and unplug the power supply.
2. Push the dial switch **3. CPU0_SPI_SEL** to **off** (check [http://open-estuary.com/d03-2/](http://open-estuary.com/d03-2/) for the board picture)
   - The board has two SPI flash chips, and this switch selects which one to boot from.
3. Power on the device, stop the boot from the serial console, and get into the the 'Embedded Boot Loader (EBL)' shell
4. Push the dial switch **3. CPU0_SPI_SEL** to **on**
   - **NOTE:** make sure to run the step above before running 'biosupdate' (as it modifies the flash), or else the backup BIOS will also be modified and there will be no way to unbrick the board (unless sending it back to Huawei).
5. Download and flash the firmware file from the FTP server:
'biosupdate <server ip> -u <user> -p <password> -f <UEFI image file name> master' like
'D02 > biosupdate 10.0.0.10 -u anonymous -p anonymous -f D03.fd master'
6. Exit the EBL console and reboot the board

##### Upgrading firmware

There are 2 options for updating the firmware, first via network and the second via USB storage.

Network upgrade:

1. Make sure the built firmware is available in your FTP server ('D03.fd')
2. Stop UEFI boot, select 'Boot Manager' then 'Embedded Boot Loader (EBL)'
3. Download and flash the firmware file from the FTP server:
'biosupdate <server ip> -u <user> -p <password> -f <UEFI image file name> master', like
'D02 > biosupdate 10.0.0.10 -u anonymous -p anonymous -f D03.fd master'
4. Exit the EBL console and reboot the board

USB storage upgrade:
- Copy the '.fd' file to a FAT32 partition on USB (UEFI can only recognize FAT32 file system), then run the following command (from **EBL**):
'newbios fs1:\<file path to .fd file>'

On EBL fs1 is for USB first partition, while fs0 the ramdisk.

#### AMD Overdrive / HuskyBoard / Cello

##### Clean flash

###### DediProg SF100

Use [DediProg SF100](http://www.dediprog.com/pd/spi-flash-solution/sf100) to flash the firmware via SPI, by plugging the programming unit into the Overdrive/Husky/Cello board 2x4 pin header (labeled SCP SPI J5 on Overdrive).

The Dediprog flashing tool is also available for Linux, please check for [https://github.com/DediProgSW/SF100Linux](https://github.com/DediProgSW/SF100Linux) for build and use instructions.

First unplug the power cord before flashing the new firmware, then erase the SPI flash memory:

```shell
dpcmd --type MX25L12835F -e
```

Now just flash the new firmware:

```shell
dpcmd --type MX25L12835F -p FIRMWARE.rom
```

Then just power cycle the board, and it should boot with the new firmware.

###### SPI Hook

Use [SPI Hook](http://www.tincantools.com/SPI_Hook.html) and _flashrom_ to flash the firmware via SPI, by plugging the programming unit into the Overdrive/Husky/Cello board 2x4 pin header (labeled SCP SPI J5 on Overdrive).

In order to use SPI Hook, make sure _flashrom_ is recent enough. This utility is used to identify, read, write, verify and erase flash chips. You can find the _flashrom_ package in most Linux distributions, but make sure the version at least v.0.9.8. If older, please just build latest from source, by going to [flashrom Downloads](https://www.flashrom.org/Downloads)

Depending on the size of the firmware image, flashrom might not be able to flash as it will complain that the size of the image is not a perfect match for the size of the SPI (partial flash only supported via the use of layouts). One easy way is just appending 0s at the end of the file, until it got the right size.

Example for the 4.5M based firmware:

```shell
dd if=/dev/zero of=FIRMWARE.ROM ibs=512K count=23 obs=1M oflag=append conv=notrunc
```

Connect the SPI cable, unplug the power cord and flash SPI:

```shell
sudo flashrom -p ft2232_spi:type=2232h,port=A,divisor=2 -c "MX25L12835F/MX25L12845E/MX25L12865E" -w FIRMWARE.rom
```

Then just power cycle the board, and it should boot with the new firmware.

##### Upgrading firmware

There is currently no easy way to update just the UEFI/EDK2 firmware, so please follow the clean flash process instead.

### Links and References:

- [ARM - Using Linaro's deliverables on Juno](https://community.arm.com/docs/DOC-10804)
- [ARM - FAQ: General troubleshooting on the Juno](https://community.arm.com/docs/DOC-8396)
