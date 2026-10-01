# Repository guidance

This fork packages systemd units and Bash helpers for installing and enrolling Defined Networking's `dnclient`. Read `README.md` and the affected script before making lifecycle changes. `install` installs binaries and templates; `bin/dnctl` owns enrollment/configuration; `units/` holds service templates.

## Critical behavior

The default shutdown path unenrolls a host, while reboot preserves enrollment. `DN_SKIP_UNENROLL=true` prevents unenrollment; `DN_UNENROLL_ON_REBOOT=true` changes reboot behavior. Preserve those distinctions and test them with disposable hosts. Unenrollment deletes the remote host and local network configuration and changes services; stopping a unit is not necessarily harmless.

The units use `envsubst`: copying raw templates directly does not produce installed units. Preserve network-instance naming and `dnclient` service notification compatibility. The README has historical version examples; inspect the actual installer and intended client version rather than assuming those examples describe a current deployment.

Lighthouse and relay flags are mutually exclusive. Lighthouses require static addresses and a nonzero listen port; relays also require a nonzero port. Keep API key scopes, role/network identity, cloud-instance naming, and service dependencies explicit.

## Verification

The README lists Bash 4.2+, systemd, envsubst, jq, and curl prerequisites. No automated CI or test suite was found in the current tree. Review template output and lifecycle behavior in an isolated systemd VM before deployment. Installation and `dnctl` commands can change network access; do not run them on a live host merely to verify documentation.

Keep `/etc/defined/dnctl` credential values out of logs and patches. Preserve upstream license and attribution; document fork-specific lifecycle changes in the README.
