# Builds

[**Member Build:**](http://builds.96boards.org/releases/dragonboard410c/qualcomm/android/) Created and maintained by the SoC and/or board vendor. Member builds typically use a vendor BSP and are intended to enable all the features and functionality of a particular SoC. Linaro only hosts these builds on the 96Boards website and has no involvement in the creation of these builds. These builds are the ideal starting point for all users of a platform and are usually the most stable. Users who need more up to date software versions can try out the reference platform build for the platform (if available) while those who want to track other work happening inside Linaro can choose a Linaro Engineering Build for the platform (if available).

[**Linaro Engineering Build:**](http://builds.96boards.org/releases/) Typically created and maintained by Linaro with help from the SoC vendor. Linaro creates and manages these builds which might include some Linaro sauce applied to the vendor BSP. Examples of such sauce might include using new toolchain optimizations, incorporating new technology being developed in the working groups or provided QA and benchmarking services around the build.

[**Reference Platform Build:**](http://builds.96boards.org/releases/reference-platform/) Created and maintained by Linaro, these builds target the open source community by shipping the freshest possible versions of software across the entire software stack - firmware, bootloader, kernel, middleware and applications. Reference Platform Builds are intended to be fully functional, though they might lack features that require binary blobs which are tied to a specific version of the kernel. Linaro engineering tries to keep  these builds on the latest possible kernel. One key objective of this build is to unify the kernel and bootloader for all supported platforms so that a single kernel binary works across all these platforms. These builds are ideal for engineers working on the evolution of the system software ecosystem.

[**Community Build:**]() Created and maintained by the community. These builds are typically aimed at enabling new distributions on the HW besides the ones supported through the Member and Linaro engineering builds. Linaro only hosts or links to these builds on the 96Boards website and typically has no involvement in the creation of these builds other than providing technical assistance in it’s enablement.

***

On a scale of 1-5, how well supported a feature is, with 5 being full support

| Feature | Memeber Build | Linaro Engineering Build | Reference Platform Build | Community Build | LCR cariants |
|:-------|:----------|:----------|:---------|:---------|:------------|
| Expose full HW functionallity | 5 | 5 | 3 | 3 | 5? |
| Periodic update | 3 | 5 | 5 | ? | ? |
| Latest kernel/bootloader | 3 | 3 | 5 | ? | ? |
| Latest distro packages | 2 | ? | 5 | ? | ? |


