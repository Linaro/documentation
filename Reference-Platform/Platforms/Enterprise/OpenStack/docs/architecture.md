# Network Diagram

This diagram is orientative to show how the physical networks
are expected to be set up.

The two networks are physical networks segmented between 2 different VLANS. The
internal network is a traditional lab internal network that all the servers can
see. The openstack services will communicate with each other on this
fairly safe network. Outbound traffic on this network is routed through the
external router.

The "VMS NET" is a 2nd VLAN (can be same or different physical switch).
This network is private with no outbound routes. The compute nodes and the
network node talk over this network using VXLAN to provide private virtualized
networks defined and managed by OpenStack. The network node as a single interface
bridged to the public internet and a range of public IPv4 addresses that can
be assigned as floating IPs to expose VMs to the internet.

```
+---+        +---------------------------------+        +---+
| V |        |                                 +--------+ I |
| M |        |  control-node-1                 |eth0    | N |
| S |        |  mysql, rabbit, ceph-mon        |        | T |
|   |        |                                 |        | E |
| N |        +---------------------------------+        | R +-----+
| E |                                                   | N |     |eth0
| T |        +---------------------------------+        | A |  +---------------+
|   |        |                                 +--------+ L |  |               |
|   |        | control-node-2                  |eth0    |   |  | External      |
|   |        |  keystone, glance, memcached,   |        | N |  |  router       |
|   |        |  nova(api etc), neutron-server, |        | E |  |               |
| 1 |        |  horizon, cinder, ceph-mon      |        | T |  +---------------+
| 9 |        |                                 |        | W |       |eth1
| 2 |        +---------------------------------+        | O |       |
| . |                                                   | R |       |
| 1 |        +---------------------------------+        | K |       |
| 6 |        |                                 +--------+   |       |
| 8 |        | control-node-3                  |eth0    |   |       |
| . |        |  openvswitch_agent, l3_agent,   |        |   |       |
| 0 |    eth1|  dhcp_agent, metadata_agent     |eth2    |10 |       |
| . +--------+  ceph-mon                       |__      | . |       |
| X |        +---------------------------------+  \     |10 |       |
|   |                                              \___/| . |-      |
|   |        +---------------------------------+        | X | \     \  XXXXX
|   |        |                                 +--------+ . |  \    XXXX    X
|   |        | compute-$X                      |eth0    | X |   \  XX       XX
|   |    eth1|  nova-compute, ceph-osd         |        |   |    \XX         XXX
|   +--------+  neutron-openvswitch_agent      |        |   |     X Internet   X
|   |        +---------------------------------+        |   |     XX         XXX
|   |                                                   |   |      XXXXXXXXXXX
|   |        +---------------------------------+        |   |
|   |        |                                 +--------+   |
|   |        | compute-$X                      |eth0    |   |
|   |    eth1|  nova-compute, ceph-osd         |        |   |
|   +--------+  neutron-openvswitch_agent      |        |   |
+---+        +---------------------------------+        +---+
```
