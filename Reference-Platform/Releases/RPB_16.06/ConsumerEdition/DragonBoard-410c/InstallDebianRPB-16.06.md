# Install Instructions - Reference Software Platform

This page provides download and installation instructions inteded for those interested in flashing the DragonBoard 410c board with pre-built Linaro Reference Software. Two methods are currently available: **SD card method** and **Fastboot method**. If you are already familiar with these methods, you may find all necessary files in the [96Boards RPB 16.06 build folder](https://builds.96boards.org/releases/reference-platform/debian/dragonboard410c/16.06/).

## Contents

- [SD Card Method](#sd-card-method)
- [Fastboot Method](#fastboot-method)

***

# SD Card Method

#### Step 1: Read about the SD Card Method

The SD card method allows you to place a microSD card into the DragonBoard™ 410c to automatically boot and install the Linux Desktop onto the board. This method is generally simpler and should be used by beginners. 

This method requires the following hardware:
- DragonBoard™ 410c with power supply
- Host machine (Linux, Mac OS X, or Windows)
- MicroSD card with 4GB or more of storage
- USB Mouse and/or keyboard
- HDMI Monitor with full size HDMI cable 


***

#### Step 2: Download SD Card Image

[SD Card Image - Direct Download](https://builds.96boards.org/releases/reference-platform/debian/dragonboard410c/16.06/dragonboard410c-debian-jessie-alip-sdcard-*.img.gz)

>Note the location of all downloads, they will be needed once you access your instruction set

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
$ lsblk
```
- Note all recognized disk names
- **Insert SD card** and run the following command (again):
```shell
$ lsblk
```
- Note the newly recognized disk. This will be your SD card.
- **Remember** your SD card device name for a later step

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
- `cd` to the directory with your unzipped **Debian SD Card Install Image**

```shell
$ cd <extraction directory>

#Example: 
#<extraction directory> = /home/YourUserName/Downloads
#For this example we assume the "Debian SD Card Install Image" is in the Downloads folder.
$ cd /home/YourUserName/Downloads
```


***

#### Step 8: Locate SD Card Install Image

- Make sure you are in the extraction directory

**Unzipped Debian SD Card download will be a folder. This folder should be listed in your directory. Type `ls` from command line for a list of files that can be found in your current directory**:

```shell
$ ls

#output
dragonboard410c_sdcard_install_debian-XX
```

- Unzipped folder should be called dragonboard410c_sdcard_install_debian-XX, where XX represents the Debian release number
- `cd` into this directory

```shell
$ cd dragonboard410c_sdcard_install_debian-XX
```

- Inside this folder you will find the install image
   - `db410c_sd_install_debian.img`
- This `.img` file is what will be flashed to your SD Card.


***

#### Step 9: Install Image onto SD Card

**Checklist:**

- SD card inserted into host Linux computer
- Recall SD Card device name from [**Step 2**](https://github.com/sdrobertw/test-wiki-/wiki/Linux-host-SD-CARD#step-2-find-sd-card-device-name)
- From within the dragonboard410c_sdcard_install_debian-XX folder, using the Terminal execute the following commands:

**Execute:**

```shell
$ sudo dd if=db410c_sd_install_debian.img of=/dev/XXX bs=4M oflag=sync status=noxfer
```

**Note:**

- `if=db410c_sd_install_debian.img`: should match the name of the image that was downloaded.
- `of=/dev/XXX`: XXX should match the name of the SD Card device name from [**Step 2**](https://github.com/sdrobertw/test-wiki-/wiki/Linux-host-SD-CARD#step-2-find-sd-card-device-name). Be sure to use the device name with out the partition.
- This command will take some time to execute. Be patient and avoid tampering with the terminal until process has ended.
- Once SD card is done flashing, remove from host computer and set aside for a later step

***

#### Step 10: Prepare DragonBoard™ 410c with SD card

- Make sure DragonBoard™ 410c is unplugged from power
- Set S6 switch on DragonBoard™ 410c to `0-1-0-0`, "SD Boot switch" should be set to "ON".
   - Please see "1.1 Board Overview" on page 7 from [DragonBoard™ 410c Hardware Manual](https://linaro.co/96b-hwm-db) if you cannot find S6
- Connect an HDMI monitor to the DragonBoard™ 410c with an HDMI cable, and power on the monitor
- Plug a USB keyboard and/or mouse into either of the two USB connectors on the DragonBoard™ 410c
- Insert the microSD card into the DragonBoard™ 410c
- Plug power adaptor into DragonBoard™ 410c, wait for board to boot up.

***

#### Step 11: Install RPB Linaro/Debian onto DragonBoard™ 410c

<img src="https://i.imgur.com/F18wlgU.png" data-canonical-src="https://i.imgur.com/F18wlgU.png" width="400" height="250"/>

- If **Steps 1 - 8** were followed correctly, the above screen should be visible from your DragonBoard™ 410c
- Select the image to install and click “Install” (or type “i”). OS will be installed into the eMMC memory
- This process can take a few minutes to complete
- Upon completion, “Flashing has completed and OS has installed successfully....” message will appear.

Before clicking "OK":

- Remove the SD Card
- Set S6 switch on DragonBoard™ 410c to `0-0-0-0`, all switches should be set to "OFF"
- Now click "OK" button and allow DragonBoard™ 410c to reboot.

**Congratulations! You are now booting your newly installed operating system directly from eMMC on the DragonBoard™ 410c!**

[Back to top](#install-instructions---reference-software-platform)

***

# Fastboot Method


#### Step 1: Read about the Fastboot Method

Fastboot is supported by the board and can be used for installs.  This is for advanced users who are most likely modifying/customizing source code and will need to download such updates to the board for test/execution. 

This method requires the following hardware:
- DragonBoard™ 410c with power supply
- Host Linux machine
- USB to microUSB cable
- USB Mouse and/or keyboard (not required to perform flash)
- HDMI Monitor with full size HDMI cable (not required to perform flash)

***

#### Step 2: Download Debian Bootloader

- Debian Bootloader ([Direct Download](https://builds.96boards.org/releases/dragonboard410c/linaro/rescue/latest/dragonboard410c_bootloader_emmc_linux-*.zip) / <a href="https://builds.96boards.org/releases/dragonboard410c/linaro/rescue/latest/" target="_blank">Build Folder</a> )

#### Step 3: Download Boot image and Root file system

- Debian Boot ([Direct Download](https://builds.96boards.org/releases/reference-platform/debian/dragonboard410c/16.06/dragonboard410c-boot-linux-*.img.gz) / <a href="https://builds.96boards.org/releases/reference-platform/debian/dragonboard410c/16.06/" target="_blank">Build Folder</a> )
- Debian Rootfs (Desktop) ([Direct Download](https://builds.96boards.org/releases/reference-platform/debian/dragonboard410c/16.06/dragonboard410c-rootfs-debian-jessie-alip-*.emmc.img.gz) / <a href="https://builds.96boards.org/releases/reference-platform/debian/dragonboard410c/16.06/" target="_blank">Build Folder</a> )

>Note the location of all downloads, they will be needed once you access your instruction set

***
#### Step 4:  Install Debian Using Fastboot with Linux host

This section show how to install the Linaro based Debian operating system to your DragonBoard™ 410c using the fastboot method on a Linux host computer.

1 - **Make sure fastboot is set up on host computer**

- Android SDK “Tools only” for Linux can be downloaded <a href="https://developer.android.com/sdk" target="_blank">here</a>
- The Linux “Tools Only” SDK download does not come with fastboot, you will need to use the Android SDK Manager to install platform-tools.
- To do this follow the “SDK Readme.txt” instructions included in your SDK “Tools Only” download.

If you are still having trouble setting up fastboot, <a href="https://youtu.be/W_zlydVBftA" target="_blank">click here</a> for a short tutorial video

2 - **Connect host computer to DragonBoard™ 410c**

- DragonBoard™ 410c must be powered off (unplugged from power)
- Make sure microSD card slot on DragonBoard™ 410c is empty
- S6 switch on DragonBoard™ 410c must be set to ‘0-0-0-0’. All switches should be in “off” position
- Connect USB to microUSB cable from host computer to DragonBoard™ 410c

3 - **Boot DragonBoard™ 410c into fastboot mode**

**Please read all bullet points before attempting**

- Press and hold the Vol (-) button on the DragonBoard™ 410c, this is the S4 button. DragonBoard™ 410c should still NOT be powered on
- While holding the Vol (-) button, power on the DragonBoard™ 410c by plugging it in
- Once DragonBoard™ 410c is plugged into power, release your hold on the Vol (-) button.
- Wait for about 20 seconds.
- Board should boot into fastboot mode.

From the connected host machine terminal window, run the following commands:

```shell
# Check to make sure device is connected and in fastboot mode

$ fastboot devices
```

Typically it will show as bellow
```shell
de82318	fastboot
```

**At this point you should be connected to your DragonBoard™ 410c with a USB to microUSB cable. Your DragonBoard™ 410c should be booted into fastboot mode and ready to be flashed with the appropriate images.**

4 - **Flash Bootloader**

- Use host computer
- Open "Terminal" application
- Recall location of Bootloader download.
- The bootloader file should be named `dragonboard410c_bootloader_emmc_linux-XX`
- XX represents the release number of the Bootloader
- `cd` to the directory with your unzipped **Bootloader Folder**

```shell
$ cd <extraction directory>

#Example: 
cd /Users/YourUserName/Downloads
#<extraction directory> = /Users/YourUserName/Downloads
#For this example we assume the "Bootloader" is in the Downloads folder.


$ cd <unzipped Bootloader folder>

#Example:
cd dragonboard410c_bootloader_emmc_linux-40
#<unzipped Bootloader folder> = dragonboard410c_bootloader_emmc_linux-40
#This example took place during release 40

# This command will execute the flashall script within the bootloader folder
$ ./flashall

```

5 - **Recall location of `boot` and `rootfs` download from the downloads page**

- You should have downloaded the `boot` file
- You should have downloaded ONE of rootfs` file (Either `Developer` or `Desktop - ALIP` version)

6 - **Unzip both 'boot' and 'rootfs' files**

7 - **Flash `boot` image and `rootfs` to the DragonBoard™ 410c**

- Use host computer
- Use "Terminal" application
- Recall location of extracted(unzipped) `boot` file
- Recall location of extracted(unzipped) `rootfs` file (`Developer` or `Desktop - ALIP`)
- `cd` to the directory with your unzipped `boot` and `rootfs` files
- From within extraction directory, execute the following commands:

```shell
# (Once again) Check to make sure fastboot device connected
$ sudo fastboot devices
# It will show similar to bellow if the device is connected successfully
de82318	fastboot

# cd to the directory the boot image and  were extracted
$ cd <extraction directory>

# Make sure you have properly unzipped the boot and rootfs downloads
$ sudo fastboot flash boot boot-linaro-jessie-qcom-snapdragon-arm64-**BUILD#**.img
$ sudo fastboot flash rootfs linaro-jessie-developer-qcom-snapdragon-arm64-**BUILD#**.img
```
**Note**: Replace **BUILD#** in the above commands with the file-specific date/build stamp.

8 - **Reboot DragonBoard™ 410c**

- Unplug power to DragonBoard™ 410c
- Unplug micro USB cable from DragonBoard™ 410c
- Ensure HDMI connection to monitor
- Ensure keyboard and/or mouse connection (Depending on your rootfs selection)
- Plug power back into DragonBoard™ 410c
- Wait for board to boot up
- Board will boot into either command line or desktop depending on rootfs

**Note:** the **username** and **password** are both **“linaro”** when the login information is requested.

**Congratulations! You are now booting your newly installed OS directly
from eMMC on the DragonBoard™ 410c!**

[Back to top](#install-instructions---reference-software-platform)
