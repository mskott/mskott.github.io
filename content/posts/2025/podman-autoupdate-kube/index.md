---
title: "Podman auto-update and Kubernetes YAML"
date: 2025-09-20T17:38:43+01:00
draft: true
tags:
    - podman
    - kubernetes
    - containers
---
One of the great features of Podman is the support for describing your deployments using Kubernetes YAML - a bit like Docker Compose where an entire deployment is described in a single file. In this post I will show how I combine it with another great Podman feature, automatic update of images, and fix a problem with podman `auto-update ` not being able to update some images. I recommend first reading [Major Haydens post](https://major.io/p/podman-quadlet-automatic-updates/) about podman auto-update if you are new to the feature.

On my home server I use Kubernetes YAML to run Immich in Podman. I chose to use Kubernetes YAML because I find it better suited for applications composed of multiple containers and volumes. The alternative was to write multiple Quadlets describing the network, pod, containers, and volumes.

I use `podman auto-update` to keep my container images up to date but every run of the service would fail.

```
systemd[1]: Starting podman-auto-update.service - Podman auto-update service...
podman[626102]: 2025-09-15 00:03:21.516740946 +0200 CEST m=+0.034405024 system auto-update
podman[626102]:             UNIT              CONTAINER                            IMAGE                                                                                                                                   POLICY      UPDATED
podman[626102]:             readeck.service   40168a3e772a (readeck-readeck)       codeberg.org/readeck/readeck:latest                                                                                                     registry    false
podman[626102]:             immich.service    289e43670aa7 (immich-redis)          docker.io/valkey/valkey:8-bookworm@sha256:facc1d2c3462975c34e10fccb167bfa92b0e0dbd992fc282c29a61c3243afb11                              registry    failed
podman[626102]:             immich.service    4a711b1385ef (immich-postgresql)     ghcr.io/immich-app/postgres:14-vectorchord0.4.3-pgvectors0.2.0@sha256:32324a2f41df5de9efe1af166b7008c3f55646f8d0e00d9550c16c9822366b4a  registry    failed
podman[626102]:             immich.service    0d8ccd4d14f2 (immich-immich-ml)      ghcr.io/immich-app/immich-machine-learning:release                                                                                      registry    false
podman[626102]:             immich.service    874fb48738f7 (immich-immich-server)  ghcr.io/immich-app/immich-server:release                                                                                                registry    false
podman[626102]:             jellyfin.service  068371c2db19 (systemd-jellyfin)      docker.io/jellyfin/jellyfin:latest                                                                                                      registry    false
podman[626102]:             caddy.service     761edf9561d1 (caddy-caddy)           docker.io/caddy:2.10                                                                                                                    registry    false
podman[626102]: Error: 2 errors occurred:
podman[626102]:         * checking image updates for container 289e43670aa7c5b379c7163fe064a111236d6bc76af2a7b926fb1ff24d9fc574: Docker references with both a tag and digest are currently not supported
podman[626102]:         * checking image updates for container 4a711b1385efa865f128070ad0e2e84b1f1c2ccc3c337ce795fb485f7815e6da: Docker references with both a tag and digest are currently not supported
systemd[1]: podman-auto-update.service: Main process exited, code=exited, status=125/n/a
systemd[1]: podman-auto-update.service: Failed with result 'exit-code'.
systemd[1]: Failed to start podman-auto-update.service - Podman auto-update service.
```

The error message `Docker references with both a tag and digest are currently not supported` can be a bit hard to decipher but it refers to the containers /immich-redis/ and /immich-postgresql/. Looking at the immich-redis container it uses the Valkey image labeled /8-bookworm/ followed by a hash /@sha256:.../ eg. a very specific version of the Valkey image. It makes sense /podman auto-update/ doens't know what to do about that image because it is never going to change - a label can be changed to point to a different version of the image but the hash can not.
