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
#### Step 4: Choose your host computer to access your instruction set

- [Linux](InstallAOSPRPB-16.03.md)
