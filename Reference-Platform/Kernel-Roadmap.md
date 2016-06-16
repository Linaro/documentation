## Introduction
The Reference Platform Kernel (RPK) brings together WIP code that is still under review upstream in case that is useful.

The kernel tree is managed similar to linux-next, in that topic branches adding support for various platforms and new kernel features are merged on top of a (close-to-mainline) vanilla kernel. These topic branches are provided by the relevant segment group, Landing Team or vendor engineers who want to add support for a hardware platform or a new feature into RPK. Please review the [[patch-acceptance policy|RP-Kernel-Policy]] for RPK. It is implicit that the person responsible for the feature/platform suppport will rebase it to the new kernel version if that feature is not to be dropped in subsequent kernel releases.

See the [table](#kernel-version-table) below for a roadmap of proposed kernel versions for future releases.

## Kernel Version Table
| RPB Release | Kernel Version | LEG | LHG | LNG | LMG |
|---|---|---|---|---|---|
|16.03   |4.4   | Y | - | - | - |
|16.06   |4.4   | Y | Y | Y | ? |
|16.09   |4.6?  | S | ? | ? | ? |
|16.12   |4.8?  | Y | ? | ? | ? |

     - Not supported
     ? Undecided
     S Skipped (e.g. LEG follows a 6-month cycle)
     Y Supported (features and platforms)

## Hardware Platforms supported in each version
| RPB Release | Kernel Version | Hikey | DB410c | D02 | Overdrive | APM X-Gene | HP Proliant m400 |
|---|---|---|---|---|---|---|---|
|16.03   |4.4   | Y | Y | Y | Y | Y | Y |
|16.06   |4.4   | Y | Y | Y | Y | Y | Y |

## Development Branches
FIXME

## FAQ
 1. How is this different from linux-linaro?  
    RPK has a lot of similarities to linux-linaro. However, RPK focuses on code that is being actively reviewed upstream. RPK will drop any code that shows no progress upstream.
 1. Will you support an LTS kernel for 'X' years?  
    No. RPK's main focus is on engineers and teams that need to get their code upstream as a requirement to get distribution support (e.g. LEG features enabled in RHEL, Ubuntu) or that need to work on tip to get new features accepted into the kernel (e.g. core engineering teams such at KWG, PMWG). We don't have resources to maintain a long-term kernel. Please talk to the LSK team for long-term supported kernels.

## Additional Material
 * [Talk](https://www.youtube.com/watch?v=fW6_eL3U7OQ) about RPK at BKK16 (March 2016)
 * [Patch-acceptance Policy](KernelPolicy.md) for RPK

## Communications
 * [Dev](https://lists.96boards.org/mailman/listinfo) mailing list
