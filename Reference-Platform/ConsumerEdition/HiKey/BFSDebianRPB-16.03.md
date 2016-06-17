## Debian RPB 16.06 - Build from Source

- Building Linux Kernel from Source
   - Step 1: Setting up your environment on your host computer 
   - Step 2: Download the Linaro cross compiler toolchain
   - Step 3: Export path to cross compiler tool and confirm version
   - Step 5: Set the right kernel .config file
   - Step 6: Build kernel image and debian package
   - Step 7: Copy Modules
   - Step 8: Find kernel release string
   - Step 9: Generate modules.dep and map files
   - Step 10: Find DragonBoard™ 410c IP Address
   - Step 11: Transfer the modules to the target HiKey
   - Step 12: Generate the initramfs
   - Step 13: Create the device tree image and boot image
- Customize Bootloader
- Build Rootfs from source

***

#### Building the Linux kernel from source

The Linux kernel used in this release is available via tags in the git [repository](https://github.com/96boards/linux)

```shell
git: https://github.com/96boards/linux
Dynamic tag: 96b-kernelci
Fixed tag: 96b/releases/2016.06
defconfig: arch/arm64/configs/distro.config
```

The kernel image (`Image`) and the kernel modules are installed in the root file system (e.g. `/boot/vmlinuz-4.4.0-104-arm64` and `/lib/modules/4.4.0-104-arm64`). It is possible for a user to rebuild the kernel and run a custom kernel image instead of the released kernel. You can build the kernel using any recent GCC release using the git tree, tag and defconfig mentioned above. This release only supports booting with device tree, as such both the device tree blobs need to be built as well.

The HiKey is an ARMv8 platform, and the kernel is compiled for the Aarch64 target. Even though it is possible to build natively, on the target board, It is recommended to build the Linux kernel on a PC development host. In which case you need to install a cross compiler for the ARM architecture. It is recommended to download the Linaro GCC cross compiler [Aarch64 little-endian](http://releases.linaro.org/components/toolchain/binaries/5.3-2016.02/aarch64-linux-gnu/gcc-linaro-5.3-2016.02-x86_64_aarch64-linux-gnu.tar.xz), also available [here](http://releases.linaro.org/components/toolchain/binaries/5.3-2016.02/)

To build the Linux kernel, you can use the following instructions:

#### Step 1: Setting up your environment on your host computer

- Open your Terminal and cd into your desired directory
- Make a new folder using `mkdir`, name it something relevant

```shell
#Example of desired directory
$ cd ~/Desktop

#Example of relevant folder
$ mkdir HiKey-16.06
$ cd HiKey-16.06
```

#### Step 2: Step 2: Download the Linaro cross compiler toolchain

- From within the directory you just made
- Download and unzip by executing the following commands

###### Linaro Cross Compiler

```shell
#Download
$ wget http://releases.linaro.org/components/toolchain/binaries/5.3-2016.02/aarch64-linux-gnu/gcc-linaro-5.3-2016.02-x86_64_aarch64-linux-gnu.tar.xz
#Unzip
$ tar -Jxvf gcc-linaro-5.3-2016.02-x86_64_aarch64-linux-gnu.tar.xz
```

#### Step 3: Export path to cross compiler tool and confirm version

- Exporting path will allow build system can find and use the right kernel

```shell
#Create path
$ export PATH=~/Desktop/HiKey-16.06/gcc-linaro-5.3-2016.02-x86_64_aarch64-linux-gnu/bin/:$PATH
#Check version
$ aarch64-linux-gnu-gcc --version
aarch64-linux-gnu-gcc (Linaro GCC 5.3-2016.02) 5.3.1 20160113
Copyright (C) 2015 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

#### Step 4: Clone the Reference Platform kernel

- **96b-kernelci** is the development branch
- This branch will have the latest changes
- Use **96b/releases/2016.06** if you want the same version used by the 16.06 release

```shell
$ git clone -b 96b/releases/2016.06 http://github.com/96boards/linux.git
```

- Cloning the kernel may take a few minutes
- If you already have a local clone of another kernel git tree, use _--reference path/your/old/tree/.git_ for a faster clone process
- Once kernel source has been cloned cd into its directory

```shell
$ cd linux
```

#### Step 5: Set the right kernel .config file

- This step creates the '.config' file
- The .config file is used by the build system when compiling the kernel
- Current Reference Platform config can be made by using distro.config
- From with in kernel directory execute the following command:

```shell
$ cp arch/arm64/configs/distro.config .config
$ make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- oldconfig
```

- New .config file will be hidden but can be seen by executing `ls -a` from within kernel folder
- To view all current configuration the .config file can be opened with a text editor such a `vim`

#### Step 6: Build kernel image and debian package

- This step will take some time (~20-30 minutes or more), depending on your cpu/memory
- Creating the kernel package is recommended for HiKey, as it supports Grub 2

```shell
#Replace X from -jX with the number of cores on your host computer
$ make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- -jX deb-pkg LOCALVERSION=-yourowntag
```

#### Step 7: Find HiKey IP Address

- On your HiKey board
- Connect to the internet through WIFI
- Open one of the Terminal applications

```shell
$ /sbin/ifconfig
```
- Look for your `wlan0` connection
- Here you will see an `inet addr`
- This is your board's IP address and should look something like this: `192.168.0.10`

#### Step 8: Transfer the modules to the target HiKey

- Using your board's IP Address for linaro@<yourIPaddress>

```shell
$ scp ../linux-image-4.4.0-yourowntag.deb linaro@192.168.1.15:~/
$ ssh linaro@192.168.1.15
#HiKey shell

$ hikey $ sudo dpkg -i linux-image-4.4.0-yourowntag.deb
```
Congratulations! Your new kernel is now ready to be used by your HiKey.

- You can check `/boot/grub/grub.cfg` for the new boot entry based on your own kernel
- If you want only your kernel to be available, you can remove the default linux-image package, and grub will be automatically updated

### Boot Loader

Please see go [here](BuildSourceBL.md) for instructions on how to built the boot loader from source.

#### How to get and customize Debian packages source code

This release is based on Debian 8.2 "Jessie".

Since all packages installed in Linaro Debian-based images are maintained either in Debian archives or in Linaro repositories, it is possible for users to update their environment with commands such as:

```shell
sudo apt-get update
sudo apt-get upgrade
```

All user space software is packaged using Debian packaging process. As such you can find extensive information about using, patching and building packages in The Debian New Maintainers Guide. If you quickly want to rebuild any package, you can run the following commands to fetch the package source code and install all build dependencies:

```shell
sudo apt-get update
sudo apt-get build-dep <pkg>
apt-get source <pkg>
```

Then you can rebuild the package locally with:

```shell
cd <pkg-version>
dpkg-buildpackage -b -us -uc
```

#### TODO

* Explain how to build the rootfs from source
