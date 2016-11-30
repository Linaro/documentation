This page provides instructions to get started with OpenEmbedded and the Yocto Project on the DragonBoard 410c. 

# Introduction

This wiki is not an introduction on OpenEmbedded or Yocto Project. If you are not familiar with OpenEmbedded and the Yocto Project, it is very much recommended to read the appropriate documentation first. For example, you can start with:
* http://openembedded.org/wiki/Main_Page
* http://yoctoproject.org/
* https://www.yoctoproject.org/documentation

In this wiki, we assume that the reader is familiar with basic concepts of OpenEmbedded.

The support for DragonBoard 410c is available in the [meta-qcom BSP layer](http://git.yoctoproject.org/cgit/cgit.cgi/meta-qcom).

This layer has been tested with OpenEmbedded Core layer, and is expected to work with any other standard layers and of course any OpenEmbedded based distributions.

The Linux kernel used for the DragonBoard 410c is the Linaro Landing team kernel, e.g. the same kernel used for the Linaro Linux release builds. The graphic stack is based on mesa, using the freedreno driver.

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

The Qualcomm BSP layer can be used with any OE based distribution, such as Poky. The following instructions are provided to get started with 96boards Open Embedded Reference Software Platforms. 

To manage the various git trees and the OpenEmbedded environment, a repo manifest is provided. If you do not have `repo` installed on your host machine, you first need to install it, using the following instructions (or similar):

    mkdir -p ${HOME}/bin
    curl https://storage.googleapis.com/git-repo-downloads/repo > ${HOME}/bin/repo
    chmod a+x ${HOME}/bin/repo
    export PATH=${HOME}/bin:${PATH}

To initialize your build environment, you need to run:

    mkdir oe-qcom && cd oe-qcom
    repo init -u https://github.com/96boards/oe-rpb-manifest.git -b jethro
    repo sync
    source setup-environment [<build folder>]

* after the command `repo sync` returns, all the OpenEmbedded recipes have been downloaded locally.
* you will be prompted to choose the target machine, pick `dragonboard-410c`
* you will be prompted to choose the distro, for now, it is recommended to use 'rpb'
* <build folder> is optional, if missing it will default to `build-$DISTRO`

The script `setup-environment` will create sane default configuration files in <build folder>/conf, you can inspect them and modify them if needed. Note that conf/local.conf and conf/bblayers.conf are symlink , and under source control. So it is generally better not to modify them, and use conf/site.conf and conf/auto.conf instead.

# Build a minimal, console-only image

To build a console image, you can run:

    $ bitbake rpb-console-image

At the end of the build, your build artifacts will be found in `tmp-eglibc/deploy/images/dragonboard-410c`. The two artifacts you will use to update your DragonBoard are:
* `rpb-console-image-dragonboard-410c.ext4.gz` and
* `boot-dragonboard-410c.img`

# Bootloaders and eMMC partitions

Build artifacts from your OE build will be flashed into the DragonBoard's on-board eMMC (in contrast to some other boards which run their images from an SDcard). The OpenEmbedded BSP layer assumes that the _Linux_ Bootloaders and eMMC partition layout are used on the DragonBoard 410c (not the _Android_ ones; by default DragonBoards come pre-configured with the Android eMMC partition layout). You can download the latest Linux bootloader package from [here](http://builds.96boards.org/releases/dragonboard410c/linaro/rescue/latest/) to your development host, it will be named something like `dragonboard410c_bootloader_emmc_linux-<version>.zip`.

Whether your DragonBoard is using the Android eMMC partition layout or the Linux partition eMMC layout, you will use the Android `fastboot` utility on your development host for managing the board's eMMC partitions. If you are using a relatively recent Linux distribution on your development host, it probably already has a package that includes the `fastboot` utility (it might be named something like `android-tools` or `android-tools-fastboot`) so go ahead and install it on your development host. In order for your development host's fastboot utility to interact with the DragonBoard, the DragonBoard must be booted into a special `fastboot mode`. The procedure to do so is as follows:
* remove power from your DragonBoard
* plug a USB cable from your development host to your DragonBoard's J4 connector
* while holding down S4 on the DragonBoard (the one marked "(-)"), insert the power adapter
* after a few seconds you can release S4

To verify your cables and that the above procedure worked, on your development host run:

    # sudo fastboot devices

and you should get a non-empty response, e.g.

    # sudo fastboot devices
    83581d40        fastboot

If this is your first time using a particular DragonBoard, you will need to switch its eMMC partition layout to the Linux layout, but this procedure only needs to be done once for a given board. After switching your layout, you only have to update your board with your latest build artifacts.

The procedure for updating your eMMC partitions is as follows. Put your DragonBoard into `fastboot mode` (see procedure above) then perform these steps on your development host:
* download the latest Linux bootloader package (e.g. `dragonboard410c_bootloader_emmc_linux-<version>.zip`)
* unzip its contents
* run the `flashall` script (as root) that you will find after unzipping the Linux bootloader package

At this point your eMMC has the following partition layout:

* `/dev/mmcblk0p7` , aka `aboot` is used for the bootloader (LK/fastboot)
* `/dev/mmcblk0p8` , aka `boot` is used for the boot image (kernel, device tree, initrd)
* `/dev/mmcblk0p10` , aka `rootfs` is used for the root file system

# Flashing build artifacts

In the following description, replace `image` with the name of the image you built. For example: if you built `rpb-console-image` then `image` will be `rpb-console-image`.

At the end of any successful build you will end up with the following artifacts (amongst others)
* `image-dragonboard-410c.ext4.gz` and
* `boot-dragonboard-410c.img`

These will be found in your `tmp-eglibc/deploy/images/dragonboard-410c` directory.

To install these to your DragonBoard's eMMC from your development host:

    # gzip -d < image-dragonboard-410c.ext4.gz > image-dragonboard-410c.ext4
    # fastboot flash rootfs image-dragonboard-410c.ext4
    # fastboot flash boot boot-dragonboard-410c.img
 
# Proprietary firmware blob

When running the `setup-environment` script, you were asked to read/accept the Qualcomm EULA. The EULA is required to access the proprietary firmware, such as the GPU firmware , WLAN, ... 

If you accepted the EULA, when building an image for DragonBoard 410c all proprietary firmware are installed automatically in `/lib/firmware`, and a copy of the EULA is added as '/etc/license.txt`. 

If you did not accept the EULA, the firmware are not downloaded, and not installed into the image. You can manually manage the firmware and download them separately from [Qualcomm Developer Network](https://developer.qualcomm.com/download/linux-ubuntu-board-support-package-v1.1.zip).

# Build a simple X11 image

To build an X11 image with GPU hardware accelerated support run:

    $ bitbake rpb-desktop-image

At the end of the build, the root file system image will be available as `tmp-eglibc/deploy/images/dragonboard-410c/rpb-desktop-image-dragonboard-410c.ext4.gz`.

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

For Wayland/weston, it is recommended to change the DISTRO and use `rpb-wayland` instead of `rpb`. The main reason is that in the `rpb-wayland` distro, the support for X11 is completely removed. So , in a new terminal prompt, setup a new environment and make sure to use `rpb-wayland` for DISTRO, then, you can run a sample image with:

    $ bitbake rpb-weston-image

This image includes a few additional features, such as `systemd`, `connman` which makes it simpler to use. Once built, the image will be available at `tmp-eglibc/deploy/images/dragonboard-410c/rpb-weston-image-dragonboard-410c.ext4.gz`. And it can be flashed into `rootfs` partition.

If you boot this image on the board, you should get a command prompt on the HDMI monitor. A user called `linaro` exists (and has no password). Once logged in a VT, you run start weston with:

    weston-launch

And that should get you to the Weston desktop shell.

# Support

For general question or support request, please go to [96boards.org Community forum](https://www.96boards.org/forums/forum/products/dragonboard410c/).

For any bug related to this release, please submit issues to the [96Board.org Bug tracking system](https://bugs.96boards.org/). To submit a bug, follow this [link](https://bugs.96boards.org/enter_bug.cgi?product=Dragonboard%20410c).
