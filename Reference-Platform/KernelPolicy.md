## Why
The Reference Kernel is the combination of kernel features from the various Linaro segment groups and HW enablement branches for each of the supported platforms. It focuses on a mainline version of the kernel containing features that aren't merged into mainline yet.

The end result, members of Linaro will contribute to a kernel branch that incorporates the activities and development of all members.   The topics will be targeted towards tip, and rebased once the kernel has moved from -rc to stable.  As in, if the current -rc is for 4.4, then all topics will be based on that kernel.  Once the 4.4 kernel is released, all patches that were not accepted into the kernel release, will be rebased to the next developement kernel.  In the case of this example, rebase from 4.4 to 4.5.

See [Responsibilities](#Responsibilities) on how to get your patches into RPK.

## Rules
1. Patches will not break the single multi-platform kernel that is shipped as part of the RP
1. Each patch will ensure that it doesn't break the kernel build and booting. e.g. It is not OK for patches 1-3 to break the build and then patch 4 in a series fixing the build. Tools such at [git test-sequence](http://dustin.sallings.org/2010/03/28/git-test-sequence.html) can help automate this testing for a topic branch.
1. In general, kernel patches will be accepted in the RP kernel (for staging) only if they’re undergoing review on LKML or other relevant upstream (not @linaro.org) mailing lists first
1. Patches will be dropped from the RP kernel tree in the following cases:
  * Patch has been rejected upstream and maintainer has outlined a different approach to take
  * Patch is showing no sign of progress upstream - either through ongoing discussion or posting of follow-up versions based on earlier reviews
  * Patch doesn’t apply to a newer kernel
1. Changes to common core code will require signoff from the relevant kernel maintainer at Linaro
1. Members will designate an engineer who is responsible for each patch series and will address the questions/comments from the tree maintainers
1. Patches can only be accepted for HW platforms that are shared with Linaro
1. Each out-of-tree patch will be tagged (by the RPK maintainer) with one of the following keywords to allow easy accounting and tracking their progress upstream:
  * `fromlist`: Patch is picked up from a public mailing list where it is being reviewed
  * `fromtree`: Patch is picked up from a subsystem maintainer tree where it is already queued for merging
  * `temphack`: Patch is needed temporarily until some underlying code is fixed or refactored correctly upstream
  * `noup`: Patch is only needed for enabling pre-production HW or is only needed for local testing and won’t be pushed upstream. The problem will be fixed correctly upstream
  * `topost`: (to post) Patch isn’t posted to the list yet but posting is imminent. These patches will only be allowed very rarely solely at the discretion of the RP kernel maintainer
1. Statistics will be published regularly on how much functionality is still out-of-tree for each platform and high-level highlights of status of upstreaming for each feature being carried out-of-tree.

## Responsibilities
### Linaro Enterprise Group (LEG)
 LEG members will submit their kernel topics to LEG for inclusion.  These will then be integrated into a branch by the LEG kernel maintainer for inclusion into the RPK.

### Hardware enablement
 Basic support for new platforms should already exist in mainline before it'll be enabled in RPK. Additional features that are still pending upstreaming may be submitted via the relevant landing team or platform maintainer for inclusion into RPK.

