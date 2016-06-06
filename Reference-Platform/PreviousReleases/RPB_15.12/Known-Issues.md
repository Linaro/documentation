## Reference Platform Build - 15.12 Release - Known Issues

### Enterprise

#### Kernel

- "Bug 1966":https://bugs.linaro.org/show_bug.cgi?id=1966 - KVM errors when booting on overdrive and d02

#### HiSilicon D02

- "Bug 1965":https://bugs.linaro.org/show_bug.cgi?id=1965 - D02: kernel unable to find valid mac for the network interfaces
- "Bug 1967":https://bugs.linaro.org/show_bug.cgi?id=1967 - D02: unhandled level 3 permission fault (11)
- "Bug 1975":https://bugs.linaro.org/show_bug.cgi?id=1975 - D02: Kernel can only see 2GB of memory (from 8GB)
- *SATA*: Due to bugs in the SATA controller, there is a risk of disk corruption when installing to a SATA disk. This is expected to be fixed in subsequent silicon revisions.
- *SAS*: not yet supported in EDK2. For it to work on linux only, this "patch":https://git.linaro.org/uefi/OpenPlatformPkg.git/commit/96d58c4318584f066b1bb7f1c48b72e7e25cf709 needs to be reverted on UEFI/EDK2, but then an alternative boot method needs to be used (since UEFI/EDK2 is unable to load grub/kernel from SAS).

#### AMD Overdrive

- UEFI/EDK2 is not yet supported

### Debian

#### HiKey

- Mali not supported, missing kernel and userspace support
- "Bug 20":https://bugs.96boards.org/show_bug.cgi?id=20 - USB kernel trace errors -22
- "Bug 43":https://bugs.96boards.org/show_bug.cgi?id=43 - Iceweasel browser exits after file download complete
- "Bug 86":https://bugs.96boards.org/show_bug.cgi?id=86 - Debian ALIP: resize UI screen when underlying DRM resolution changed.
- "Bug 143":https://bugs.96boards.org/show_bug.cgi?id=143 - Mouse cursor invisible after boot (until you open an application)
- "Bug 144":https://bugs.96boards.org/show_bug.cgi?id=144 - Shutdown is not clean
- "Bug 145":https://bugs.96boards.org/show_bug.cgi?id=145 - Thermal sensor is not readable
- "Bug 147":https://bugs.96boards.org/show_bug.cgi?id=147 - Highest resolution of 1080p monitor cannot be detected
- "Bug 148":https://bugs.96boards.org/show_bug.cgi?id=148 - Bluetooth doesn't work
- "Bug 151":https://bugs.96boards.org/show_bug.cgi?id=151 - glxgears: couldn't get an RGB, Double-buffered visual
- "Bug 152":https://bugs.96boards.org/show_bug.cgi?id=152 - SD-Card doesn't work
- "Bug 159":https://bugs.96boards.org/show_bug.cgi?id=159 - No sound cards found
- "Bug 160":https://bugs.96boards.org/show_bug.cgi?id=160 - Behaviors of power on button not following hardware user guide
- "Bug 166":https://bugs.96boards.org/show_bug.cgi?id=166 - Support 8GB emmc
- "Bug 211":https://bugs.96boards.org/show_bug.cgi?id=211 - Fails to enter fastboot mode from grub boot menu

#### DragonBoard 410c

- Freedreno graphics driver not provided by the image
   - Newer mesa, libdrm and freedreno xorg driver is needed (work in progress to be included by default as part of the next release)
   - Workaround is to enable the qcom overlay PPA, and install the required packages:

```shell
sudo su -
echo "deb http://repo.linaro.org/ubuntu/qcom-overlay jessie main" > /etc/apt/sources.list.d/qcom-overlay-repo.list
apt-get update
apt-get install libdrm2 libdrm-freedreno1 libegl1-mesa libegl1-mesa-drivers libgbm1 libgl1-mesa-dri libgl1-mesa-glx libglapi-mesa libgles1-mesa libgles2-mesa libosmesa6 libwayland-egl1-mesa libxatracker2 xserver-xorg-video-freedreno
reboot
```

* Slow USB throughput: "https://www.96boards.org/forums/topic/super-slow-usb/":https://www.96boards.org/forums/topic/super-slow-usb/
* "Bug 43":https://bugs.96boards.org/show_bug.cgi?id=43 - Iceweasel browser exits after file download complete
* "Bug 121":https://bugs.96boards.org/show_bug.cgi?id=121 - Cannot soft power off or shutdown db410c
* "Bug 154":https://bugs.96boards.org/show_bug.cgi?id=154 - glxgears and tuxracer benchmarks failed to run (due to the missing freedreno driver)
* "Bug 160":https://bugs.96boards.org/show_bug.cgi?id=160 - Behaviors of power on button not following hardware user guide
* "Bug 207":https://bugs.96boards.org/show_bug.cgi?id=208 - Bluetooth does not work on Dragonboard debian
* "Bug 208":https://bugs.96boards.org/show_bug.cgi?id=208 - Real Time clock not working: due to /dev/rtc not found

### AOSP

#### HiKey

* "Bug 20":https://bugs.96boards.org/show_bug.cgi?id=20 - USB kernel trace errors -22
* "Bug 124":https://bugs.96boards.org/show_bug.cgi?id=124 - CPU frequency will be reset to lowest when it is heavily loaded
* "Bug 136":https://bugs.96boards.org/show_bug.cgi?id=136 - HDMI goes off while running CTS
* "Bug 163":https://bugs.96boards.org/show_bug.cgi?id=163 - HDMI audio not working
* "Bug 164":https://bugs.96boards.org/show_bug.cgi?id=164 - Behaviors of power on button not following hardware user guide
* "Bug 180":https://bugs.96boards.org/show_bug.cgi?id=180 - Shutdown cannot turn off HDMI monitor
* "Bug 204":https://bugs.96boards.org/show_bug.cgi?id=204 - File download crashes the build-in browser
