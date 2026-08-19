# Rationale

I maintain a local NFS mirror of the Slackware-current tree. To support
downgrades and experiments, the mirror intentionally retains older
package versions instead of pruning them when newer versions appear.

That layout is useful for normal package management, but it is
not equivalent to the package layout expected by the Slackware
installer. The installer loop in `usr/lib/setup/slackinstall` processes
all matching package files in a series directory. Pointing a fresh
installation at a retained-version mirror will therefore install many
versions of each package.

The result might include hundreds of versions of the
same package and can fill the target filesystem. This is an
operator-error/configuration-mismatch rather than a defect in the
installer: the installer behaves as expected for a conventional mirror.

The patch addresses this specific case by selecting the latest
version/build of each package before the normal installation loop
runs. It does not change Slackware's package-management policy or
attempt to remove older files from the source mirror.
