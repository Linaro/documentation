[<img src="http://i.imgur.com/jl4GG0d.png" data-canonical-src="http://i.imgur.com/jl4GG0d.png" width="125" height="157" />]()
[<img src="http://i.imgur.com/yRQKDI6.png" data-canonical-src="http://i.imgur.com/yRQKDI6.png" width="125" height="157" />]()
[<img src="http://i.imgur.com/7wy1996.png" data-canonical-src="http://i.imgur.com/7wy1996.png" width="125" height="157" />]()
[<img src="http://i.imgur.com/yRQKDI6.png" data-canonical-src="http://i.imgur.com/yRQKDI6.png" width="125" height="157" />]()
[<img src="http://i.imgur.com/tXXN5bZ.png" data-canonical-src="http://i.imgur.com/tXXN5bZ.png" width="125" height="157" />]()

>**Note:** CE AOSP RPB - 16.03 is a Developer Preview operating system

***

#### Step 1: Read about the Fastboot Method

Fastboot is supported by the board and can be used for installs.  This is for advanced users who are most likely modifying/customizing source code and will need to download such updates to the board for test/execution. 

This method requires the following hardware:
- DragonBoard™ 410c with power supply
- Host machine (Linux, Mac OS X, or Windows)
- USB to microUSB cable
- USB Mouse and/or keyboard (not required to perform flash)
- HDMI Monitor with full size HDMI cable (not required to perform flash)

***

#### Step 2: Download Android Bootloader and Boot file

- Android Bootloader ([Direct Download](https://builds.96boards.org/releases/dragonboard410c/linaro/rescue/latest/dragonboard410c_bootloader_emmc_android-*.zip) / <a href="https://builds.96boards.org/releases/dragonboard410c/linaro/rescue/latest/" target="_blank">Build Folder</a> )
- Android Boot ([Direct Download](https://builds.96boards.org/releases/reference-platform/aosp/dragonboard410c/16.03/boot-db410c.img.xz) / <a href="https://builds.96boards.org/releases/reference-platform/aosp/dragonboard410c/16.03/" target="_blank">Build Folder</a> )

>Note the location of all downloads, they will be needed once you access your instruction set

#### Step 3: Download all remaining files

- system.img ([Direct Download](https://builds.96boards.org/releases/reference-platform/aosp/dragonboard410c/16.03/system.img.xz) / <a href="https://builds.96boards.org/releases/reference-platform/aosp/dragonboard410c/16.03/" target="_blank">Build Folder</a> )
- userdata.img ([Direct Download](https://builds.96boards.org/releases/reference-platform/aosp/dragonboard410c/16.03/userdata.img.xz) / <a href="https://builds.96boards.org/releases/reference-platform/aosp/dragonboard410c/16.03/" target="_blank">Build Folder</a> )
- cache.img ([Direct Download](https://builds.96boards.org/releases/reference-platform/aosp/dragonboard410c/16.03/cache.img.xz) / <a href="https://builds.96boards.org/releases/reference-platform/aosp/dragonboard410c/16.03/" target="_blank">Build Folder</a> )

>Note the location of all downloads, they will be needed once you access your instruction set

***
#### Step 4: Install Android using Fastboot with Linux host

This section show how to install the Android operating system to your DragonBoard™ 410c using the fastboot method on a Mac OS X host computer.

1 - **Make sure fastboot is set up on host computer**

- Android SDK “Tools only” for Linux can be downloaded <a href="http://developer.android.com/sdk" target="_blank">here</a>
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
- Board should boot into fastboot mode.

From the connected host machine terminal window, run the following commands:

```shell
# Check to make sure device is connected and in fastboot mode

fastboot devices
```

**At this point you should be connected to your DragonBoard™ 410c with a USB to microUSB cable. Your DragonBoard™ 410c should be booted into fastboot mode and ready to be flashed with the appropriate images.**

4 - **Flash Bootloader**

- Use host computer
- Open "Terminal" application
- Recall location of Bootloader download.
- The bootloader file should be named `dragonboard410c_bootloader_emmc_android`
- `cd` to the directory with your unzipped **Bootloader Folder**

```shell
cd <extraction directory>

#Example: 
cd /Users/YourUserName/Downloads
#<extraction directory> = /Users/YourUserName/Downloads
#For this example we assume the "Bootloader" is in the Downloads folder.


cd <unzipped Bootloader folder>

#Example:
cd dragonboard410c_bootloader_emmc_android
#<unzipped Bootloader folder> = dragonboard410c_bootloader_emmc_android

# This command will execute the flashall script within the bootloader folder
./flashall

```

5 - **Recall location of all downloaded files from downloads page**

This will include the files listed below:

###### Reference Platform files

- boot.img.tar.xz 
- system.img.tar.xz 
- userdata.img.tar.xz 
- cache.img.tar.xz 

6 - **Unzip all files**

7 - **Flash all files to the DragonBoard™ 410c**

- Use host computer
- Use "Terminal" application
- Recall location of all extracted(unzipped) files
- `cd` to the directory with your unzipped files
- From within extraction directory, execute the following commands:

###### Reference Platform

```shell
# (Once again) Check to make sure fastboot device connected
sudo fastboot devices

# cd to the directory the boot image and  were extracted
$ cd <extraction directory>

# Make sure you have properly unzipped the downloads
sudo fastboot flash boot boot.img
sudo fastboot flash system system.img
sudo fastboot flash userdata userdata.img
sudo fastboot flash cache cache.img
```

8 - **Reboot DragonBoard™ 410c**

- Unplug power to DragonBoard™ 410c
- Unplug micro USB cable from DragonBoard™ 410c
- Ensure HDMI connection to monitor
- Ensure keyboard and/or mouse connection (Depending on your rootfs selection)
- Plug power back into DragonBoard™ 410c
- Wait for board to boot up
- Board will boot into Android lock screen.

**Congratulations! You are now booting your newly installed OS directly
from eMMC on the DragonBoard™ 410c!**

