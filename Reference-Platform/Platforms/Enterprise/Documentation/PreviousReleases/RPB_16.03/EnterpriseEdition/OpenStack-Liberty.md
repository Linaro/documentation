# OpenStack Liberty - Debian Jessie

# Introduction

In general, the instructions in the Liberty install guide should be followed: http://docs.openstack.org/liberty/install-guide-ubuntu/overview.html.  This guide will describe changes to the documented procedures that should be kept in mind while going through the guide.

Each section below will correspond to a section in the guide.  Guide sections that do not have a corresponding section below may be followed as-is.

# Release Notes

## Configuring images for aarch64

An image must be configured specially in glance to be able to boot correctly on aarch64.  
To attach the devices to the virtio bus (which does not allow hotplugging a volume, but will work if the image does not have SCSI support), the following properties must be set:

```shell
--property hw_machine_type=virt
--property os_command_line='root=/dev/vda rw rootwait console=ttyAMA0'
--property hw_cdrom_bus=virtio
```

To attach the devices to the SCSI bus (which does allow hotplugging a volume, but might not be supported by the guest image), the following properties must be set:

```shell
--property hw_scsi_model='virtio-scsi'
--property hw_disk_bus='scsi'
--property os_command_line='root=/dev/sda rw rootwait console=ttyAMA0'
```

You can set these properties when you are uploading the image into glance, or modify the image if you have already uploaded it.


# Pre-Installation

## Verify/enable additional repositories

Verify that the `linaro-overlay` and `jessie-backports` repositories are enabled.

Check if they are available by checking `/etc/apt/sources.list` and `/etc/apt/sources.list.d`.

If missing, add the following to `/etc/apt/sources.list.d` directory:

```shell
$ echo "deb http://repo.linaro.org/ubuntu/linaro-overlay jessie main" | sudo tee /etc/apt/sources.list.d/linaro-overlay-repo.list
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E13D88F7E3C1D56C
```

If missing, add the following to `/etc/apt/sources.list.d` directory:

```shell
$ echo "deb http://httpredir.debian.org/debian jessie-backports main" | sudo tee /etc/apt/sources.list.d/jessie-backports.list
```

## Modify repository priorities

Create `/etc/apt/preferences.d/jessie-backports`:

```shell
Package: *
Pin: release a=jessie-backports
Pin-Priority: 500
```

Then, make sure to run apt-get update:

```shell
$ sudo apt-get update
```

## Environment

Update `/etc/hosts` to add “controller” as an alias for localhost.

```shell
127.0.0.1       localhost controller
```

## Disable IPV6

Add the following to `/etc/sysctl.conf`:

```shell
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
net.ipv6.conf.eth0.disable_ipv6 = 1
```

Run sysctl to apply the changes:

```shell
$ sudo sysctl -p
```

# Following the Openstack guide...

OpenStack guide: http://docs.openstack.org/liberty/install-guide-ubuntu/overview.html

## Environment

### Openstack Packages

Do not enable the `cloud-archive:liberty` repository.

Install some dependencies:

```shell
$ sudo apt-get install openstack-cloud-services python-pymysql
```

Answer the questions asked by debconf:

* New password for the MySQL **root** user: \<enter a password -- possibly "root">

Install the openstack client:

```shell
$ sudo apt-get install python-openstackclient
```

### NoSQL Database

The instructions in this section are not required, as Telemetry is not installed.

## Add the Identity service (Keystone)

Follow the Openstack guide with the exception of the following changes documented here.

### Install and configure

#### Prerequisites

Omit this section of the guide.  These operations will be done during meta package installation later.

#### Install and configure components

Install the apache and the keystone meta package:

```shell
$ sudo apt-get install openstack-cloud-identity
```

Answer the questions asked by debconf:

* Set up a database for Keystone: **Yes**
* Configure database for keystone with dbconfig-common: **Yes**
* Database type to be used by keystone: **mysql**
* Password of the database's administrative user: **\<use the password you used during database install>**
* MySQL application password for keystone: **\<enter a password>**
* Authentication server administration token: **\<enter a token value>**
* Register administration tenants? **Yes**
* Password of the administrative user: **\<enter a password>**
* Register Keystone endpoint? **Yes**
* Keystone endpoint IP address: **\<use default>**

#### Configure the Apache HTTP server

Omit this section of the guide.

#### Finalize the installation

Omit this section of the guide.

### Create the service entity and API endpoints

Omit this section of the guide.

### Create projects, users, and roles

Omit this section of the guide.


## Add the Image service (Glance)

Follow the Openstack guide with the exception of the following changes documented here.

### Install and configure

#### Prerequisites

Omit this section of the guide.  These operations will be done during package installation later.

#### Install and configure components

```shell
$ sudo apt-get install glance
```

Answer the questions asked by debconf:

* Set up a database for Glance: **Yes**
* Configure database for glance-common with dbconfig-common? **Yes**
* Database type to be used by glance-common: **mysql**
* Password of the database's administrative user: **\<enter a password>**
* MySQL application password for glance-common: **\<enter a password>**
* IP address of your RabbitMQ host: **\<use default, or localhost, or controller>**
* Username for connection to the RabbitMQ server: **guest**
* Password for connection to the RabbitMQ server: **guest**
* Pipeline flavor: **keystone**
* Authentication server hostname: **\<use default, or localhost, or controller>**
* Authentication server password: **\<enter a password>**
* Register Glance in the Keystone endpoint catalog? **Yes**
* Keystone authentication token: **\<enter the keystone token>**

#### Finalize installation

Omit this section of the guide.

### Verify operation

The CirrOS image to run on aarch64 is the file that ends in `-uec.tar.gz`.  It must be extracted and each file (kernel, initrd, disk image) uploaded to Glance separately.

Download the CirrOS AArch64 UEC tarball and untar it:

```shell
$ wget http://download.cirros-cloud.net/daily/20150923/cirros-d150923-aarch64-uec.tar.gz
$ tar xvf cirros-d150923-aarch64-uec.tar.gz
```

Upload the image parts into Glance.  You will need to make note of the IDs assigned to the kernel and initrd and pass them on the command line when uploading the disk image:

```shell
$ glance image-create --name "cirros-kernel" --visibility public --progress \
  --container-format aki --disk-format aki --file cirros-d150923-aarch64-vmlinuz

$ glance image-create --name "cirros-initrd" --visibility public --progress \
  --container-format ari --disk-format ari --file cirros-d150923-aarch64-initrd

$ glance image-create --name "cirros" --visibility public --progress \
  --property hw_machine_type=virt --property hw_cdrom_bus=virtio \
   -property os_command_line='console=ttyAMA0' \
  --property kernel_id=KERNEL_ID --property ramdisk_id=INITRD_ID \
  --container-format ami --disk-format ami --file cirros-d150923-aarch64-blank.img
```

## Add the Compute service (Nova)

Follow the Openstack guide with the exception of the following changes documented here.

### Install and configure

#### Prerequisites

Omit this section of the guide.  These operations will be done during package installation later.

#### Install and configure components

```shell
$ sudo apt-get install nova-api nova-cert nova-conductor \
  nova-consoleauth nova-scheduler nova-compute
```

Answer the questions asked by debconf:

* Set up a database for Nova: **Yes**
* Configure database for nova-common with dbconfig-common? **Yes**
* Database type to be used by nova-common: **mysql**
* Password of the database's administrative user: **\<enter a password>**
* MySQL application password for nova-common: **\<enter a password>**
* IP address of your RabbitMQ host: **\<use default, or localhost, or controller>**
* Username for connection to the RabbitMQ server: **guest**
* Password for connection to the RabbitMQ server: **guest**
* Auth server hostname: **\<use default, or localhost, or controller>**
* Auth server password: **\<enter a password>**
* Neutron server URL: **http://\<use default, or localhost, or controller>:9696**
* Neutron administrator password: **\<enter a password>**
* Metadata proxy shared secret: **\<enter a shared secret string>**
* API to activate: choose **osapi_compute and metadata**
* Value for my_ip: **\<default>**
* Register Nova in the Keystone endpoint catalog? **Yes**
* Keystone authentication token: **\<enter the keystone token>**

#### Finalize installation

Ensure that vnc and spice are disabled in `/etc/nova/nova.conf`.  Look for the following keys in `nova.conf` and set them to False:

```shell
vnc_enabled=false

[spice]
enabled=false
```

Enable KVM by ensuring the following is in `nova-compute.conf`:

```shell
[DEFAULT]
compute_driver=libvirt.LibvirtDriver

[libvirt]
virt_type=kvm
```

**NOTE: Until kernel support for KVM is properly enabled, instances can be run in emulation by ensuring the following is in `nova-compute.conf`**:

```shell
[DEFAULT]
compute_driver=libvirt.LibvirtDriver

[libvirt]
cpu_mode = custom
virt_type = qemu
cpu_model = cortex-a57
```

**IMPORTANT: If you make changes to `nova.conf`, or `nova-compute.conf`, restart the nova services:**

```shell
$ sudo service nova-compute restart
```


## Add the Networking service (Neutron)

Follow the Openstack guide with the exception of the following changes documented here.

### Install and configure

#### Prerequisites

Omit this section of the guide.  These operations will be done during package installation later.

#### Install and configure components

```shell
$ sudo apt-get install neutron-server neutron-plugin-ml2 \
  neutron-plugin-linuxbridge-agent neutron-dhcp-agent \
  neutron-metadata-agent
```

Answer the questions asked by debconf:

* neutron-common
  * Set up a database for Neutron: **Yes**
  * Configure database for neutron-common with dbconfig-common? **Yes**
  * Database type to be used by neutron-common: **mysql**
  * Password of the database's administrative user: **\<enter a password>**
  * MySQL application password for neutron-common: **\<enter a password>**
  * IP address of your RabbitMQ host: **\<use default, or localhost, or controller>**
  * Username for connection to the RabbitMQ server: **guest**
  * Password for connection to the RabbitMQ server: **guest**
  * Authentication server hostname: **\<use default, or localhost, or controller>**
  * Authentication server password: **\<enter a password>**
  * Neutron plugin: **ml2**
* neutron-metadata-agent
  * Auth server hostname: **\<use default, or localhost, or controller>**
  * Auth server password: **\<enter a password>**
  * Name of the region to be used by the metadata server: **\<default>**
  * Metadata proxy shared secret: **\<enter the shared secret string entered for Nova>**
* neutron-server
  * Register Neutron in the Keystone endpoint catalog? **Yes**
  * Keystone authentication token: **\<enter the keystone token>**

#### Configure networking options
Follow "Networking Option 1: Provider networks".

#### Finalize installation

Omit this section of the guide.


## Launch an instance

### Create virtual networks

Follow section “Public provider network”

### Launch an instance 

Follow section “Launch an instance on the public network”

NOTE: Accessing an image via the virtual console (VNC) will not work, as VNC is not supported.  You may access the console log using the following command:

```shell
$ nova console-log --length=10 INSTANCE_ID
```
