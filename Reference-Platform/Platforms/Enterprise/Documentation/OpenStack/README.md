# OpenStack

This repository provides all the support code required to deploy a "Developer
Cloud".

# OpenStack packages

The OpenStack packages are built by Linaro and made available in the following
location:

https://repo.linaro.org/debian/erp-17.08-stable/

The build scripts for the packages are available in this repository on the
[`openstack-venvs`](https://git.linaro.org/leg/sdi/openstack-ref-architecture.git/tree/openstack-venvs) folder. These scripts are provided on as is basis, and they are tailored specifically for Linaro's building environment. Use only at your
own risk.

# Reference Architecture

The reference architecture deploys a cloud that uses Ceph as backend for OpenStack:

[https://git.linaro.org/leg/sdi/openstack-ref-architecture.git](https://git.linaro.org/leg/sdi/openstack-ref-architecture.git)

See block diagram of how the servers should be connected to the network and how to
spread the services on the different hosts on a default configuration in the [architecture document](docs/architecture.md).

# Pre-requisites


1. All the servers are supposed to have Linaro ERP 16.12 installed and they are supposed to
have networking configured in a way that they can see/resolve each other's names.

1. The nodes that will be used as Ceph OSDs need to have at least one extra harddrive for Ceph.

1. The networking node should have 3 NICs connected as described in the [architecture document](docs/architecture.md).

# Configuration

Some example configuration files are provided in this repo as example, go through them and
generate the equivalent ones for your particular deployment:

    ansible/hosts.example
    group_vars/all

Ensure to use host names, instead of ips to avoid some known deployment issues.

The playbook assumes your own files are in a folder called `ansible/secrets`, so the recommendation
is to place your files there.

# The deployment

The deployment can be split in two different parts. Ceph and OpenStack.

## Deploying Ceph

1) Monitors are deployed and the cluster bootstrapmd:

    ansible-playbook -K -v -i ./hosts ./site.yml --tags ceph-mon

Check that the cluster is up and running by connecting to one of the monitors
and checking:

    ssh server1
    ceph daemon mon.server1 mon_status

2) OSDs assume a full hard drive is dedicated to Ceph at least. A default
configuration if all the servers that will be OSDs have the same HD layout
can be spedified in group_vars/all as follows:

```
ceph_host_osds:
  - sbX
  - sbY
  - sbZ
```

If some server has a different configuration, this will be specified in the
hostvars folder, in a file with the name of your server. For example:

```
$ cat hostvars/server1

ceph_host_osds:
  - sbZ
  - sbY
```

After configuring, the OSDs are deployed as follows:

  ansible-playbook -K -v -i ./secrets/hosts ./site.yml --tags ceph-osd

2.1) In the case of setting up a cluster from scratch where ceph has been installed
previously, there is an option to force the resetting of all the disks (this
option WILL DELETE all the data on the OSDs). This option is not
idempotent, use at your own risk. It is safe to use if you have cleanly deployed
the machine and the disk to be used as OSD had a previously installed Ceph:

    --extra-vars 'ceph_force_prepare=true'

## Deploying OpenStack

OpenStack is deployed using Ansible with the playbook defined in the "ansible"
directory. You'll need to create the files "deployment-vars" and "hosts" to
match your environment. There are examples to help guide you. Once those files
are in place, OpenStack can be deployed with:

    ansible-playbook -K -v -i secrets/hosts site.yml

### Deploying Swift and Ceph RADOS Gateway

OpenStack Swift is also deployed using Ansible on ONE host only.

1) At least one dedicated partition is required. Partitions can be specified in
group_vars/all as follows:

```
swift_partition:
  - sdX1
  - sdX2
  - sdY1
  - sdY2
```

If some server has a different configuration, this will be specified in the
hostvars folder, in a file with the name of your server. For example:

```
$ cat hostvars/server1

swift_partition:
  - sdZ1
```

Please note the deployment scripts won't partition the hard drive. The partitions must
be created before deployment.

2) Option 'swift_force_prepare' can be used to force resetting of all related
partitions (note this option WILL DELETE all the data on the swift partition).

    --extra-vars 'swift_force_prepare=true'

3) Object storage can be deployed by using either Swift or Ceph (RADOS gateway).
This choice is controlled by setting the variable 'use_ceph_rgw' on the
group_vars/all file. The default value for 'use_ceph_rgw' is false, which means
deploying Swift is the default option.

Add 'use_ceph_rgw = True' to your deployment-vars file to enable RADOS gateway
API. On a deployed system, change this option and re-deploy swift will switch
according endpoints to required API.
