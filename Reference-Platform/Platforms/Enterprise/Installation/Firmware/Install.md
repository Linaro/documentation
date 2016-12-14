# Install

## Juno R0/R1

### Clean flash

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

### Upgrade Firmware

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

***

## D02

Flashing D02 requires the board to have a working ethernet connection to the FTP server hosting the firmware (since the recovery UEFI image provides an update path via FTP fetch + flash). Flashing also requires entering the Embedded Boot Loader (EBL). This can be reached by typing 'exit' on the UEFI shell that will bring you to a bios-like menu. Goto 'Boot Manager' to find EBL.

### Clean flash

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

### Upgrade Firmware

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

***

## D03

Flashing D03 requires the board to have a working ethernet connection to the FTP server hosting the firmware (since the recovery UEFI image provides an update path via FTP fetch + flash). Flashing also requires entering the Embedded Boot Loader (EBL). This can be reached by typing 'exit' on the UEFI shell that will bring you to a bios-like menu. Goto 'Boot Manager' to find EBL.

### Clean flash

To do a clean flash you will require access to the board's BMC.

1. Make sure the board's BMC port is connected, and with a known IP address.
2. Login the BMC website, The username/passwd is root/Huawei12#$. Go to "System", "Firmware Upgrade", and "Browse" to select the UEFI file in hpm format. (Please contact support@open-estuary.org to get the hpm file).
3. Pull out the power cable to power off the board. Find the pin named "COM_SW" at J44. Then connect it with jump cap.
4. Power on the board and connect to the board's serial port. When the screen display message "You are trying to access a restricted zone. Only Authorized Users allowed.", type "Enter", input username/passwd (username/passwd is root/Huawei12#$).
5. After you login the BMC interface which start with "iBMC:/->", use command "ifconfig" to see the modified BMC IP. When you get the board's BMC IP, please visit the BMC website by "https://BMC IP ADDRESS/".
6. Go to "Start Update" (Do not power off during this period).
7. After updating the UEFI firmware, reboot the board to enter UEFI menu.

### Upgrade Firmware

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

***

## AMD Overdrive / HuskyBoard / Cello

### Clean flash

#### DediProg SF100

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

#### SPI Hook

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

### Upgrade Firmware

There is currently no easy way to update just the UEFI/EDK2 firmware, so please follow the clean flash process instead.

# Links and References:

- [ARM - Using Linaro's deliverables on Juno](https://community.arm.com/docs/DOC-10804)
- [ARM - FAQ: General troubleshooting on the Juno](https://community.arm.com/docs/DOC-8396)
