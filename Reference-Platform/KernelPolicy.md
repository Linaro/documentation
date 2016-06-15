## Why

The reference platform kernel will provide a common tree collecting Linaro-relevant platform support and other features that are actively being upstreamed in order to support integration testing, provide platform support for the reference platform and track upstreaming efforts.  A single tree will be maintained that merges relevant topics.

Some reference platforms may choose to add additional, out of tree code that does not meet the requirements for the RPK when building the kernels that they use.

See [Responsibilities](#responsibilities) on how to get your patches into RPK.

## Rules
1. Patches will not break the single multi-platform kernel that is shipped as part of the RP
1. Each patch will ensure that it doesn't break the kernel build and booting. e.g. It is not OK for patches 1-3 to break the build and then patch 4 in a series fixing the build. Tools such at [git test-sequence](http://dustin.sallings.org/2010/03/28/git-test-sequence.html) can help automate this testing for a topic branch.
1. In general, kernel patches will be accepted in the RP kernel (for staging) only if they’re undergoing review on LKML or other relevant upstream (not @linaro.org) mailing lists first
1. Patches will be dropped from the RP kernel tree in the following cases:
  * Patch has been rejected upstream and maintainer has outlined a different approach to take
  * Patch is showing no sign of progress upstream - either through ongoing discussion or posting of follow-up versions based on earlier reviews
1. Branch maintainers will be responsible for bringing branches forward to new versions, only branches based on current -rc releases will be included.
1. Changes to shared code will require more detailed review than driver specific functionality, ideally including some indication that the upstream community is happy with this approach.
1. Members will designate an engineer who is responsible for each patch series and will address the questions/comments from the tree maintainers
1. Statistics will be published regularly on how much functionality is still out-of-tree for each platform and high-level highlights of status of upstreaming for each feature being carried out-of-tree.

## Responsibilities
### Linaro Enterprise Group (LEG)
 LEG members will submit their kernel topics to LEG for inclusion.  These will then be integrated into a branch by the LEG kernel maintainer for inclusion into the RPK.

### Hardware enablement
 Basic support for new platforms should already exist in mainline before it'll be enabled in RPK. Additional features that are actively being worked on upstream may be submitted via the relevant landing team or platform maintainer for inclusion into RPK.

