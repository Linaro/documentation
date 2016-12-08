h4. Building AOSP from source

*NOTE*: This build instructions are not yet reflecting the 15.10 image

AOSP sources are hosted in these repositories:
* https://github.com/96boards/android_hardware_ti_wpan
* https://github.com/96boards/android_external_wpa_supplicant_8
* https://github.com/96boards/android_device_linaro_hikey
* https://github.com/96boards/android_manifest

*Build setup:*

Please setup the host machine by following the instructions here: "http://source.android.com/source/initializing.html":http://source.android.com/source/initializing.html

NOTE: The build tries to mount a loop device as fat partition to create the boot-fat.uefi.img filesystem image. Please make sure your user is allowed to run those commands in sudo without password by running "visudo" and appending the following lines (replacing "<USER>" with your username):

bc. <USER> ALL= NOPASSWD: /bin/mount
<USER> ALL= NOPASSWD: /bin/umount
<USER> ALL= NOPASSWD: /sbin/mkfs.fat
<USER> ALL= NOPASSWD: /bin/cp

*Download the code:*

bc. $ mkdir android/
$ cd android/

Download and extract the Mali vendor binaries in the above directory: http://builds.96boards.org/snapshots/hikey/linaro/binaries/20150706/vendor.tar.bz2

*Build the image:*

bc. $ repo init -u https://android.googlesource.com/platform/manifest -b android-5.1.1_r1\
> -g "default,-device,hikey"
$ cd .repo/
$ git clone https://github.com/96boards/android_manifest -b android-5.0 local_manifests
$ cd -
$ repo sync -j8
$ source build/envsetup.sh
$ lunch hikey-userdebug
$ make droidcore -j8
$ cd out/target/product/hikey

h4. TODO

* Update to reflect the latest/current build
