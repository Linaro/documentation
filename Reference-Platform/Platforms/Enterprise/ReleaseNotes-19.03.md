# 19.03 Release Notes - Linaro Enterprise Reference Platform

The goal of the Linaro Enterprise Reference Platform is to provide a fully tested, end to end, documented, open source implementation for ARM based Enterprise servers. The Reference Platform includes kernel, a community supported userspace and additional relevant open source projects, and is validated against existing firmware releases. The Linaro Enterprise Reference Platform is built and tested on Linaro Enterprise Group members hardware and the Linaro Developer Cloud. It is intended to be a reference example for use as a foundation for members and partners for their products based on open source technologies. The members and partners to include distribution, hyperscaler or OEM/ODM vendors, can leverage the reference for ARM in the datacenter.

## Reference Platform Kernel

- 5.0 stable based kernel
- Unified tree, used by both the CentOS and Debian Reference Platforms
- Focused on ACPI and UEFI use-cases.
- Single kernel config and binary (package) for all hardware platforms
- kdump implemented.

## Debian

- Network Installer based on Debian 9.8 "Stretch"
- Unified Reference Platform Kernel based on 5.0

## Enterprise Components

- 19.03 is base platform only release.

## Supported Hardware Platforms

- Cavium ThunderX
- Cavium ThunderX2
- Huawei Kunpeng 916
- Huawei Kunpeng 920
- Qualcomm Centriq 2400

## Known Issues

Currently there are no known issues in this release.

