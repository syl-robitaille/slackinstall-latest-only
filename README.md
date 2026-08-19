# Slackware Installer: Select Latest Package Versions

This repository contains a patch for Slackware-current's installer
script that makes package selection saner when installing from a
local mirror containing multiple versions of the same package.

## Problem

The Slackware installer normally processes every matching package
in a package-series directory. That is correct for a conventional
Slackware mirror, where obsolete packages have been removed. It can
produce an unusable installation when a local mirror intentionally
retains older package versions for rollback or testing.

In that situation, the installer will attempt to install every version
of the same package that it finds, consuming excessive space on the
target disk.

## Change

The patch changes `usr/lib/setup/slackinstall` (in the installation
mini-root) so that, for each package name in a series directory, it
selects only the latest package filename according to version-aware
sorting. The existing installation flow remains otherwise unchanged.

## Repository contents

- `CHANGELOG.md` will describe any changes made since I created the patch.
- `LICENSE` project documentation and patch licensing note.
- `README.md` this file
- `rationale.md` background and intended use case.
- `setup-slackinstall-latest-package.patch` unified diff of the installer script.

## Applying the patch

You'll need to work as root ...

```sh
# create a working directory
mkdir /path/to/workdir
cd /path/to/workdir

# unpack the miniroot
xzcat /path/to/initrd.img | cpio -iv

# apply the patch
patch -p0 < /path/to/setup-slackinstall-latest-package.patch

# create new initrd image
find . -print0 | cpio --null -ov --format=newc | xz -9 > /path/to/initrd_NEW.img

# make sure your installer will use the new initrd image
mv -i /path/to/initrd.img /path/to/initrd_OLD.img

ln /path/to/initrd_NEW.img /path/to/initrd.img
```

Install as you would normally from your Slackware-current mirror.


## Important note

This is an independent community patch, not an official Slackware
change. Test it with the exact Slackware-current installer and package
layout you intend to use. Keep a conventional mirror or installation
medium available as a fallback.
