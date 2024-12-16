---
title: "Bootc Signed Images"
date: 2024-12-16T21:55:06+01:00
tags:
    - bootc
    - containers
    - podman
---

Lately I have been having a lot of fun experimenting with bootable containers using the [bootc](https://containers.github.io/bootc/) project.
I have also been fiddling around with signing container images which led me to the obvious question: *Does bootc support signed images?*

We first need to create a key-pair we can use to sign the image. In this case I'm using [Skopeo](https://github.com/containers/skopeo), but [Cosign](https://github.com/sigstore/cosign) can also be used.

``` shellsession
# skopeo generate-sigstore-key --output-prefix bootc-demo
```

Then we can go on to creating a container image, in this instance I'm using RHEL 9.5 as the base, but it doesn't really matter.

``` dockerfile
FROM registry.redhat.io/rhel9/rhel-bootc:latest
COPY quay.io.yaml /etc/containers/registries.d
COPY bootc-demo.pub /usr/local/etc
COPY policy.json /etc/containers
```

The three files being copied are what is needed to verify the signature of a image when downloading it.
[quay.io.yaml](https://github.com/mskott/bootc-signed/blob/main/quay.io.yaml) tells Podman to download signatures when pulling an image and [policy.json](https://github.com/mskott/bootc-signed/blob/main/policy.json) defines that images pulled from my repository on quay.io must be verified using the public key created earlier.

Now we are ready to build our container, sign it using our private key, and upload it to the repository.

``` shellsession
# podman build -t quay.io/rh-ee-mskoett/bootc-signed:latest .
# podman push --sign-by-sigstore-private-key ./bootc-demo.private  quay.io/rh-ee-mskoett/bootc-signed:latest
```

With the image pushed to a repository we can now boot a VM with it using [podman-bootc](https://github.com/containers/podman-bootc).

``` shellsession
$ podman-bootc run --filesystem=xfs quay.io/rh-ee-mskoett/bootc-signed:latest
  Booting `Red Hat Enterprise Linux 9.20241216.0.5 (Plow) (ostree:0)'

Connecting to vm aefbbc0576844bdf41da77e296c5b20fbc049b45821b715f7791425e8d296c9b. To close connection, use `~.` or `exit`
root@localhost ~]# bootc status
No staged image present
Current booted image: quay.io/rh-ee-mskoett/bootc-signed:latest
    Image version: 9.20241216.0 (2024-12-16 15:40:10.955783592 UTC)
    Image digest: sha256:cf59749ea0ecf31a6c5f482f76fd59a78f49a5a4bb8095db04a9eeeab15e6b64
No rollback image present
```

Let's rebuild our container image, but this time we wont be signing it when pushing it to the repository.

``` shellsession
# podman build -t quay.io/rh-ee-mskoett/bootc-signed:latest .
# podman push quay.io/rh-ee-mskoett/bootc-signed:latest
```

Now let's see what happens if we try to upgrade our running VM to the updated image:

``` shellsession
[root@localhost ~]# bootc upgrade
ERROR Upgrading: Pulling: Creating importer: failed to invoke method OpenImage: failed to invoke method OpenImage: A signature was required, but no signature exists
```

As we can see, bootc didn't allow the VM to be upgraded to the unsigned image.
Being able to verify that OS updates come from a trusted source is a key requirement in modern computing.
Bootable containers are still a relatively new thing, so already having verifiable updates is testament to the benefits of building it on top of container technology.

The files used in this experiment can be found in [this Git repository](https://github.com/mskott/bootc-signed/blob/main/quay.io.yaml).
