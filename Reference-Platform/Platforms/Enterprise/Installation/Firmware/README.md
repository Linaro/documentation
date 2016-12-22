<table align="center">
<tr>
    <td align="center">UEFI/EDK2<br><a href="../README.md">Go Back</a></td>
    <td align="center"><a href="Download.md">Download</a><br>Get the latest pre-built firmware images</td>
    <td align="center"><a href="Build.md">Build</a><br>Instructions for building latest firmware images</td>
    <td align="center"><a href="Install.md">Install</a><br>Instructions on how to install firmware</td>
    <th align="center"><a href="README.md">Read more</a><br>Learn more about UEFI/EDK2</td>
</tr>
</table>

EDK2 is a modern, feature-rich, cross-platform firmware development environment for the UEFI and PI specifications.

The reference UEFI/EDK2 tree used by the EE-RPB comes directly from [upstream](https://github.com/tianocore/edk2), based on a specific commit that gets validated and published as part of the Linaro EDK2 effort (which is available at [https://git.linaro.org/uefi/linaro-edk2.git](https://git.linaro.org/uefi/linaro-edk2.git)).

Since there is no hardware specific support as part of EDK2 upstream, an external module called [OpenPlatformPkg](https://git.linaro.org/uefi/OpenPlatformPkg.git) is also required as part of the build process.

EDK2 is currently used by 96boards LeMaker Cello, AMD Overdrive, ARM Juno r0/r1/r2, HiSilicon D02 and HiSilicon D03.

This guide provides enough information on how to build UEFI/EDK2 from scratch, but meant to be a quick guide. For further information please also check the official Linaro UEFI documentation, available at [https://wiki.linaro.org/ARM/UEFI](https://wiki.linaro.org/ARM/UEFI) and  [https://wiki.linaro.org/LEG/Engineering/Kernel/UEFI/build](https://wiki.linaro.org/LEG/Engineering/Kernel/UEFI/build)
