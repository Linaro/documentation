## EE Reference Platform Release Status

### EE Release Schedule

| Date          | Checkpoint |
| ------------- | ------------- |
| 2016-07-04    | New release cycle starting      |
| 2016-07-18    | Release goals proposal deadline |
| 2016-0x-xx    | Alpha                           |
| 2016-0x-xx    | Beta                            |
| 2016-0x-xx    | RC                              |
| 2016-0x-xx    | Final freeze                    |
| 2016-12-xx    | Release                         |

**TBA:**
* upstream projects key dates and events
  * v4.8 kernel predictions: merge window closes on 2016-08-07 and release on 2016-09-25
  * v4.9 kernel predictions: merge window closes on 2016-10-09 and release on 2016-11-27
  * Linaro Connect Las Vegas, from 2016-09-26 to 2016-09-30
  * OP-TEE 2.2.0, around 2016-10-15
* QA checkpoints
  * Alpha: feature complete, no blocker bugs
  * Beta: code freeze, no blocker bugs, no critical bugs (only bugfixes are accepted after this point)
  * RC: no critical bugs, possibly no major bugs

### EE Release Goals

This section lists the current release goals for EE Reference Platform 16.12 and additional candidates which may become a release goal in the future.

The criteria for release goals are described below:
* SMART (Specific, Measurable, Attainable, Realistic, Timely)
* Have an 'advocate/owner', who can track and keep status of progress
* Be generally consensual

The template to propose entries is describerd below:
```
* **Summary:** one-liner summary
* **Description:** one-liner short description with more info
* **Advocate/Owner:** Firstname Lastname, ...
* **State:** {proposed,accepted,rejected,completed}
* **Bug(s):** https://...
```

Note: if it's relevant, add tags in the summary. e.g. [Debian] [CentOS]

#### Proposed Goals

* **Summary:** update kernel to 4.9
* **Description:** update kernel to 4.9, 4.9-rcX will be used if required.
* **Advocate/Owner:** Graeme Gregory
* **State:** proposed
* **Bug(s):** TBA
<br />
<br />
* **Summary:** use Tianocore and OpenPlatformPkg for D02, D03, Overdrive and Cello boards
* **Description:** use Tianocore and OpenPlatformPkg for D02, D03, Overdrive and Cello boards
* **Advocate/Owner:** Leif Lindholm
* **State:** proposed
* **Bug(s):** TBA
<br />
<br />
* **Summary:** release OpenStack Newton
* **Description:** release OpenStack Newton
* **Advocate/Owner:** Gema Gomez
* **State:** proposed
* **Bug(s):** TBA
<br />
<br />
* **Summary:** release Ceph Jewel
* **Description:** release Ceph Jewel
* **Advocate/Owner:** Gema Gomez
* **State:** proposed
* **Bug(s):** TBA
<br />
<br />
* **Summary:** release EE RPB Debian/CentOS cloud images
* **Description:** release EE RPB Debian/CentOS cloud images
* **Advocate/Owner:** Marcin Juszkiewicz
* **State:** proposed
* **Bug(s):** TBA
<br />
<br />
* **Summary:** use config fragments for RP Kernel
* **Description:** use config fragments for RP Kernel
* **Advocate/Owner:** TBA
* **State:** proposed
* **Bug(s):** TBA
<br />
<br />
* **Summary:** Debian/CentOS installers support in LAVA
* **Description:** LAVA supports Debian/CentOS installers
* **Advocate/Owner:** Milosz Wasilewski
* **State:** proposed
* **Bug(s):** TBA
<br />
<br />
* **Summary:** Improve Colo/lab handling of PXE servers
* **Description:** Reserve 2 systems in their own subnet and allow PXE server configuration to users
* **Advocate/Owner:** Andy Doan
* **State:** proposed
* **Bug(s):** TBA
<br />
<br />

#### Accepted Goals

NONE atm.

#### Rejected Goals

NONE atm.

#### Completed Goals

NONE atm.

### EE Release Hardware Platforms

Tested hardware platforms are splitted into 2 categories: Tier 1 (Reference Hardware Platforms) and Tier 2 (Optional Test Hardware Platforms).

**Note:** Serious bugs on Tier 1 platforms are release blockers, while they will be listed as known issues on Tier 2 platforms.

#### Tier 1 - Reference Hardware Platforms

* AMD Overdrive (revision B)
* HiSilicon D02
* LeMaker Cello

#### Tier 2 - Optional Test Hardware Platforms

* AMD Overdrive (revision A)
* Cavium Thunder-X
* HiSilicon D03
* Qualcomm Q2432LZB
