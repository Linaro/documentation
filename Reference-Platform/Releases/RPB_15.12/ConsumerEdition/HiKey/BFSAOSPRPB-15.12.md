h4. Building AOSP from source

Additional AOSP repositories are hosted at:
* https://github.com/96boards/android_hardware_ti_wpan
* https://github.com/96boards/android_device_linaro_hikey
* https://github.com/96boards/android_manifest
* https://github.com/96boards/linux (branch android-hikey-linaro-4.1)

*Build setup:*

Please setup the host machine by following the instructions here: "https://source.android.com/source/initializing.html":https://source.android.com/source/initializing.html

NOTE: The build tries to mount a loop device as fat partition to create the boot-fat.uefi.img filesystem image. Please make sure your user is allowed to run those commands in sudo without password by running "visudo" and appending the following lines (replacing "<USER>" with your username):

bc. <USER> ALL= NOPASSWD: /bin/mount
<USER> ALL= NOPASSWD: /bin/umount
<USER> ALL= NOPASSWD: /sbin/mkfs.fat
<USER> ALL= NOPASSWD: /bin/cp

*Download the code:*

bc. mkdir android/
cd android/

Download and extract the Mali vendor binaries in the above directory: "https://builds.96boards.org/snapshots/hikey/linaro/binaries/20150706/vendor.tar.bz2":https://builds.96boards.org/snapshots/hikey/linaro/binaries/20150706/vendor.tar.bz2

*Build the image:*

bc. repo init -u https://android.googlesource.com/platform/manifest -b android-6.0.1_r16 -g "default,-device,-non-default,hikey"
cd .repo/
git clone https://github.com/96boards/android_manifest -b android-6.0 local_manifests
cd -
repo sync -j8
source build/envsetup.sh
lunch hikey-userdebug
make droidcore -j8
cd out/target/product/hikey
