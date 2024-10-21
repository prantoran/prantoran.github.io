---
title: Packages out of sync after trying to install udev in Ubuntu 24.04 LTS
menu:
  sidebar:
    name: Packages out of sync after trying to install udev in Ubuntu 24.04 LTS
    identifier: udev_ubuntu24_conflicts
    parent: pkg_deps
    weight: 10
---

So I was blindly installing deps for a repo and ended up with conflicint packages after installing udev. 

When I tried to fix the deps using `apt --fix-broken install`, I got the error `The udev package cannot be installed because this system does not have a merged /usr`:
```bash

...
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
Correcting dependencies... Done
The following additional packages will be installed:
  systemd udev
Suggested packages:
  systemd-container systemd-homed systemd-userdbd systemd-boot
The following packages will be upgraded:
  systemd udev
2 upgraded, 0 newly installed, 0 to remove and 153 not upgraded.
9 not fully installed or removed.
Need to get 0 B/5,343 kB of archives.
After this operation, 0 B of additional disk space will be used.
Do you want to continue? [Y/n] y
(Reading database ... 228618 files and directories currently installed.)
Preparing to unpack .../systemd_255.4-1ubuntu8.4_amd64.deb ...


******************************************************************************
*
* The systemd package cannot be installed because this system does
* not have a merged /usr.
*
* Please install the usrmerge package to convert this system to merged-/usr.
*
* For more information please read https://wiki.debian.org/UsrMerge.
*
******************************************************************************


dpkg: error processing archive /var/cache/apt/archives/systemd_255.4-1ubuntu8.4_
amd64.deb (--unpack):
 new systemd package pre-installation script subprocess returned error exit stat
us 1
dpkg: considering deconfiguration of systemd, which would be broken by installat
ion of udev ...
dpkg: yes, will deconfigure systemd (broken by udev)
Preparing to unpack .../udev_255.4-1ubuntu8.4_amd64.deb ...
De-configuring systemd (255.4-1ubuntu8), to allow installation of udev (255.4-1u
buntu8.4) ...


******************************************************************************
*
* The udev package cannot be installed because this system does
* not have a merged /usr.
*
* Please install the usrmerge package to convert this system to merged-/usr.
*
* For more information please read https://wiki.debian.org/UsrMerge.
*
******************************************************************************


dpkg: error processing archive /var/cache/apt/archives/udev_255.4-1ubuntu8.4_amd
64.deb (--unpack):
 new udev package pre-installation script subprocess returned error exit status 
1
Errors were encountered while processing:
 /var/cache/apt/archives/systemd_255.4-1ubuntu8.4_amd64.deb
 /var/cache/apt/archives/udev_255.4-1ubuntu8.4_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

I tried `sudo apt upgrade` but got conflicts:
```bash
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
You might want to run 'apt --fix-broken install' to correct these.
The following packages have unmet dependencies:
 libnss-systemd : Depends: systemd (= 255.4-1ubuntu8.4) but 255.4-1ubuntu8 is installed
 libpam-systemd : Depends: systemd (= 255.4-1ubuntu8.4) but 255.4-1ubuntu8 is installed
 systemd : Depends: libsystemd-shared (= 255.4-1ubuntu8) but 255.4-1ubuntu8.4 is installed
           Depends: libsystemd0 (= 255.4-1ubuntu8) but 255.4-1ubuntu8.4 is installed
 systemd-resolved : Depends: systemd (= 255.4-1ubuntu8.4) but 255.4-1ubuntu8 is installed
 systemd-sysv : Depends: systemd (= 255.4-1ubuntu8.4) but 255.4-1ubuntu8 is installed
 udev : Depends: libudev1 (= 255.4-1ubuntu8) but 255.4-1ubuntu8.4 is installed
E: Unmet dependencies. Try 'apt --fix-broken install' with no packages (or specify a solution).
```

## Option 1: Manually update the deps

After some soul searching [1], I found the suggestion to manually update the deps in the `/var/lib/dpkg/status` file.

So basically there was a security patch [2] for systemd that lead a version from 255.4-1ubuntu8 to 255.4-1ubuntu8.4 but I messed up the deps somehow.

As an experiment, I decided to go with  255.4-1ubuntu8.4 and iteratively change the dependencies in the status file one at at a time and try to `apt --fix-broken install`.

For example, for `udev`, I searched for the package entry in the `/var/lib/dpkg/status` file using the vim search command `/Package: udev`, pressed Enter, then searched for `libudev1 (= 255.4-1ubuntu8)` and changed it to `libudev1 (= 255.4-1ubuntu8.4)`.

Also, we can find the correct version to add for a package by going into the package block and look for the `Version` entry.

And voila! It works (at least no crashes yet).


## Restore dpkg/status from backups

`dpkg` does a regular backup of the complete package status of the system to `/var/backups/dpkg.status.*.gz`. If you think your package status if out of sync with the actual packages installed, you can replace the status file at `/var/lib/dpkg/status` with the status file contained in the backup [3]. Run the following commands with `sudo`:

```bash
cp /var/lib/dpkg/status /var/lib/dpkg/status.bak
cp /var/backups/dpkg.status.*.gz /var/lib/dpkg/
gunzip -d /var/lib/dpkg/dpkg.status.*.g
```

There will be a bunch of `dpkg.status` backups in `/var/lib/dpkg`. Iteratively try each one (I tried in descending order), replacing dpkg.status and apt upgrade.

```bash
ls  /var/lib/dpkg
alternatives    dpkg.status.1  info           status
available       dpkg.status.2  lock           status.bak
cmethopt        dpkg.status.3  lock-frontend  status-old
diversions      dpkg.status.4  parts          triggers
diversions-old  dpkg.status.5  statoverride   updates
```

```bash
mv /var/lib/dpkg/dpkg.status.* /var/lib/dpkg/status
aot update && apt upgrade
```

# Ref

[1] https://askubuntu.com/a/182872/415173

[2] https://linuxpatch.com/updates/systemd_255.4-1ubuntu8.4

[3] https://askubuntu.com/a/43281/415173
