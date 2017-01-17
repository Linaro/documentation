# OpenStack

This repository provides all the support code required to deploy a "Developer
Cloud".

# OpenStack packages

The OpenStack packages are built by Linaro and made available in the following
location:

TBD

The build scripts for the packages are available in this repository on the
`openstack-venvs` folder. These scripts are provided on as is basis, and they
are tailored specifically for Linaro's building environment. Use only at your
own risk.

# Reference Architecture

The reference architecture deploys a cloud that uses Ceph as backend for OpenStack.

See block diagram of how the servers should be connected to the network and how to
spread the services on the different hosts on a default configuration in the [architecture document](docs/architecture.md).

# Pre-requisites


1. All the servers are supposed to have CentOS or Debian installed and they are supposed to
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
