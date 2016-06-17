## Installing CentOS 7.2 15.11 - Reference Platform Enterprise

This guide is not to be a replacement of the official CentOS Installer documentation, but instead be a quick walkthrough for the network installer. You can find the original documentation at [https://wiki.centos.org/SpecialInterestGroup/AltArch/AArch64](https://wiki.centos.org/SpecialInterestGroup/AltArch/AArch64)

### Setting up the TFTP server

Back to your dnsmasq server (check [this link](DHCP-TFTP-Server-UEFI.md) for instructions on how to setup your own TFTP/DCHP server), download the required CentOS 7 installer files at your tftp-root directory. In this example, this directory is configured to `/srv/tftp`.

Downloading required Grub 2 UEFI files:

```shell
sudo su -
cd /srv/tftp/
wget http://mirror.centos.org/altarch/7/os/aarch64/EFI/BOOT/BOOTAA64.EFI
wget http://mirror.centos.org/altarch/7/os/aarch64/EFI/BOOT/grubaa64.efi
```

#### Downloading the CentOS installer from the Reference Platform 16.06 release (4.4.11 RP Kernel):

```shell
mkdir /srv/tftp/centos7
cd /srv/tftp/centos7
wget https://builds.96boards.org/releases/reference-platform/components/centos-installer/16.06/images/pxeboot/vmlinuz
wget https://builds.96boards.org/releases/reference-platform/components/centos-installer/16.06/images/pxeboot/initrd.img
```

Creating the Grub 2 config file (`grub.cfg`):

```shell
menuentry 'Install CentOS 7 ARM 64-bit - Reference Platform - 16.06' --class red --class gnu-linux --class gnu --class os {
    linux (tftp)/centos7/vmlinuz ip=dhcp inst.stage2=https://builds.96boards.org/releases/reference-platform/components/centos-installer/16.06/ inst.repo=http://mirror.centos.org/altarch/7/os/aarch64/ inst.ks=file:/ks.cfg
    initrd (tftp)/centos7/initrd.img
}
```

**Note:** `inst.ks` is required because of the additional linaro-overlay repository (which contains the reference platform kernel rpm package), which is available inside the `initrd.img`. The `inst.ks` contains only one line, which is used by the installer to fetch and install the right kernel package. The content: `repo --name="linaro-overlay" --baseurl=http://repo.linaro.org/rpm/linaro-overlay/centos-7/repo/`.

Also check the [RHEL 7](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-anaconda-boot-options.html) and the [anaconda documentation](https://rhinstaller.github.io/anaconda/boot-options.html) for additional boot options. One example is using **ip=eth1:dhcp** if you want to use the second network interface as default.

You should now have the following file tree structure:

```shell
/srv/tftp/
├── BOOTAA64.EFI
├── centos7
│   ├── initrd.img
│   └── vmlinuz
├── grubaa64.efi
└── grub.cfg
```

Now just make sure that @/etc/dnsmasq.conf@ is pointing out to the right boot file, like:

```shell
dhcp-boot=BOOTAA64.EFI
```

### Booting the installer

Now boot your platform of choice, selecting PXE boot when presented by UEFI (make sure to boot with the right network interface, in case more than one is available).

You should see the following (using AMD Seattle's Overdrive as example):

```shell
NOTICE:  BL3-1: 
NOTICE:  BL3-1: Built : 15:14:55, Feb  9 2016
INFO:    BL3-1: Initializing runtime services
INFO:    BL3-1: Preparing for EL3 exit to normal world
INFO:    BL3-1: Next image address = 0x8000e80000
INFO:    BL3-1: Next image spsr = 0x3c9
Boot firmware (version  built at 15:18:14 on Feb  9 2016)
Version 2.17.1249. Copyright (C) 2016 American Megatrends, Inc.                 
BIOS Date: 02/09/2016 15:15:23 Ver: ROD1001A00                                  
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
Install CentOS 7 ARM 64-bit - Reference Platform - 16.06
.
Use the  and  keys to change the selection.                       
Press 'e' to edit the selected item, or 'c' for a command prompt.
```

Now just hit enter and wait for the kernel and initrd to load, which automatically loads the installer and provides you the installer console menu, so you can finally install CentOS 7.

You should see the following:

```shell
EFI stub: Booting Linux Kernel...
EFI stub: Using DTB from configuration table
EFI stub: Exiting boot services and installing virtual address map...
[    0.000000] Booting Linux on physical CPU 0x0
[    0.000000] Initializing cgroup subsys cpuset
[    0.000000] Initializing cgroup subsys cpu
[    0.000000] Initializing cgroup subsys cpuacct
[    0.000000] Linux version 4.4.0-reference.104.aarch64 (buildslave@r2-a19) (gcc version 4.8.3 20140911 (Red Hat 4.8.3-9) (GCC) ) #1 SMP Tue Mar 1 20:52:15 UTC 2016
[    0.000000] Boot CPU: AArch64 Processor [411fd072]
[    0.000000] efi: Getting EFI parameters from FDT:
[    0.000000] EFI v2.40 by American Megatrends
[    0.000000] efi:  ACPI 2.0=0x83ff1c3000  SMBIOS 3.0=0x83ff347798 
...
Welcome to CentOS Linux 7 (AltArch) dracut-033-359.el7 (Initramfs)!
...
dracut-initqueue[610]: RTNETLINK answers: File exists
dracut-initqueue[610]: % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
dracut-initqueue[610]: Dload  Upload   Total   Spent    Left  Speed
100   287  100   287    0     0    390      0 --:--:-- --:--:-- --:--:--   389:--:-- --:--:--     0
...
Welcome to CentOS Linux 7 (AltArch)!
...
Starting installer, one moment...
anaconda 21.48.22.56-1 for CentOS Linux AltArch 7 started.
 * installation log files are stored in /tmp during the installation
 * shell is available on TTY2
 * if the graphical installation interface fails to start, try again with the
   inst.text bootoption to start text installation
 * when reporting a bug add logs from /tmp as separate text/plain attachments
21:06:29 X startup failed, falling back to text mode
================================================================================
================================================================================
VNC
.
X was unable to start on your machine.  Would you like to start VNC to connect t
o this computer from another computer and perform a graphical installation or co
ntinue with a text mode installation?
.
 1) Start VNC
.
 2) Use text mode
.
  Please make your choice from above ['q' to quit | 'c' to continue |
  'r' to refresh]: 2
[anaconda] 1:main* 2:shell  3:log  4:storage-log  5:program-log                 
```

For the text mode installer, just enter `2` and follow the instructions available in the console.

Menu items without that are not `[x]` must be set. Enter the menu number associated with the menu in order to configure it.

### Finishing the installation

After selecting the install destination, partitioning scheme, root password and users (optional), just enter `b` to proceed with the installation.

Once the installation is completed, you should be able to simply reboot the system in order to boot into your new CentOS 7 system.

### Automating the installation with kickstart

It is possible to fully automate the installer by providing a file called kickstart. The kickstart file is a plain text file, containing keywords that serve as directions for the installation. Check the RHEL 7 [kickstart guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/sect-kickstart-howto.html) for further information about how to create your own kickstart file.

Kickstart example:

```shell
cmdline
url --url="http://mirror.centos.org/altarch/7/os/aarch64/"
repo --name="CentOS" --baseurl=http://mirror.centos.org/altarch/7/os/aarch64/
repo --name="Updates" --baseurl=http://mirror.centos.org/altarch/7/updates/aarch64/
repo --name="linaro-overlay" --baseurl=http://repo.linaro.org/rpm/linaro-overlay/centos-7/repo/
lang en_US.UTF-8
keyboard us
timezone --utc Etc/UTC
auth --useshadow --passalgo=sha512
rootpw --lock --iscrypted locked
firewall --disabled
firstboot --disabled
selinux --disabled
reboot
network --bootproto=dhcp --device=eth0 --activate --onboot=on
ignoredisk --only-use=sda
bootloader --location=mbr
clearpart --drives=sda --all --initlabel
part /boot/efi --fstype=efi --grow --maxsize=200 --size=20
part /boot --fstype=ext4 --size=512
part / --fstype=ext4 --size=10240 --grow
part swap --size=4000
%packages
wget
net-tools
chrony
%end
%post
useradd -m -U -G wheel linaro
echo linaro | passwd linaro --stdin
%end
```

#### Setting up grub.cfg

Now back to your tftp server, change the original grub.cfg file adding the location of your kickstart file:

```shell
menuentry 'Install CentOS 7 ARM 64-bit - Reference Platform - 16.06' --class red --class gnu-linux --class gnu --class os {
    linux (tftp)/centos7/vmlinuz ip=dhcp inst.stage2=https://builds.96boards.org/releases/reference-platform/components/centos-installer/16.06/ inst.ks=http://people.linaro.org/~ricardo.salveti/centos-ks.cfg
    initrd (tftp)/centos7/initrd.img
}
```

In case your system contains more than one network interface, also make sure to add the one to be used via the `ip` argument, like `ip=eth0:dhcp`.

#### Booting the system

Now just do a normal PXE boot, and anaconda should automatically load and use the kickstart file provided by grub.cfg. In case there is still a dialog that stops your installation that means not all the installer options are provided by your kickstart file. Get back to official documentation and try to find out what is the missing step.
