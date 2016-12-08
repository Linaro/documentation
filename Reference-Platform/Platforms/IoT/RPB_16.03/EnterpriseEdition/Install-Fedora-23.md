## Installing Fedora 23

This guide is not to be a replacement of the official Fedora 23 Installer documentation, but instead be a quick walkthrough for the network installer. You can find the original documentation at [https://fedoraproject.org/wiki/Architectures/AArch64/F23/Installation](https://fedoraproject.org/wiki/Architectures/AArch64/F23/Installation)

### Setting up the TFTP server

Back to your dnsmasq server (check [this link](DHCP-TFTP-Server-UEFI.md) for instructions on how to setup your own TFTP/DCHP server), download the required Fedora 23 installer files at your tftp-root directory. In this example, this directory is configured to `/srv/tftp`.

Downloading required Grub 2 UEFI files:

**Note:** Because of bug [1251600](https://bugzilla.redhat.com/show_bug.cgi?id=1251600), we need to use both `BOOTAA64.EFI` and `grubaa64.efi` from the Fedora 22 release.

```shell
sudo su -
cd /srv/tftp/
wget http://dl.fedoraproject.org/pub/fedora-secondary/releases/22/Server/aarch64/os/EFI/BOOT/BOOTAA64.EFI
wget http://dl.fedoraproject.org/pub/fedora-secondary/releases/22/Server/aarch64/os/EFI/BOOT/grubaa64.efi
```

Downloading upstream Kernel and Initrd

```shell
mkdir /srv/tftp/f23
cd /srv/tftp/f23
wget http://dl.fedoraproject.org/pub/fedora-secondary/releases/23/Server/aarch64/os/images/pxeboot/vmlinuz
wget http://dl.fedoraproject.org/pub/fedora-secondary/releases/23/Server/aarch64/os/images/pxeboot/initrd.img
```

Creating the Grub 2 config file (`grub.cfg`):

```shell
menuentry 'Install Fedora 23 ARM 64-bit' --class fedora --class gnu-linux --class gnu --class os {
    linux (tftp)/f23/vmlinuz ip=dhcp inst.repo=http://dl.fedoraproject.org/pub/fedora-secondary/releases/23/Server/aarch64/os/
    initrd (tftp)/f23/initrd.img
}
```

You should now have the following file tree structure:

```shell
/srv/tftp/
├── BOOTAA64.EFI
├── f23
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
Install Fedora 23 ARM 64-bit
.
Use the  and  keys to change the selection.                       
Press 'e' to edit the selected item, or 'c' for a command prompt.
```

Now just hit enter and wait for the kernel and initrd to load, which automatically loads the installer and provides you the installer console menu, so you can finally install Fedora 23 (just make sure that target device has external network access, since the installer is downloaded automatically after booting the kernel).

You should see the following:

```shell
EFI stub: Booting Linux Kernel...
EFI stub: Using DTB from configuration table
EFI stub: Exiting boot services and installing virtual address map...
[    0.000000] Booting Linux on physical CPU 0x0
[    0.000000] Initializing cgroup subsys cpuset
[    0.000000] Initializing cgroup subsys cpu
[    0.000000] Initializing cgroup subsys cpuacct
[    0.000000] Linux version 4.2.3-300.fc23.aarch64 (mockbuild@aarch64-08a.arm.fedoraproject.org) (gcc version 5.1.1 20150618 (Red Hat 5.1.1-4) (GCC) ) #1 SMP Thu Oct 8 01:39:38 UTC 2015
[    0.000000] CPU: AArch64 Processor [411fd072] revision 2
[    0.000000] Detected PIPT I-cache on CPU0
[    0.000000] alternatives: enabling workaround for ARM erratum 832075
[    0.000000] efi: Getting EFI parameters from FDT:
[    0.000000] EFI v2.40 by American Megatrends
[    0.000000] efi:  ACPI 2.0=0x83ff1c6000  SMBIOS 3.0=0x83ff349718 
...
Welcome to Fedora 23 (Twenty Three) dracut-043-60.git20150811.fc23 (Initramfs)!
...
[   23.105835] dracut-initqueue[685]: RTNETLINK answers: File exists
[   23.756828] dracut-initqueue[685]: % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
[   23.757345] dracut-initqueue[685]: Dload  Upload   Total   Spent    Left  Speed
100   958  100   958    0     0   1514      0 --:--:-- --:--:-- --:--:--  1513  0 --:--:-- --:--:-- --:--:--     0
...
Welcome to Fedora 23 (Twenty Three)!
...
Starting installer, one moment...
anaconda 23.19.10-1 for Fedora 23 started.
 * installation log files are stored in /tmp during the installation
 * shell is available on TTY2
 * if the graphical installation interface fails to start, try again with the
   inst.text bootoption to start text installation
 * when reporting a bug add logs from /tmp as separate text/plain attachments
00:29:26 X startup failed, falling back to text mode
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
  'r' to refresh]: 
.
[anaconda]1:main* 2:shell  3:log  4:storage-log >Switch tab: Alt+Tab | Help: F1 
```

For the text mode installer, just enter `2` and follow the instructions available in the console.

Menu items without that are not `[x]` must be set. Enter the menu number associated with the menu in order to configure it.

### Finishing the installation

After selecting the installation destination, the partitioning scheme, root password and users (optional), just enter `b` to proceed with the installation.

Once the installation is completed, you should be able to simply reboot the system in order to boot your new Fedora 23 system.

### Automating the installation with kickstart

TODO
