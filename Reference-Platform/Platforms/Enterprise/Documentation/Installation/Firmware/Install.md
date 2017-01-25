<table align="center">
<tr>
    <td align="center">UEFI/EDK2<br><a href="../README.md">Go Back</a></td>
    <td align="center"><a href="Download.md">Download</a><br>Get the latest pre-built firmware images</td>
    <td align="center"><a href="Build.md">Build</a><br>Instructions for building latest firmware images</td>
    <th align="center"><a href="Install.md">Install</a><br>Instructions on how to install firmware</td>
    <td align="center"><a href="README.md">Read more</a><br>Learn more about UEFI/EDK2</td>
</tr>
</table>

Choose instructions from the approved hardware:

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
truncate --size=16M FIRMWARE.rom
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
