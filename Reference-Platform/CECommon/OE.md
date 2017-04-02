## OpenEmbedded and Yocto

This page provides instructions to get started with OpenEmbedded and the Yocto Project. It tries (when possible) to be generic for any board supported.
The board diversity should be addressed through dedicated BSP layer then MACHINE choice.

# Introduction

This wiki is not an introduction on OpenEmbedded or Yocto Project. If you are not familiar with OpenEmbedded and the Yocto Project, it is very much recommended to read the appropriate documentation first. For example, you can start with:
* http://openembedded.org/wiki/Main_Page
* http://yoctoproject.org/
* https://www.yoctoproject.org/documentation

In this wiki, we assume that the reader is familiar with basic concepts of OpenEmbedded.

The support for a dedicated board is available in the dedicated BSP Layer. These layers have been tested with OpenEmbedded Core layer, and are expected to work with any other standard layers and of course any OpenEmbedded based distributions.

The Linux kernel used for these boards is the Reference Platform Kernel (RPK). The graphic stack is based on mesa:
* using the freedreno driver for Dragonboard 410c
* using the ARM Mali Utgard GPU driver for HiKey
* using the ARM Mali 400 GPU driver for B2260

## OE Layers

|   Layer                 |   Description                    |
|:-----------------------:|:----------------------|
| oe-core (Base layer)    | This is the main collaboration point when working on OpenEmbedded projects and is part of the core recipes. The goal of this layer is to have just enough recipes to build a basic system, this means keeping it as small as possible. |
| meta-rpb (Distro layer) |   This is a very small layer where the distro configurations live. Currently it houses both Reference Platform Build and Wayland Reference Platform Builds. |
| meta-oe                 | This layer houses many useful, but sometimes unmaintained recipes. Since the reduction in recipes to the core, meta-oe was created for everything else. There are currently approximately 650 recipes in this layer. |
| meta-browser            | This layer holds the recipes for Firefox and Chromium. Both recipes require a lot of maintenance, because of this a seperate layer was created. |
| meta-qt5                | This is a cross-platform toolkit. |
| meta-linaro             | This layer is used to get the Linaro toolchain. |
| meta-linaro-backports   | This is an experimental layer used to get newer versions into the build which were not part of the release. |
| [meta-96boards BSP layer](https://github.com/96boards/meta-96boards) | This support layer is managed by Linaro and intended for boards that do not have their own board support layer. Currently used for the HiKey Consumer Edition board, and eventually the Bubblegum-96 board. If a vendor does not support their own layer, it can be added to this layer. |
| [meta-qcom BSP layer](http://git.yoctoproject.org/cgit/cgit.cgi/meta-qcom) | This is the board support layer for Qualcomm boards. Currently supports IFC6410 and the DragonBoard 410c. |
| [meta-st-cannes2 BSP Layer](https://github.com/cpriouzeau/meta-st-cannes2) | This is the board support layer for ST B2260 board. |

# Package Dependencies

In order to successfully set up your build environment, you will need to install the following package dependencies.

**Step 1**: You will need git installed on your Linux host machine

`$ sudo apt-get install git`

**Step 2**: Visit the OpenEmbedded (Getting Started) wiki to see which distribution specific dependencies you will need

http://www.openembedded.org/wiki/Getting_started

**Step 3**: Install 96Boards specific dependencies (Case specific)

Setting up the build environment will first search for `whiptail`, if it is not present then it will search for `dialog`. You only need one of the following packages to ensure your setup-environement runs correctly:


`$ sudo apt-get install whiptail`

or

`$ sudo apt-get install dialog`

**Please Note**: If you are running Ubuntu 16.04 you will need to add the following line to your `/etc/apt/sources.list`

`deb http://archive.ubuntu.com/ubuntu/ xenial universe`

```shell
$ cd /etc/apt/
#vim text editor is used in this example
#sudo is used to allow editing, sources.list is set to read only
$ sudo vim sources.list
```

All required dependencies should now be installed on your host environment, you are ready to begin your build environment setup.


# Setup the build environment

The BSPs layesr can be used with any OE based distribution, such as Poky. The following instructions are provided to get started with OpenEmbedded Reference Software Platforms. 

To manage the various git trees and the OpenEmbedded environment, a repo manifest is provided. If you do not have `repo` installed on your host machine, you first need to install it, using the following instructions (or similar):

    mkdir -p ${HOME}/bin
    curl https://storage.googleapis.com/git-repo-downloads/repo > ${HOME}/bin/repo
    chmod a+x ${HOME}/bin/repo
    export PATH=${HOME}/bin:${PATH}

To initialize your build environment, you need to run:

    mkdir oe-rpb && cd oe-rpb
    repo init -u https://github.com/96boards/oe-rpb-manifest.git -b krogoth
    repo sync
    source setup-environment [<build folder>]

* after the command `repo sync` returns, all the OpenEmbedded recipes have been downloaded locally.
* you will be prompted to choose the target machine, pick `dragonboard-410c` or `hikey`
* you will be prompted to choose the distro, for now, it is recommended to use 'rpb'
* <build folder> is optional, if missing it will default to `build-$DISTRO`

The script `setup-environment` will create sane default configuration files in <build folder>/conf, you can inspect them and modify them if needed. Note that conf/local.conf and conf/bblayers.conf are symlink , and under source control. So it is generally better not to modify them, and use conf/site.conf and conf/auto.conf instead.

# Build a minimal, console-only image

To build a console image, you can run:

    $ bitbake rpb-console-image

At the end of the build, your build artifacts will be found under `tmp-eglibc/deploy/images/MACHINE/`. The two artifacts you will use to update your board are:
* `rpb-console-image-MACHINE.ext4.gz` and
* `boot-MACHINE.img`

where `MACHINE` is `dragonboard-410c` or `hikey`.

# Bootloaders and eMMC partitions

Build artifacts from your OE build will be flashed into the on-board eMMC (in contrast to some other boards which run their images from an SDcard). The OpenEmbedded BSP layer assumes that the _Linux_ Bootloaders and eMMC partition layout are used on the board (not the _Android_ ones; by default DragonBoards come pre-configured with the Android eMMC partition layout). You can download the latest Linux bootloader package for Dragonboard 410c from [here](http://builds.96boards.org/releases/dragonboard410c/linaro/rescue/latest/) to your development host, it will be named something like `dragonboard410c_bootloader_emmc_linux-<version>.zip`.

Whether your board is using the Android eMMC partition layout or the Linux partition eMMC layout, you will use the Android `fastboot` utility on your development host for managing the board's eMMC partitions. If you are using a relatively recent Linux distribution on your development host, it probably already has a package that includes the `fastboot` utility (it might be named something like `android-tools` or `android-tools-fastboot`) so go ahead and install it on your development host. In order for your development host's fastboot utility to interact with the board, in the case of the DragonBoard 410c, it must be booted into a special `fastboot mode`. The procedure to do so is as follows:
* remove power from your DragonBoard 410c
* plug a USB cable from your development host to your DragonBoard's J4 connector
* while holding down S4 on the DragonBoard 410c (the one marked "(-)"), insert the power adapter
* after a few seconds you can release S4

In the case of case of HiKey, see `TBA` (put a link to Debian instructions, they are the same).

To verify your cables and that the above procedure worked, on your development host run:

    # sudo fastboot devices

and you should get a non-empty response, e.g.

    # sudo fastboot devices
    83581d40        fastboot

If this is your first time using a particular board, you will need to switch its eMMC partition layout to the Linux layout, but this procedure only needs to be done once for a given board. After switching your layout, you only have to update your board with your latest build artifacts.

The procedure for updating your eMMC partitions is as follows. Put your DragonBoard into `fastboot mode` (see procedure above) then perform these steps on your development host:
* download the latest Linux bootloader package (e.g. `dragonboard410c_bootloader_emmc_linux-<version>.zip`)
* unzip its contents
* run the `flashall` script (as root) that you will find after unzipping the Linux bootloader package

At this point your eMMC has the following partition layout:

Dragonboard 410c:
* `/dev/mmcblk0p8` , aka `boot` is used for the boot image (kernel, device tree, initrd)
* `/dev/mmcblk0p10` , aka `rootfs` is used for the root file system

HiKey:
* `/dev/mmcblk0p6` , aka `boot` is used for the boot image (kernel, device tree, initrd)
* `/dev/mmcblk0p9` , aka `system` is used for the root file system

# Flashing build artifacts

In the following description, replace `IMAGE` with the name of the image you built. For example: if you built `rpb-console-image` then `IMAGE` will be `rpb-console-image`.
Same for `IMAGE`, replace with the name of your board. For example: if you built for `Dragonboard 410c` then `MACHINE` will be `dragonboard-410c`, if you built for `HiKey` then `MACHINE` will be `hikey`.

At the end of any successful build you will end up with the following artifacts (amongst others)
* `IMAGE-MACHINE.ext4.gz` and
* `boot-MACHINE.img`

These will be found in your `tmp-eglibc/deploy/images/MACHINE` directory.

To install these to your board's eMMC from your development host:

    # gunzip IMAGE-MACHINE.ext4.gz
    # fastboot flash boot boot-MACHINE.img

In the case of Dragonboard 410c:

    # fastboot flash rootfs IMAGE-MACHINE.ext4

In the case of HiKey:

    # ext2simg -v IMAGE-MACHINE.ext4 IMAGE-MACHINE.img
    # fastboot flash system IMAGE-MACHINE.img

# Proprietary firmware blob

When running the `setup-environment` script, you were asked to read/accept the Qualcomm EULA. The EULA is required to access the proprietary firmware, such as the GPU firmware , WLAN, ... 

If you accepted the EULA, when building an image for DragonBoard 410c all proprietary firmware are installed automatically in `/lib/firmware`, and a copy of the EULA is added as '/etc/license.txt`. 

If you did not accept the EULA, the firmware are not downloaded, and not installed into the image. You can manually manage the firmware and download them separately from [Qualcomm Developer Network](https://developer.qualcomm.com/download/linux-ubuntu-board-support-package-v1.1.zip).

# Build a simple X11 image

To build an X11 image with GPU hardware accelerated support, run:

    $ bitbake rpb-desktop-image

At the end of the build, the root file system image will be available as `tmp-eglibc/deploy/images/MACHINE/rpb-desktop-image-MACHINE.ext4.gz`.

Then you can finally start the X server, and run any graphical application:

    X&
    export DISPLAY=:0
    glxgears

The default X11 image includes `openbox` window manager, to use it:

    X&
    export DISPLAY=:0
    openbox &
    glxgears

Of course, you can easily add another window manager, such as `metacity` in the image. To install `metacity` in the image, add the following to `conf/auto.conf` file:

    CORE_IMAGE_EXTRA_INSTALL += "metacity"

and rebuild the `rpb-desktop-image` image, it will now include `metacity`, which can be started like this:

    X&
    export DISPLAY=:0
    metacity&
    glxgears

# Build a sample Wayland/Weston image

For Wayland/weston, it is needed to change the DISTRO and use `rpb-wayland` instead of `rpb`. The main reason is that in the `rpb-wayland` distro, the support for X11 is completely removed. So, in a new terminal prompt, setup a new environment and make sure to use `rpb-wayland` for DISTRO, then, you can run a sample image with:

    $ bitbake rpb-weston-image

This image includes a few additional features, such as `systemd`, `connman` which makes it simpler to use. Once built, the image will be available at `tmp-eglibc/deploy/images/MACHINE/rpb-weston-image-MACHINE.ext4.gz`. And it can be flashed into `rootfs` partition.

If you boot this image on the board, you should get a command prompt on the HDMI monitor. A user called `linaro` exists (and has no password). Once logged in a VT, you run start weston with:

    weston-launch

And that should get you to the Weston desktop shell.

# Build mixed 32bit/64bit image

OE RPB has support for creating mixed 32-bit/64-bit builds with 64-bit
kernel and 32-bit userland for:
* HiKey
* Dragonboard-410c

There are two variants of machine configuration for both HiKey and
Dragonboard-410c boards:

| Board |  MACHINE-32   |  MACHINE-64 |
|:-----:|:-------------:|:-----------:|
| HiKey | hikey-32 | hikey |
| DB-410c| dragonboard-410c-32 | dragonboard-410c |

MACHINE-32 configuration doesn't build the kernel. It is intended to
create the 32-bit root filesystem only.

MACHINE-64 configuration is universal. But in this mixed build only the
kernel and the kernel modules are needed from the 64-bit configuration,
so the 64-bit rpb-minimal-image is built.

## Running a mixed build

Setting up the build environment is the same as usual. The only difference
is that when running
```
$ . setup-environment
```
one should select `<MACHINE-32>` as the MACHINE.
DISTRO values can be:
* rpb-x11
* rpb-wayland

Then do
```
bitbake_secondary_image --extra-machine <MACHINE-64> <image>
```
e.g. if MACHINE=dragonboard-410c-32 and DISTRO=rpb-wayland were selected
when sourcing setup-environment, the command could be:
`bitbake_secondary_image --extra-machine dragonboard-410c rpb-weston-image`

## Creating the mixed rootfs image

`bitbake_secondary_image` actually runs two builds. So in the build directory,
under `tmp-*/deploy/images/` two directories are created: one for 32-bit build
artifacts, and the other for the 64-bit ones. E.g.
```
tmp-rpb_wayland-glibc/deploy/images/dragonboard-410c-32
```
and
```
tmp-rpb_wayland-glibc/deploy/images/dragonboard-410c
```

Unpack the 32-bit `*.rootfs.ext4` image, resize it to make sure that there is
enough space for the 64-bit modules, mount it via a loop device, and unpack the
64-bit modules into the 32-bit root filesystem. Then unmount the rootfs to get
the 32-bit rootfs.ext4 image with the 64-bit kernel modules added.

Please find more detailed instructions for the both boards below.

### Creating the image for Dragonboard-410c

Assuming that all the relevant build artifacts are in the current directory:
```
gunzip -k rpb-weston-image-dragonboard-410c-32-20161013104111.rootfs.ext4.gz
resize2fs rpb-weston-image-dragonboard-410c-32-20161013104111.rootfs.ext4 512M
mkdir root
sudo mount -o loop rpb-weston-image-dragonboard-410c-32-20161013104111.rootfs.ext4 root
cd root/
sudo tar xzf ../modules--4.4-r0-dragonboard-410c-20161013094521.tgz
cd ..
sync
sudo umount root
ext2simg rpb-weston-image-dragonboard-410c-32-20161013104111.rootfs.ext4 rpb-weston-image-dragonboard-410c.rootfs.img
```
The resulting rpb-weston-image-dragonboard-410c.rootfs.img with 32-bit userland
and 64-bit kernel modules can be flashed into the board with
```
fastboot flash rootfs rpb-weston-image-dragonboard-410c.rootfs.img
```

### Creating the image for HiKey

Creating the mixed tootfs image for HiKey is the same as for Dragonboard-410c,
but requires an extra step, as HiKey reads the kernel image to boot from the
rootfs (vs a boot partition in the case of Dragonboard-410c). So the 64-bit
kernel image and the DTB file must be copied to the 32-bit rootfs, the /boot
directory - this is where GRUB looks the kernel image for. E.g.:
```
mkdir root
mkdir root-64
gunzip -k rpb-minimal-image-hikey-20161014162659.rootfs.ext4.gz
sudo mount -o loop rpb-minimal-image-hikey-20161014162659.rootfs.ext4 root-64/
gunzip -k rpb-weston-image-hikey-32-20161014172406.rootfs.ext4.gz 
resize2fs rpb-weston-image-hikey-32-20161014172406.rootfs.ext4 512M
sudo mount -o loop rpb-weston-image-hikey-32-20161014172406.rootfs.ext4 root
sudo cp -r root-64/boot/* root/boot/
cd root
sudo tar xzf ../modules--4.4.11+git-r0-hikey-20161014162659.tgz
cd ..
sync
sudo umount root
sudo umount root-64
ext2simg rpb-weston-image-hikey-32-20161014172406.rootfs.ext4 rpb-weston-image-hikey.rootfs.img
```
The resulting rpb-weston-image-hikey.rootfs.img with a 32-bit userland, and
64-bit kernel modules and the kernel can be flashed into the board with
```
fastboot flash system rpb-weston-image-hikey.rootfs.img
```

# Support

For general question or support request, please go to [96boards.org Community forum](http://www.96boards.org/forums/forum/products/).

For any bug related to this release, please submit issues to the [Linaro Bug Tracking System](https://bugs.linaro.org/). To submit a bug, follow this [link](https://bugs.linaro.org/enter_bug.cgi?product=Reference%20Platforms).
