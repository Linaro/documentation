## Reference Software Platform - CI

List of current CI jobs ([ci.linaro.org](https://ci.linaro.org/)) which are maintained as part of the Reference Software Platform lead project.


- [Reference GRUB](https://ci.linaro.org/view/96boards/job/96boards-reference-grub-efi-arm64/)
   - Created so we can start moving the grub logic into a more desktop-standard behavior
   - Grub built to search for the root file system via labels, and loads kernel, initrd and grub.conf from the rootfs (system partition)

- [Reference UEFI](https://ci.linaro.org/view/96boards/job/96boards-reference-uefi/)
   - Created to avoid conflicts and issues with the branch and build job used by the hikey member images
   - Useful for upstreaming work and early bring up
   - Later on this job might not be required, and simply migrated to the Linaro EDK2 tree

- [Reference Kernel](https://ci.linaro.org/view/96boards/job/96boards-reference-kernel/)
   - Consumes and builds the common kernel tree that is maintained by Amit
   - Not yet used by any of the builds

- [Reference Kernel - Matrix](https://ci.linaro.org/view/96boards/job/96boards-reference-kernel-matrix/)
   - Similar to the main reference kernel job, but uses trees/branches that are based from the landing team trees
   - To be used until we can get the reference tree in place
   - Job responsible for producing the kernel used by the Debian RPBs

- [Reference Kernel - Enterprise](https://ci.linaro.org/view/96boards/job/96boards-reference-kernel-enterprise/)
   - Consumes and build the common kernel tree for Enterprise, maintained by Amit
   - Job responsible for producing the kernel used by the EE RPBs


- [Reference Build - AOSP](https://ci.linaro.org/view/96boards/job/96boards-reference-platform-aosp/)
   - Same tree and job previously done by Vishal
   - To be migrated to a 4.1 based kernel, once ready
   - Dragonboard™ 410c not yet supported

- [Reference Build - Debian](https://ci.linaro.org/view/96boards/job/96boards-reference-platform-debian/)
   - Using the reference components (grub, uefi, kernel)
   - Matrix job that will be extended to support additional boards
   - Rootfs is shared with the other builds, effort to make the changes generic enough to be used by others

- [Reference Build - OpenEmbedded](https://ci.linaro.org/view/96boards/job/96boards-reference-platform-openembedded/)
   - Still a matrix job, not yet building a single rootfs

Every artifact produced by the builds described in this page can be found at [https://builds.96boards.org/snapshots/reference-platform/](https://builds.96boards.org/snapshots/reference-platform/).
