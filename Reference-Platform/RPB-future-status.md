## Reference Platform Release Status - 16.06 

### Release Schedule

Schedule for the Core, CE Debian, CE AOSP and Enterprise deliverables:

- **Reference Kernel Platform Support Deadline:** May 6th
 - Deadline for incorporating new platforms
- **Alpha 1:** May 16th
 - Mid-cycle alpha image, mainly for QA
- **Feature Freeze:** May 31th
 - After May 31th, just bug fixes
 - Freeze Exceptions to be accepted by the 96boards team
- **Suggested RCs:** (more might be required depending on the amount of issues found and fixes provided)
 - May 31th
 - June 7th
 - June 14th
- **Final Release:** June 21th

### Release Backlog

Available at [google docs](https://docs.google.com/document/d/1utMREYtMKmC0eRM3duWCNTJ1oMNPPxMnvB_TTUOcWqg/edit) (needs Linaro account due to confidential information).

### Images

#### Release Candidate 1

- *Reference Platform Kernel:* [4.4.0-125-arm64_4.4.11.linaro.125](http://repo.linaro.org/ubuntu/linaro-staging/pool/main/l/linux/)
- **CentOS Installer (staging):** [11](https://builds.96boards.org/snapshots/reference-platform/components/centos-installer-staging/11/)
- **UEFI/EDK2:** [78](https://builds.96boards.org/snapshots/reference-platform/components/uefi/78/)
- **Debian Installer (staging):** [100](https://builds.96boards.org/snapshots/reference-platform/components/debian-installer-staging/100)
- **CE Debian RPB (staging) - HiKey:** [98](https://builds.96boards.org/snapshots/reference-platform/debian/98/hikey)
- **CE Debian RPB (staging) - DB410c:** [98](https://builds.96boards.org/snapshots/reference-platform/debian/98/dragonboard410c)

##### Known issues

- D02 is not yet functional, as UEFI-SAS support is still unstable, see bug [2286](https://bugs.linaro.org/show_bug.cgi?id=2286) for more details
- D03 fails to boot with RC1 Kernel and UEFI, check bug [2296](https://bugs.linaro.org/show_bug.cgi?id=2296) for more details.

##### Bugs per hardware platform:

- [**HiKey**](https://goo.gl/Rlu59c)
- [**Dragonboard410c**](https://goo.gl/c3K242)
- [**Cello/Overdrive**](https://goo.gl/CXTDbw)
- [**APM/HP-m400**](https://goo.gl/5hhs0l)
- [**D02**](https://goo.gl/P87Q5z)
- [**D03**](https://goo.gl/LSXpPR)

##### Out of Scope / Next Release

 - Support for Bubblegum-96 will not be part of the 16.06 release, lack of 4.4-based kernel
