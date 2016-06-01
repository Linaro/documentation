#### Your Build Choice

[<img src="http://i.imgur.com/jl4GG0d.png" data-canonical-src="http://i.imgur.com/jl4GG0d.png" width="125" height="157" />]()
[<img src="http://i.imgur.com/yRQKDI6.png" data-canonical-src="http://i.imgur.com/yRQKDI6.png" width="125" height="157" />]()
[<img src="http://i.imgur.com/7wy1996.png" data-canonical-src="http://i.imgur.com/7wy1996.png" width="125" height="157" />]()
[<img src="http://i.imgur.com/yRQKDI6.png" data-canonical-src="http://i.imgur.com/yRQKDI6.png" width="125" height="157" />]()
[<img src="http://i.imgur.com/tXXN5bZ.png" data-canonical-src="http://i.imgur.com/tXXN5bZ.png" width="125" height="157" />]()

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

#### Step 2: Download the following files

>Note: Some files have 4G and 8G options, download file which best matches your HiKey board.

- All HiKey **CircuitCo boards** will use the **4G files**
- All HiKey **LeMaker 1G boards** will use the **4G files**
- All HiKey **LeMaker 2G boards** will use the **8G files**

Build Folders (<a href="http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/bootloader/" target="_blank">**Binaries**</a> / <a href="http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/" target="_blank">**Image**</a>)

- **l-loader.bin** ([**Download**](http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/bootloader/l-loader.bin))
- **fip.bin** ([**Download**](http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/bootloader/fip.bin))
- **nvme.img** ([**Download**](http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/bootloader/nvme.img))
- **ptable-aosp.img** ([**4G Download**](http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/bootloader/ptable-aosp-4g.img) / [**8G Download**](http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/bootloader/ptable-aosp-8g.img))
- **hisi-idt.py** ([**Download**](http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/bootloader/hisi-idt.py))
- **boot_fat.uefi.img** ([**Download**](http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/boot_fat.uefi.img.tar.xz))
- **cache.img.tar.xz** ([**Download**](http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/cache.img.tar.xz))
- **userdata.img.xz** ([**4G Download**](http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/userdata.img.tar.xz) / [**8G Download**](http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/userdata-8gb.img.tar.xz))
- **system.img.tar.xz** (<a href="http://builds.96boards.org/releases/reference-platform/aosp/hikey/16.03/system.img.tar.xz" target="_blank">**Download**</a>)

***

#### Step 3: Install AOSP Using Fastboot with Linux host

This section show how to install the AOSP operating system to your HiKey using the fastboot method on a Linux host computer.

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
- Open Terminal application and execute the following:

```shell
$ sudo fastboot devices
0123456789abcdef fastboot
```

>Note: If your HiKey is not being detected by fastboot, you might want to try [Board Recovery](https://github.com/96boards/documentation/wiki/HiKey-Board-Recovery) and return to this step once your board is ready

3 - **Set HiKey into Recovery Mode using J15 header**

- Remove link between pins 5 and 6
- Link pins 1 and 2
- Link pins 3 and 4

Name | Link | State
---- | ---- | -----
Auto Power up | Link 1-2 | closed
Boot Select | Link 3-4 | closed
GPIO3-1 | Link 5-6 | open

4 - **Install Operating System update using downloaded files**

>**NOTE:** the ptable must be flashed first. Wait for a few seconds after the reboot command to allow the bootloader to restart using the new partition table.

```shell
$ sudo fastboot flash ptable ptable-aosp-8g.img
$ sudo fastboot reboot
$ sudo fastboot flash boot boot_fat.uefi.img
$ sudo fastboot flash cache cache.img
$ sudo fastboot flash system system.img
$ sudo fastboot flash userdata userdata-8gb.img
```

5 - **Reboot HiKey into new OS**

- Wait untill all files have been flashed onto HiKey board
- Power down HiKey by unplugging the power adapter
- Remove microUSB cable from HiKey
- Remove Link 3-4 from J15 header

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
