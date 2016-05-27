# Contributions to OpenEmbedded Reference Platform Build

OpenEmbedded Reference Platform Build layer descriptions and how/where to send patches.

## Layers

### OE-core (Base layer)

###### About

This is the main collaboration point when working on OpenEmbedded projects and is part of the core recipes. The goal of this layer is to have just enough recipes to build a basic system, this means keeping it as small as possible.

###### Contribute

Submit all patches through the OpenEmbedded Core Mailing list

http://lists.openembedded.org/mailman/listinfo/openembedded-core

***

### Meta-rpb (Distro)

###### About

This is a very small layer where the distro configurations live. Currently it houses both Reference Platform Build and Wayland Reference Platform Builds.

###### Contribute

Submit all patches through github with a pull request

https://github.com/96boards/meta-rpb

***

### Meta-oe

###### About

This layer houses many useful, but sometimes unmaintained recipes. Since the reduction in recipes to the core, meta-oe was created for everything else. There are currently approximately 650 recipes in this layer.

###### Contribute

Submit all patches through the OpenEmbedded Developer Mailing list

http://lists.openembedded.org/mailman/listinfo/openembedded-devel

***

### Meta-browser

###### About

This layer holds the recipes for Firefox and Chromium. Both recipes require a lot of maintenance, because of this a seperate layer was created.

###### Contribute

Submit all patches through the OpenEmbedded Developer Mailing list

http://lists.openembedded.org/mailman/listinfo/openembedded-devel

***

### Meta-qt5

###### About

This is a cross-platform toolkit.

###### Contribute

Submit all patches through the OpenEmbedded Developer Mailing list

http://lists.openembedded.org/mailman/listinfo/openembedded-devel

***

### Meta-linaro

###### About

This layer is used to get the Linaro toolchain.

###### Contribute

**For Linaro internal:**

- https://review.linaro.org/#/dashboard/self
- https://git.linaro.org/openembedded/meta-linaro.git/tree

**For external contributors:**

Submit all patched through the OpenEmbedded Mailing list

https://lists.linaro.org/mailman/listinfo/openembedded

***

### Meta-linaro-backports

###### About

This is an experimental layer used to get newer versions into the build which were not part of the release.

###### Contribute

Submit all patched through the OpenEmbedded Mailing list

https://lists.linaro.org/mailman/listinfo/openembedded

***

### Meta-96Boards

###### About

This support layer is managed by Linaro and intended for boards that do not have their own board support layer. Currently used for the HiKey Consumer edition board, and eventually the Bubblegum-96 board. If a vendor does not support their own layer, it can be added to this layer.

###### Contribute

Submit all patches through github with a pull request

https://github.com/96boards/meta-96boards

***

### Meta-qcom (BSP)

###### About

This is the board support layer for Qualcomm boards. Currently supports IFC6410 and the DragonBoard 410c.

###### Contribute

Submit all patches through github with a pull request

https://github.com/ndechesne/meta-qcom

***
