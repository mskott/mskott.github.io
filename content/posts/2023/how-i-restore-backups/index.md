---
title: "How I Restore Backups"
date: 2023-12-08T23:01:21+01:00
draft: true
tags:
    - backup
    - restore
    - vorta
    - borg
    - borgbase
    - linux
---
Recently wrote about how I do backups here is how I restore data

Pre-requisites:
- username, password, 2FA for Borgbase
- passphrase for the repo

- generate new SSH key and add it to Borgbase
- add the new key to the repository on Borgbase

## Borg CLI

- install borgbase `dnf install borgbase`
- `export BORG_BASE=` get URL from Borgbase
- `borg info` information about repo
- `borg list` to view archives in repo
- `borg extract` to restore files

## Vorta

- add new repo (choose existing)
- browse archives
- extract files