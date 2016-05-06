## Reference Platform Build - 16.03 Release - Known Issues

### Enterprise

Fixed Issues
<a href="https://bugs.linaro.org/buglist.cgi?bug_status=RESOLVED&bug_status=VERIFIED&component=Enterprise&list_id=8645&product=Reference%20Platforms&query_format=advanced&version=16.03" target="_blank">( Bugzilla )</a>

|  Enterprise Edition   | Known Issues  <a href="https://bugs.linaro.org/buglist.cgi?bug_status=CONFIRMED&bug_status=IN_PROGRESS&component=Enterprise&list_id=8646&product=Reference%20Platforms&query_format=advanced&version=16.03" target="_blank">( Bugzilla )</a> |
|:-----:|:-----|
|[bug 2079](https://bugs.linaro.org/show_bug.cgi?id=2079)| [RPB] D02- Sometimes root partition is missing when booting Debian/CentOS|
|[bug 2100](https://bugs.linaro.org/show_bug.cgi?id=2100)| [RPB] Placing D02 under major stress and SAS driver starts to have errors|
|[bug 2067](https://bugs.linaro.org/show_bug.cgi?id=2067)| [RPB] irq 5: nobody cared (try booting with the "irqpoll" option when rebooting the system|
|[bug 2068](https://bugs.linaro.org/show_bug.cgi?id=2068)| [RPB] D02- Detailed information about firmware version is needed|
|[bug 2085](https://bugs.linaro.org/show_bug.cgi?id=2085)| [RPB] D02- CentOS installer fails to detect SATA drive|
|[bug 2106](https://bugs.linaro.org/show_bug.cgi?id=2206)| [RPB] D02- shutdown works as reboot|
|[bug 2097](https://bugs.linaro.org/show_bug.cgi?id=2097)| [RPB] kernel fails to build on amd64|
|[bug 2069](https://bugs.linaro.org/show_bug.cgi?id=2069)| [RPB] D02- Selected item in BIOS is not highlighted in minicom|
|[bug 2086](https://bugs.linaro.org/show_bug.cgi?id=2086)| [RPB] D02: Booting CentOS Linux failed|
|[bug 2066](https://bugs.linaro.org/show_bug.cgi?id=2066)| QEMU can't launch an instance with more than 30GB RAM|
|[bug 2075](https://bugs.linaro.org/show_bug.cgi?id=2075)| [RPB] D02: Latest EDK2 breaks network support in UEFI|

***

### HiKey

Fixed Issues <a href="https://bugs.96boards.org/buglist.cgi?bug_status=RESOLVED&bug_status=VERIFIED&classification=Consumer%20Edition%20Boards&list_id=1613&product=HiKey&query_format=advanced&target_milestone=Reference%20Software%20Platform%20-%2016.03" target="_blank">( Bugzilla )</a>

| Debian    | Known Issues  <a href="https://bugs.96boards.org/buglist.cgi?bug_status=CONFIRMED&bug_status=IN_PROGRESS&classification=Consumer%20Edition%20Boards&component=ARM%20Trusted%20Firmware&component=Debian&component=default&component=Documentation&component=Graphics&component=Linux%20Kernel&component=OPTEE&component=U-Boot&component=UEFI&component=USB%20Tools&component=Utilities&component=WIFI&list_id=1615&product=HiKey&query_format=advanced&version=RPB%2015.12&version=RPB%2016.03&version=RPB%2016.06" target="_blank">( Bugzilla )</a>   |
|:-----:|:-----|
|[bug 187](https://bugs.96boards.org/show_bug.cgi?id=187)| Missing XWindows video acceleration - Weston (needs Mali r6p0)|
|[bug 262](https://bugs.96boards.org/show_bug.cgi?id=262)| [RPB] LG W2253V fails to work with 4.4.0-93-arm64|
|[bug 212](https://bugs.96boards.org/show_bug.cgi?id=212)| Suspend/resume support needed in 4.1/4.4|
[bug 223](https://bugs.96boards.org/show_bug.cgi?id=223)| **HiKey**: Linux 4.4: USB unstable with SMP|
|[bug 27](https://bugs.96boards.org/show_bug.cgi?id=27)| UEFI variable runtime service not working|
|[bug 222](https://bugs.96boards.org/show_bug.cgi?id=222)| **HiKey**: RTC RTS code accesses unrelocated address|
|[bug 267](https://bugs.96boards.org/show_bug.cgi?id=267)| [RPB] UEFI does not provide devicetree to OS|
|[bug 290](https://bugs.96boards.org/show_bug.cgi?id=290)| [RPB] fastboot erase/flash system is just too slow when flashing the Debian images|
|[bug 176](https://bugs.96boards.org/show_bug.cgi?id=176)| Upgrade HiKey Mali Lib to r6p0|
|[bug 205](https://bugs.96boards.org/show_bug.cgi?id=205)| [RPB] USB OTG fails after hot removal and reinsertion|
|[bug 286](https://bugs.96boards.org/show_bug.cgi?id=286)| [RPB] 4.4.0-102-arm64 - Bad mode in Synchronous Abort handler detected|
|[bug 20](https://bugs.96boards.org/show_bug.cgi?id=20)| [RPB] USB kernel trace errors -22|
|[bug 152](https://bugs.96boards.org/show_bug.cgi?id=152)| [RPB] SD-Card doesn't work|
|[bug 163](https://bugs.96boards.org/show_bug.cgi?id=163)| [RPB-AOSP] HDMI audio not working|
|[bug 233](https://bugs.96boards.org/show_bug.cgi?id=233)| [RPB] Bluetooth driver prevents board from rebooting|
|[bug 265](https://bugs.96boards.org/show_bug.cgi?id=265)| fastboot reboot-bootloader doesn't work|
|[bug 291](https://bugs.96boards.org/show_bug.cgi?id=291)| [RPB] fastboot erase not supported in l-loader (recovery)|
|[bug 282](https://bugs.96boards.org/show_bug.cgi?id=282)| [RPB] Missing wl18xx wlconf setup as part of the first boot process|
|[bug 145](https://bugs.96boards.org/show_bug.cgi?id=145)| [RPB] unable to read thermal sensors|
|[bug 151](https://bugs.96boards.org/show_bug.cgi?id=151)| [RPB] glxgears: couldn't get an RGB, Double-buffered visual|

| AOSP     | Known Issues   <a href="https://bugs.96boards.org/buglist.cgi?bug_status=CONFIRMED&bug_status=IN_PROGRESS&classification=Consumer%20Edition%20Boards&component=AOSP&list_id=1617&product=HiKey&query_format=advanced&version=RPB%2015.12&version=RPB%2016.03&version=RPB%2016.06" target="_blank">( Bugzilla )</a>  |
|:-----:|:------|
|[bug 180](https://bugs.96boards.org/show_bug.cgi?id=180)| [RPB] Shutdown cannot turn off HDMI monitor|
|[bug 224](https://bugs.96boards.org/show_bug.cgi?id=224)| [RPB-AOSP] BT status LED doesn't blink when BT transfer is in progress|
|[bug 225](https://bugs.96boards.org/show_bug.cgi?id=225)| [RPB] User LED numbers on the board don't match the sysfs entries|
|[bug 228](https://bugs.96boards.org/show_bug.cgi?id=228)| [RPB] Bluetooth mice pair and connect but don't show input|

***

### DragonBoard™ 410c

Fixed Issues
<a href="https://bugs.96boards.org/buglist.cgi?bug_status=RESOLVED&bug_status=VERIFIED&classification=Consumer%20Edition%20Boards&component=Android&component=Bootloader%20%2F%20Firmware&component=Documentation&component=Kernel&component=OpenEmbedded%20%2F%20Yocto&component=Tools%20%2F%20Installer&component=Ubuntu%20%2F%20Debian&list_id=1623&product=Dragonboard%20410c&query_format=advanced&resolution=---&resolution=FIXED&resolution=INVALID&resolution=WONTFIX&resolution=WORKSFORME&resolution=NON%20REPRODUCIBLE&version=RPB%2016.03" target="_blank">( Bugzilla )</a>

| Debian | Known Issues <a href="https://bugs.96boards.org/buglist.cgi?bug_status=CONFIRMED&bug_status=IN_PROGRESS&classification=Consumer%20Edition%20Boards&component=Android&component=Bootloader%20%2F%20Firmware&component=Documentation&component=Kernel&component=OpenEmbedded%20%2F%20Yocto&component=Tools%20%2F%20Installer&component=Ubuntu%20%2F%20Debian&list_id=1620&product=Dragonboard%20410c&query_format=advanced&resolution=---&version=RPB%2015.12&version=RPB%2016.03" target="_blank">( Bugzilla )</a>|
|:-------:|:---------|
| [bug 285](https://bugs.96boards.org/show_bug.cgi?id=285) | USB host doesn't detect any plugged devices |
| [bug 121](https://bugs.96boards.org/show_bug.cgi?id=121) | [RPB] Cannot soft power off or shutdown db410c |
| [bug 284](https://bugs.96boards.org/show_bug.cgi?id=284) | [RPB] Dragon board Display sleep not working |
| [bug 289](https://bugs.96boards.org/show_bug.cgi?id=289) | [RPB] USB devices don't work after reboot |
| [bug 207](https://bugs.96boards.org/show_bug.cgi?id=207) | [RPB] Bluetooth does not work on Dragon board debian |
| [bug 153](https://bugs.96boards.org/show_bug.cgi?id=153) | [RPB] Missing information about hwpack usage|


Fixed Issues
<a href="https://bugs.96boards.org/buglist.cgi?bug_status=RESOLVED&bug_status=VERIFIED&classification=Consumer%20Edition%20Boards&component=AOSP&list_id=1621&product=Dragonboard%20410c&query_format=advanced&version=RPB%2016.03" target="_blank">( Bugzilla )</a>


| AOSP | Known Issues <a href="https://bugs.96boards.org/buglist.cgi?bug_status=CONFIRMED&bug_status=IN_PROGRESS&classification=Consumer%20Edition%20Boards&component=AOSP&list_id=1619&product=Dragonboard%20410c&query_format=advanced&resolution=---&version=RPB%2015.12&version=RPB%2016.03" target="_blank">( Bugzilla )</a> |
|:----------:|:-----------|
| [bug 254](https://bugs.96boards.org/show_bug.cgi?id=254) |  [RPB] wpa_supplicant crashes wcn36xx |
| [bug 276](https://bugs.96boards.org/show_bug.cgi?id=276) | [RPB-AOSP] USB-OTG doesn't work |
| [bug 278](https://bugs.96boards.org/show_bug.cgi?id=278) | [RPB-AOSP] Free internal disk space is too small |
| [bug 279](https://bugs.96boards.org/show_bug.cgi?id=279) | [RPB-AOSP] "x App has stopped" happens frequently |
| [bug 280](https://bugs.96boards.org/show_bug.cgi?id=280) | [RPB-AOSP] App crashes when SD card mounted manually |
| [bug 277](https://bugs.96boards.org/show_bug.cgi?id=277) | [RPB-AOSP] SD card auto mount from UI doesn't work |


***



| Bug Legend   |        |
|:-----:|:-------|
| CONFIRMED      | If a bug can be reproduced, a member from the 96Boards, Linaro or QA team will change its status from **UNCONFIRMED** to **CONFIRMED** |
| IN_PROGRESS    |  This bug is currently being worked on by either the 96Boards, Linaro, or QA team    |
|   RESOLVED  | Development is finished with a bug. Please [click here](https://wiki.documentfoundation.org/QA/Bugzilla/Fields/Status/RESOLVED) for information on sub-states  |
| VERIFIED | A team has VERIFIED a working solution for a bug |

***
