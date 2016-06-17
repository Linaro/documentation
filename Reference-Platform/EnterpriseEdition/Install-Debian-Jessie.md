## Installing Debian "Jessie" 8.5

This guide is not to be a replacement of the official Debian Installer documentation, but instead be a quick walkthrough for the network installer. You can find the original documentation at [https://www.debian.org/releases/jessie/arm64/index.html.en](https://www.debian.org/releases/jessie/arm64/index.html.en)

### Debian Installer

The released debian-installer from Debian Jessie contains a kernel based on 3.16, which doesn't yet provide support for development boards used by the reference software project. For a complete enterprise experience (including support for tip-based kernel with ACPI support and additional platforms), we also build and publish a custom debian installer that incorporates a more recent kernel.

Our custom installer changes nothing more than the kernel, and you can also find the instructions to build it from source at the end of this document.

## Loading debian-installer from the network
### Setting up the TFTP server

Back to your dnsmasq server (check [this link](../DHCP-TFTP-Server-UEFI.md) for instructions on how to setup your own TFTP/DCHP server), download the required Debian installer files at your tftp-root directory. In this example, this directory is configured to `/srv/tftp`.

Since the kernel, initrd and GRUB 2 is part of the debian-installer tarball (`netboot.tar.gz`), that is the only file you will need to download and use.

#### Downloading debian-installer:

```shell
sudo su -
cd /srv/tftp/
wget https://builds.96boards.org/releases/reference-platform/components/debian-installer/16.06/netboot.tar.gz
tar -zxvf netboot.tar.gz
```

You should now have the following file tree structure:

```shell
/srv/tftp/
├── debian-installer
│   └── arm64
│       ├── bootnetaa64.efi
│       ├── grub
│       │   ├── arm64-efi
│       │   │   ├── acpi.mod
│       │   │   ├── adler32.mod
│       │   │   ├── all_video.mod
│       │   │   ├── archelp.mod
│       │   │   ├── bfs.mod
│       │   │   ├── bitmap.mod
│       │   │   ├── bitmap_scale.mod
│       │   │   ├── blocklist.mod
│       │   │   ├── boot.mod
│       │   │   ├── btrfs.mod
│       │   │   ├── bufio.mod
...
│       │   │   ├── xzio.mod
│       │   │   └── zfscrypt.mod
│       │   ├── font.pf2
│       │   └── grub.cfg
│       ├── initrd.gz
│       └── linux
├── netboot.tar.gz
└── version.info
```

Now just make sure that `/etc/dnsmasq.conf` is pointing out to the right boot file, like:

```shell
dhcp-boot=debian-installer/arm64/bootnetaa64.efi
```
## Loading debian-installer from the minimal CD

Together with the debian-installer netboot files, a minimal ISO is also provided containing the same installer, which can be loaded as normal boot disk media.

Making a bootable SATA disk / USB stick / microSD card (attention to **/dev/sdX**, make sure that it is your target device first):

```
wget https://builds.96boards.org/releases/reference-platform/components/debian-installer/16.06/mini.iso
sudo cp mini.iso /dev/sdX
sync
```

Please refer to the [debian-manual](https://www.debian.org/releases/jessie/amd64/ch04s03.html.en) for a more complete guide on creating a CD, SATA disk, USB stick or micro SD with the minimal ISO.

## Booting the installer

If you are booting the installer from the network, simply select PXE boot when presented by UEFI (make sure to boot with the right network interface, in case more than one is available). In case you are booting with the minimal ISO via SATA / USB / microSD, simply select the right boot option in UEFI.

You should see the following (using AMD Seattle's Overdrive as example):

```shell
NOTICE:  BL3-1: 
NOTICE:  BL3-1: Built : 18:22:46, Nov 23 2015
INFO:    BL3-1: Initializing runtime services
INFO:    BL3-1: Preparing for EL3 exit to normal world
INFO:    BL3-1: Next image address = 0x8000000000
INFO:    BL3-1: Next image spsr = 0x3c9
Boot firmware (version  built at 18:27:24 on Nov 23 2015)
Version 2.17.1249. Copyright (C) 2015 American Megatrends, Inc.                 
BIOS Date: 11/23/2015 18:23:09 Ver: ROD0085E00                                  
Press <DEL> or <ESC> to enter setup.  
.
>>Checking Media Presence......
>>Media Present......
>>Start PXE over IPv4.
  Station IP address is 192.168.3.57
  Server IP address is 192.168.3.1
  NBP filename is BOOTAA64.EFI
  NBP filesize is 885736 Bytes
>>Checking Media Presence......
>>Media Present......
 Downloading NBP file...
 Succeed to download NBP file.
 Fetching Netboot Image
```

At this stage you should be able to see the Grub 2 menu, like:

```shell
Install
Advanced options ...
Install with speech synthesis
.
Use the  and  keys to change the selection.                       
Press 'e' to edit the selected item, or 'c' for a command prompt.
```

Now just hit enter and wait for the kernel and initrd to load, which automatically loads the installer and provides you the installer console menu, so you can finally install Debian.

You should see the following:

```shell
EFI stub: Booting Linux Kernel...
EFI stub: Using DTB from configuration table
EFI stub: Exiting boot services and installing virtual address map...
[    0.355175] ACPI: IORT: Failed to get table, AE_NOT_FOUND
[    0.763784] kvm [1]: error: no compatible GIC node found
[    0.763818] kvm [1]: error initializing Hyp mode: -19
[    0.886298] Failed to find cpu0 device node
[    0.947082] zswap: default zpool zbud not available
[    0.951959] zswap: pool creation failed
Starting system log daemon: syslogd, klogd.
...
  ┌───────────────────────┤ [!!] Select a language ├────────────────────────┐
  │                                                                         │
  │ Choose the language to be used for the installation process. The        │
  │ selected language will also be the default language for the installed   │
  │ system.                                                                 │
  │                                                                         │
  │ Language:                                                               │
  │                                                                         │
  │                               C                                         │
  │                               English                                   │
  │                                                                         │
  │     <Go Back>                                                           │
  │                                                                         │
  └─────────────────────────────────────────────────────────────────────────┘
<Tab> moves; <Space> selects; <Enter> activates buttons
```

### Finishing the installation

For using the installer, please check the documentation available at [https://www.debian.org/releases/jessie/arm64/ch06.html.en](https://www.debian.org/releases/jessie/arm64/ch06.html.en)

**NOTE - Cello Only:** In case your mac address is empty (e.g. early boards), you will be required to change your default network mac address in order to proceed with the network install. Please open a shell after booted the installer (the installer offers the shell option at the first menu), and change the mac address as described below. Once changed, simply proceed with the install process.

```
~ # ip link set dev enp1s0 address de:5e:60:e4:6b:1f
~ # exit
```

Once the installation is completed, you should be able to simply reboot the system in order to boot your new Debian system.

**NOTE - Cello Only:** If you had to set a valid mac address during the installer, you will be required to also set the mac address in debian, after your first boot. Please change _/etc/network/interfaces_ and add your mac address again, like below:

```
root@debian:~# cat /etc/network/interfaces
...
allow-hotplug enp1s0
iface enp1s0 inet dhcp
  hwaddress ether de:5e:60:e4:6b:1f
```

### Automating the installation using preseeding

Preseeding provides a way to set answers to questions asked during the installation process, without having to manually enter the answers while the installation is running. This makes it possible to fully automate the installation over network, when used together with the debian-installer.

This document only provides a quick way for you to get started with preseeding. For the complete guide, please check the [Debian GNU/Linux Installation Guide](https://www.debian.org/releases/jessie/arm64/apb.html) and [example-preseed.txt](https://www.debian.org/releases/jessie/example-preseed.txt)

**Note:** Since we require an external kernel to be installed during the install process, this is done via the `preseed/late_command` argument, so you if you decide to use that command as part of your preseed file, make sure to add the following as part of the multi-line command:

```shell
d-i preseed/late_command string in-target apt-get install -y linux-image-reference-arm64; # here you can add 'in-target foobar' for additional commands
```

#### Creating the preseed file

Check [example-preseed.txt](https://www.debian.org/releases/jessie/example-preseed.txt) for a wide list of options supported by the Debian Jessie installer. Your file needs to use a similar format, but customized for your own needs.

Once created, make sure the file gets published into a network address that can be reachable from your target device.

Preseed example (`preseed.cfg`):

```shell
d-i debian-installer/locale string en_US
d-i keyboard-configuration/xkb-keymap select us
d-i netcfg/dhcp_timeout string 60
d-i netcfg/get_hostname string unassigned-hostname
d-i netcfg/get_domain string unassigned-domain
d-i netcfg/hostname string debian
d-i mirror/country string manual
d-i mirror/http/hostname string httpredir.debian.org
d-i mirror/http/directory string /debian
d-i mirror/http/proxy string
d-i passwd/root-password password linaro123
d-i passwd/root-password-again password linaro123
d-i passwd/user-fullname string Linaro User
d-i passwd/username string linaro
d-i passwd/user-password password linaro
d-i passwd/user-password-again password linaro
d-i passwd/user-default-groups string audio cdrom video sudo
d-i time/zone string UTC
d-i clock-setup/ntp boolean true
d-i clock-setup/utc boolean true
d-i partman-auto/disk string /dev/sda
d-i partman-auto/method string regular
d-i partman-lvm/device_remove_lvm boolean true
d-i partman-md/device_remove_md boolean true
d-i partman-auto/choose_recipe select atomic
d-i partman/confirm_write_new_label boolean true
d-i partman/choose_partition select finish
d-i partman/confirm boolean true
d-i partman/confirm_nooverwrite boolean true
popularity-contest popularity-contest/participate boolean false
tasksel tasksel/first multiselect standard, web-server
d-i pkgsel/include string openssh-server build-essential ca-certificates sudo vim ntp
d-i pkgsel/upgrade select safe-upgrade
d-i finish-install/reboot_in_progress note
```

In this example, this content is also available at [http://people.linaro.org/~ricardo.salveti/preseed.cfg](http://people.linaro.org/~ricardo.salveti/preseed.cfg)

#### Setting up grub.cfg

Now back to your tftp server, change the original `grub.cfg` file adding the location of your preseed file:

```shell
$ cat /srv/tftp/debian-installer/arm64/grub/grub.cfg 
# Force grub to automatically load the first option
set default=0
set timeout=1
menuentry 'Install with preseeding' {
    linux    /debian-installer/arm64/linux auto=true priority=critical url=http://people.linaro.org/~ricardo.salveti/preseed.cfg ---
    initrd   /debian-installer/arm64/initrd.gz
}
```

The `auto` kernel parameter is an alias for `auto-install/enable` and setting it to `true` delays the locale and keyboard questions until after there has been a chance to preseed them, while `priority` is an alias for `debconf/priority` and setting it to `critical` stops any questions with a lower priority from being asked.

In case your system contains more than one network interface, also make sure to add the one to be used via the `interface` argument, like `interface=eth1`.

#### Booting the system

Now just do a normal PXE boot, and debian-installer should automatically load and use the preseeds file provided by `grub.cfg`. In case there is still a dialog that stops your installation that means not all the debian-installer options are provided by your preseeds file. Get back to [example-preseed.txt](https://www.debian.org/releases/jessie/example-preseed.txt) and try to identify what is missing step.

Also make sure to check debian-installer's `/var/log/syslog` (by opening a shell) when debugging the installer.

### Building debian-installer from source

#### Build kernel package and udebs

Check the Debian [kernel-handbook](http://kernel-handbook.alioth.debian.org/ch-common-tasks.html) for the instructions required to build the debian kernel package from scratch. Since the installer only understands `udeb` packages, it is a good idea to reuse the official kernel packaging instructions and rules.

You can also find the custom kernel source package created as part of the EE-RPB effort at [https://builds.96boards.org/snapshots/reference-platform/components/linux/enterprise/latest/](https://builds.96boards.org/snapshots/reference-platform/components/linux/enterprise/latest/)

#### Rebuilding debian-installer with the new udebs

To build the installer, make sure you're running on a native `arm64` system, preferably running Debian Jessie.

Download the installer (from jessie):

```shell
sudo apt-get build-dep debian-installer
dget http://ftp.us.debian.org/debian/pool/main/d/debian-installer/debian-installer_20150422+deb8u4.dsc
```

Change the kernel abi and set a default local preseed (so it can install your kernel during the install process):

```shell
cd debian-installer-*
cd build
sed -i "s/LINUX_KERNEL_ABI.*/LINUX_KERNEL_ABI = YOUR_KERNEL_ABI/g" config/common
sed -i "s/PRESEED.*/PRESEED = default-preseed/g" config/common
```

Download the kernel udebs that you created at the localudebs folder:

```shell
cd localudebs
wget <list of your custom udeb files created by the kernel debian package>
cd ..
```

Create a local pkg-list to include the udebs created (otherwise d-i will not be able to find them online):

```shell
cat <<EOF > pkg-lists/local
ext4-modules-\${kernel:Version}
fat-modules-\${kernel:Version}
btrfs-modules-\${kernel:Version}
md-modules-\${kernel:Version}
efi-modules-\${kernel:Version}
scsi-modules-\${kernel:Version}
jfs-modules-\${kernel:Version}
xfs-modules-\${kernel:Version}
ata-modules-\${kernel:Version}
sata-modules-\${kernel:Version}
usb-storage-modules-\${kernel:Version}
EOF
```

Set up the local repo, so the installer can locate your udebs (from localudebs):

```shell
cat <<EOF > sources.list.udeb
deb [trusted=yes] copy:/PATH/TO/your/installer/d-i/debian-installer-20150422/build/ localudebs/
deb http://httpredir.debian.org/debian jessie main/debian-installer
EOF
```

Default preseed to skip known errors (as the kernel provided by local udebs):

```
cat <<EOF > default-preseed
# Continue install on "no kernel modules were found for this kernel"
d-i anna/no_kernel_modules boolean true
# Continue install on "no installable kernels found"
d-i base-installer/kernel/skip-install boolean true
d-i base-installer/kernel/no-kernels-found boolean true
d-i preseed/late_command string in-target wget <your linux-image.deb>; dpkg -i linux-image-*.deb 
EOF
```

Now just build the installer:

```shell
fakeroot make build_netboot
```

You should now find your custom debian-installer at `dest/netboot/netboot.tar.gz`.
