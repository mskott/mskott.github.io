---
title: "Configuring libvirt guest lookup on Fedora"
date: 2023-09-22
draft: false
tags:
    - libvirt
    - fedora
    - authselect
---

[Gnome Boxes](https://apps.gnome.org/Boxes/) and [Virtual Machine Manager](https://virt-manager.org) are great tools for running virtual machines on Linux.
Both are using the [libvirt](https://libvirt.org) API to run and manage the virtual machines.
This doesn't just make the two tools interoperable, but also opens up for working with the entire ecosystem of utilities around libvirt and [libguestfs](https://libguestfs.org) allowing for endless automation of virtual machines.

A common use-case for me is to run headless VMs and then use SSH to connect to them.
Out of the box on Fedora I have to use the IP address of the VM to connect to it, which is a hassle. 
libvirt comes with the [libvirt-nss](https://libvirt.org/nss.html) module which plugs in to the hosts name resolution, and extends it to dynamically resolve hostnames and guest names.

Installation is really simple, but the configuration is a little different on Fedora which uses [authselect(8)](https://github.com/authselect/authselect/blob/1a8e4ed55981c9d95cb073f72a31649fe60935c5/src/man/authselect.8.adoc) to manage `/etc/nsswitch.conf` and related files.
Below is a simple how-to describing how I got it working on Fedora 38.

## How-to

1. Install the module: 
`dnf install libvirt-nss`
2. Enable the feature in authselect:
```shell
$ authselect enable-feature with-libvirt
Make sure that SSSD service is configured and enabled. See SSSD documentation for more information.
 
- with-libvirt is selected, make sure that the libvirt NSS plugins are installed
```