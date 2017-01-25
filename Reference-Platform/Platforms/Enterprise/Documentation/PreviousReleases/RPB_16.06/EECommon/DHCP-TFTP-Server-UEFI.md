## Setting up DHCP/TFTP server for UEFI distro network installers

A simple way to install the major Linux Distributions (e.g. Debian, Fedora, CentOS, openSUSE, etc) is by booting the network installer via PXE. In order to have a working PXE environment, a DHCP and TFTP server is required, which is responsible for providing the target device a valid IP configuration and the required files to boot the system (usually Grub 2 + kernel + initrd).

In order to simplify the setup, this document will use dnsmasq, which is a lightweight, easy to configure DNS forwarder and DHCP server with BOOTP/TFTP/PXE functionality.

### Installing and configuring dnsmasq

Debian/Ubuntu:

```shell
sudo apt-get install dnsmasq
```

Fedora/CentOS/RHEL:

```shell
yum install dnsmasq
```

This guide assumes you already know the network interface that will provide the DHCP/TFTP/PXE functionality for the target device. In this case, we are using _eth1_ as our secondary interface, with address _192.168.3.1_.

Following is the /etc/dnsmasq.conf providing the required functionality for PXE:

```shell
interface=eth1
dhcp-range=192.168.3.10,192.168.3.100,255.255.255.0,1h
dhcp-boot=BOOTAA64.EFI
enable-tftp
tftp-root=/srv/tftp
```

Check [http://www.thekelleys.org.uk/dnsmasq/docs/dnsmasq-man.html](http://www.thekelleys.org.uk/dnsmasq/docs/dnsmasq-man.html) for more information and additional dnsmasq config options.

Now make sure the tftp-root directory is available, and then start/restart the dnsmasq service:

```shell
sudo mkdir -p /srv/tftp
sudo systemctl restart dnsmasq
```

Since we require UEFI support for the Reference Platform Software Enterprise Edition (EE-RPB), this document doesn't cover the traditional pxelinux specific configuration (used with the traditional BIOS setup).

For UEFI, we only require DHCP to provide the UEFI binary name (retrieved via TFTP), which in this case is the Grub 2 bootloader (which then loads the kernel, initrd and other extra files from the TFTP server).
