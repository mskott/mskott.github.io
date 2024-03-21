---
title: "Running ZFS on Fedora"
date: 2024-03-21
tags:
    - fedora
    - openzfs
    - zfs
---

I have an old desktop PC running Fedora Workstation, which I use as a media server and secondary workstation.
All the media files are stored on a [OpenZFS](https://github.com/openzfs/zfs) mirror on two SSD's.
OpenZFS is a bit of a strange bird on Fedora, since Fedora doesn't include support for it[^1].
In my experience it works perfectly, but you have to take some precautions to avoid risking some nasty issues.

Most importantly - I don't use OpenZFS as my root file system in order to avoid getting into the situation where the system can't boot due to being unable to mount it.
I might not be able to mount the zpool, but I have a better chance of fixing the issue since I can still log in.

One of the unique, but not often discussed, features of Fedora is that it closely follows kernel releases (see the [docs](https://docs.fedoraproject.org/en-US/quick-docs/kernel-overview/) for details) even after a version of Fedora has been released.
For most users that isn't an issue, but it can be when depending on kernel modules which aren't being updated at the same cadence as the kernel package - like OpenZFS.
Meaning that if you aren't careful you risk loosing access to your OpenZFS volumes after a kernel upgrade!
So I'm usually a little hesitant to upgrade the minor version of the kernel, e.g. 6.7 to 6.8, until OpenZFS has released a version supporting it.

[^1]: Fedora includes [zfs-fuse](https://github.com/gordan-bobic/zfs-fuse) which looks quite dead.
