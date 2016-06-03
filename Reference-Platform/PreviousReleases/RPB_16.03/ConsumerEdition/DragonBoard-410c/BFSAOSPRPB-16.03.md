## AOSP RPB 16.03 - Build from Source

Additional AOSP repositories are hosted at:
- [https://github.com/96boards/android_device_linaro_db410c](https://github.com/96boards/android_device_linaro_db410c)
- [https://github.com/96boards/android_manifest](https://github.com/96boards/android_manifest)
- [https://github.com/rsalveti/linux (branch qcomlt-4.4)](https://github.com/rsalveti/linux)
- [https://github.com/robherring/mesa](https://github.com/robherring/mesa)
- [https://github.com/robherring/drm_gralloc](https://github.com/robherring/drm_gralloc)
- https://github.com/robherring/drm_hwcomposer](https://github.com/robherring/drm_hwcomposer)

*Build setup:*

Please setup the host machine by following the instructions here: [http://source.android.com/source/initializing.html](http://source.android.com/source/initializing.html)

Also install make sure to install the following packages:

```shell
sudo apt-get install libfdt-dev python-mako get text
```

*Download the firmware blobs:*

```shell
mkdir android/
cd android/
mkdir -p vendor/db410c
cd vendor/db410c
wget http://developer.qualcomm.com/download/db410c/firmware-410c-1.2.0.bin
sh firmware-410c-1.2.0.bin
cd -
```

*Build the image:*

```shell
repo init -u https://android.googlesource.com/platform/manifest -b android-6.0.1_r16
cd .repo
git clone https://github.com/96boards/android_manifest -b android-6.0-db410c local_manifests
cd -
repo sync -j8
source build/envsetup.sh
lunch db410c-userdebug
make droidcore -j8
cd out/target/product/db410c
```

