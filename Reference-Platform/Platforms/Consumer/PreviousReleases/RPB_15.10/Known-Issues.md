## Reference Platform Build - 15.10 Release - Known Issues

### UEFI - HiKey

- "Bug 117":https://bugs.96boards.org/show_bug.cgi?id=117 - UEFI fastboot uploads hangs for large images
   - Workaround is to use the fastboot support provided by the ARM TF (@bl1.bin@, provided by the binary @l-loader.bin@)

### Debian

#### HiKey

- Mali not supported, missing kernel and userspace support
- [Bug 20](https://bugs.96boards.org/show_bug.cgi?id=20) - USB kernel trace errors -22
- [Bug 43](https://bugs.96boards.org/show_bug.cgi?id=43) - Iceweasel browser exits after file download complete
- [Bug 86](https://bugs.96boards.org/show_bug.cgi?id=86) - Debian ALIP: resize UI screen when underlying DRM resolution changed.
- [Bug 143](https://bugs.96boards.org/show_bug.cgi?id=143) - Mouse cursor invisible after boot (until you open an application)
- [Bug 144](https://bugs.96boards.org/show_bug.cgi?id=144) - Shutdown is not clean
- [Bug 145](https://bugs.96boards.org/show_bug.cgi?id=145) - Thermal sensor is not readable
- [Bug 147](https://bugs.96boards.org/show_bug.cgi?id=147) - Highest resolution of 1080p monitor cannot be detected
- [Bug 148](https://bugs.96boards.org/show_bug.cgi?id=148) - Bluetooth doesn't work
- [Bug 150](https://bugs.96boards.org/show_bug.cgi?id=150) - Cannot login when the screen is locked
- [Bug 151](https://bugs.96boards.org/show_bug.cgi?id=151) - glxgears: couldn't get an RGB, Double-buffered visual
- [Bug 152](https://bugs.96boards.org/show_bug.cgi?id=152) - SD-Card doesn't work
- [Bug 159](https://bugs.96boards.org/show_bug.cgi?id=159) - No sound cards found
- [Bug 160](https://bugs.96boards.org/show_bug.cgi?id=160) - Behaviors of power on button not following hardware user guide

#### DragonBoard 410c

 - Freedreno graphics driver not provided by the image
    -Newer mesa, libdrm and freedreno xorg driver is needed (to be available as part of the next release)
   - Workaround is to enable the qcom overlay PPA, and install the required packages:

```
sudo su -
echo "deb http://repo.linaro.org/ubuntu/qcom-overlay jessie main" > /etc/apt/sources.list.d/qcom-overlay-repo.list
apt-get update
apt-get install libdrm2 libdrm-freedreno1 libegl1-mesa libegl1-mesa-drivers libgbm1 libgl1-mesa-dri libgl1-mesa-glx libglapi-mesa libgles1-mesa libgles2-mesa libosmesa6 libwayland-egl1-mesa libxatracker2 xserver-xorg-video-freedreno
reboot
```

- Slow USB throughput: https://www.96boards.org/forums/topic/super-slow-usb/
- [Bug 43](https://bugs.96boards.org/show_bug.cgi?id=43) - Iceweasel browser exits after file download complete
- [Bug 121](https://bugs.96boards.org/show_bug.cgi?id=121) - Cannot soft power off or shutdown db410c
- [Bug 154](https://bugs.96boards.org/show_bug.cgi?id=154) - glxgears and tuxracer benchmarks failed to run (due to the missing freedreno driver)
- [Bug 156](https://bugs.96boards.org/show_bug.cgi?id=156) - HDMI resolution change not workin
- [Bug 157](https://bugs.96boards.org/show_bug.cgi?id=157) - HDMI audio not working (MIGHT NOT BE A BUG)
- [Bug 165](https://bugs.96boards.org/show_bug.cgi?id=165) - HDMI Display sleep - no way to wake back up

### AOSP

#### HiKey

- [Bug 136](https://bugs.96boards.org/show_bug.cgi?id=136) - HDMI goes off while running CTS
- [Bug 161](https://bugs.96boards.org/show_bug.cgi?id=161) - Fails to enter sleep mode
- [Bug 163](https://bugs.96boards.org/show_bug.cgi?id=163) - HDMI audio not working
- [Bug 164](https://bugs.96boards.org/show_bug.cgi?id=164) - Behaviors of power on button not following hardware user guide
