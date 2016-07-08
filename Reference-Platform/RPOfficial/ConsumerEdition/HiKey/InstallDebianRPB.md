# Install Instructions - Reference Software Platform

This page provides download and installation instructions inteded for those interested in flashing the HiKey board with pre-built Linaro Reference Software. Two methods are currently available: **SD card method** and **Fastboot method**. If you are already familiar with these methods, you may find all necessary files in the [96Boards RPB 16.06 build folder](http://builds.96boards.org/releases/reference-platform/debian/hikey/16.06/).

## Contents

- [SD Card Method](#sd-card-method)
- [Fastboot Method](#fastboot-method)

***

# SD Card Method

<img src="http://i.imgur.com/jl4GG0d.png" data-canonical-src="http://i.imgur.com/jl4GG0d.png" width="125" height="157" />
<img src="http://i.imgur.com/yRQKDI6.png" data-canonical-src="http://i.imgur.com/yRQKDI6.png" width="125" height="157" />
<img src="http://i.imgur.com/OQGR5yY.png" data-canonical-src="http://i.imgur.com/OQGR5yY.png" width="125" height="157" />
<img src="http://i.imgur.com/yRQKDI6.png" data-canonical-src="http://i.imgur.com/yRQKDI6.png" width="125" height="157" />
<img src="http://i.imgur.com/g8N21m1.png" data-canonical-src="http://i.imgur.com/g8N21m1.png" width="125" height="157" />

#### Step 1: Read about the SD Card Method

The SD card method allows you to place a microSD card into the DragonBoard™ 410c to automatically boot and install the Linux Desktop onto the board. This method is generally simpler and should be used by beginners. 

This method requires the following hardware:
- HiKey with power supply
- Host Linux machine (Linux, Mac OS X, or Windows)
- MicroSD card with 4GB or more of storage
- USB Mouse and/or keyboard
- HDMI Monitor with full size HDMI cable 


***
#### Step 2: Download SD Card Image

**Debian Linux Reference Software Platform - SD Card Image** 

[SD Card Image - Direct Download](http://builds.96boards.org/releases/reference-platform/debian/hikey/16.06/hikey-debian-jessie-alip-sdcard-*.img.gz)

***

#### Step 3: Prepare MicroSD card

- Ensure data from mircoSD card is backed up
- Everything on microSD card will be lost by the end of this procedure.

***

#### Step 4: Find SD Card Device name

- Use host Linux computer
- Open "Terminal" application
- Remove SD card from host computer and run the following command:
```shell
lsblk
```
- Note all recognized disk names
- **Insert SD card** and run the following command (again):
```shell
lsblk
```
- Note the newly recognized disk. This will be your SD card.
- You will need to remember this device name for a later step.

***

#### Step 5: Recall Download Location

- Locate SD card install file from Downloads page.
- This file will be needed for the next step.

***

#### Step 6: Unzip _SD Card Install Image_

- When unzipped, you will have a folder with the following contents:
   - Linaro/Debian Install Image (.img)
   - Readme

***

#### Step 7: Go to directory with _SD Card Install Image_ folder using Terminal

- Use host Linux computer
- Open "Terminal" application
- `cd` to the directory with your unzipped **SD Card Install Image**

```shell
cd <extraction directory>

#Example: 
#<extraction directory> = /home/YourUserName/Downloads
#For this example we assume the "Debian SD Card Install Image" is in the Downloads folder.
cd /home/YourUserName/Downloads
```

***

#### Step 8: Install Image onto SD Card

**Checklist:**

- SD card inserted into host Linux computer
- Recall SD Card device name **Step 4**
- From within the extraction folder, using the Terminal execute the following commands:

**Execute:**

```shell
sudo dd if=hikey-jessie_alip_2015MMDD-nnn.img of=/dev/XXX bs=4M oflag=sync status=noxfer
```

**Note:**

- `if=hikey-jessie_alip_2015MMDD-nnn.img`: should match the name of the image that was downloaded.
- `of=/dev/XXX`: XXX should match the name of the SD Card device name from [**Step 2**](). Be sure to use the device name with out the partition.
- This command will take some time to execute. Be patient and avoid tampering with the terminal until process has ended.
- Once SD card is done flashing, remove from host computer and set aside for a later step


***

#### Step 9: Prepare HiKey with SD card

- Make sure HiKey is unplugged from power
- Connect an HDMI monitor to the HiKey with an HDMI cable, and power on the monitor
- Plug a USB keyboard and/or mouse into either of the two USB connectors on the HiKey
- Insert the microSD card into the HiKey
- Plug power adaptor into HiKey, wait for board to boot up.

***

#### Step 10: Install Linaro/Debian onto HiKey

<img src="http://i.imgur.com/F18wlgU.png" data-canonical-src="http://i.imgur.com/F18wlgU.png" width="400" height="250"/>

- If **Steps 1 - 8** were followed correctly, the above screen should be visible from your HiKey
- Select the image to install and click “Install” (or type “i”). OS will be installed into the eMMC memory
- This process can take a few minutes to complete
- Upon completion, “Flashing has completed and OS has installed successfully....” message will appear.

Before clicking "OK":

- Remove the SD Card
- Now click "OK" button and allow HiKey to reboot.

**Congratulations! You are now booting your newly installed operating system directly from eMMC on the HiKey**

[Back to top](#install-instructions---reference-software-platform)

***

# Fastboot Method

<img src="http://i.imgur.com/jl4GG0d.png" data-canonical-src="http://i.imgur.com/jl4GG0d.png" width="125" height="157" />
<img src="http://i.imgur.com/yRQKDI6.png" data-canonical-src="http://i.imgur.com/yRQKDI6.png" width="125" height="157" />
<img src="http://i.imgur.com/OQGR5yY.png" data-canonical-src="http://i.imgur.com/OQGR5yY.png" width="125" height="157" />
<img src="http://i.imgur.com/yRQKDI6.png" data-canonical-src="http://i.imgur.com/yRQKDI6.png" width="125" height="157" />
<img src="http://i.imgur.com/tXXN5bZ.png" data-canonical-src="http://i.imgur.com/tXXN5bZ.png" width="125" height="157" />

***

#### Step 1: Read about the Fastboot Method

Fastboot is supported by the board and can be used for installs.  This is for advanced users who are most likely modifying/customizing source code and will need to download such updates to the board for test/execution. 

This method requires the following hardware:
- HiKey with power supply
- Host Linux machine (Linux, Mac OS X, or Windows)
- USB to microUSB cable
- USB Mouse and/or keyboard (not required to perform flash)
- HDMI Monitor with full size HDMI cable (not required to perform flash)

***

#### Step 2: Download Debian partition table

> Note: Some files have 4G and 8G options, download file which best matches your HiKey board.

- All HiKey **CircuitCo boards** will use the **4G files**
- All HiKey **LeMaker 1G boards** will use the **4G files**
- All HiKey **LeMaker 2G boards** will use the **8G files**

**ptable-linux.img** ([**4G Download**](http://builds.96boards.org/releases/reference-platform/debian/hikey/16.06/bootloader/ptable-linux-4g.img) / [**8G Download**](http://builds.96boards.org/releases/reference-platform/debian/hikey/16.06/bootloader/ptable-linux-8g.img))

***

#### Step 3: Download Boot image and Root File System

- **Debian Boot** ([**Download**](http://builds.96boards.org/releases/reference-platform/debian/hikey/16.06/hikey-boot-linux-*.uefi.img.gz))
- **Debian Rootfs** (<a href="http://builds.96boards.org/releases/reference-platform/debian/hikey/16.06/hikey-rootfs-debian-jessie-alip-*.emmc.img.gz" target="_blank">**Download**</a>)

***

#### Step 4: Install Debian Using Fastboot with Linux host

This section show how to install the Linaro based Debian operating system to your HiKey using the fastboot method on a Linux host computer.



1 - **Make sure fastboot is set up on host computer**

- Android SDK “Tools only” for Linux can be downloaded <a href="http://developer.android.com/sdk" target="_blank">here</a>
- The Linux “Tools Only” SDK download does not come with fastboot, you will need to use the Android SDK Manager to install platform-tools.
- To do this follow the “SDK Readme.txt” instructions included in your SDK “Tools Only” download.

If you are still having trouble setting up fastboot, <a href="https://youtu.be/W_zlydVBftA" target="_blank">click here</a> for a short tutorial video

2 - **Boot HiKey into Fastboot mode using J15 header**

- Link pins 1 and 2
- Link pins 5 and 6
- Connect host computer to HiKey board using USB to microUSB cable

Name | Link | State
---- | ---- | -----
Auto Power up | Link 1-2 | closed
Boot Select | Link 3-4 | open
GPIO3-1 | Link 5-6 | closed

- Power on HiKey board by plugging in power adapter
- Esure HiKey is detected by host computere
- Wait for about 10 seconds
- Open Terminal application and execute the following:

```shell
$ sudo fastboot devices
0123456789abcdef fastboot
```

>Note: If your HiKey is not being detected by fastboot, you might want to try [Board Recovery](https://github.com/96boards/documentation/wiki/HiKey-Board-Recovery) and return to this step once your board is ready

3 - **Install Operating System update using downloaded files**

>**NOTE:** the ptable must be flashed first. Wait for a few seconds after the reboot command to allow the bootloader to restart using the new partition table.

```shell
$ sudo fastboot flash ptable <ptable_FILE_NAME>.img
$ sudo fastboot reboot
$ sudo fastboot flash boot <boot_FILE_NAME>.uefi.img
$ sudo fastboot flash system hikey-jessie_alip_YYYYMMDD-nnn-Xg.emmc.img
```

4 - **Reboot HiKey into new OS**

- Wait untill all files have been flashed onto HiKey board
- Power down HiKey by unplugging the power adapter
- Remove microUSB cable from HiKey
- Remove Link 5-6 from J15 header

Name | Link | State
---- | ---- | -----
Auto Power up | Link 1-2 | closed
Boot Select | Link 3-4 | open
GPIO3-1 | Link 5-6 | open

- Plug mouse/keyboard USB into type A USB ports
- Power up HiKey by plugging in power adapter


**Note:** the **username** and **password** are both **“linaro”** when the login information is requested.

**Congratulations! You are now booting your newly installed OS directly
from eMMC on the HiKey!**

[Back to top](#install-instructions---reference-software-platform)
