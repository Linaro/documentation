# Reference Platform Build - 16.06

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
- Host machine (Linux, Mac OS X, or Windows)
- USB to microUSB cable
- USB Mouse and/or keyboard (not required to perform flash)
- HDMI Monitor with full size HDMI cable (not required to perform flash)

***

#### Step 2: Download Debian partition table

> Note: Some files have 4G and 8G options, download file which best matches your HiKey board.

- All HiKey **CircuitCo boards** will use the **4G files**
- All HiKey **LeMaker 1G boards** will use the **4G files**
- All HiKey **LeMaker 2G boards** will use the **8G files**

**ptable-linux.img** ([**4G Download**](https://builds.96boards.org/snapshots/reference-platform/components/uefi/latest/release/hikey/ptable-linux-4g.img) / [**8G Download**](https://builds.96boards.org/snapshots/reference-platform/components/uefi/latest/release/hikey/ptable-linux-8g.img))

***

#### Step 3: Download Boot image and Root File System

- **Debian Boot** ([**Download**](https://builds.96boards.org/snapshots/reference-platform/debian/117/hikey/hikey-boot-linux-*.uefi.img.gz))
- **Debian Rootfs** (<a href="https://builds.96boards.org/snapshots/reference-platform/debian/117/hikey/hikey-rootfs-debian-jessie-alip-*.emmc.img.gz" target="_blank">**Download**</a>)

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
