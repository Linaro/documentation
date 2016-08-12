The reference platform kernel is located at:

  https://github.com/Linaro/rpk master

Changes for the reference platform kernel should have been posted upstream for
the current development kernel prior to submission to the reference platform
kernel. Changes do not need to have been accepted, they only need to have been
posted since the last merge window. They *do* need to work with other
changes in the reference platform kernel and meet quality standards but can
still be in review, the full policy can be seen in KernelPolicy.md

To submit changes:

1. Make a git branch based off Linus' most recent -rc1 tag (or a newer one if
   there are dependencies) with the changes
2. Create a tag (ideally signed using 'git tag -s'). The tag message should
   describe the change, why it is being proposed for the reference platform
   and link to the upstream submission (ideally using thread.gmane.org).
3. Send a pull request for the tag generated using 'git request-pull' to
  Mark Brown <broonie@linaro.org> and the RPK list (TBD).
