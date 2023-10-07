---
title: "How I Do Backups"
date: 2023-10-07T17:49:35+02:00
draft: true
tags:
    - backup
    - borg
    - vorta
    - spideroak
    - borgbase
---

For the last six years I have been using a backup product called [SpiderOak One](https://crossclave.com/one/) which puts a large focus on privacy - it even got a recommendation from Edward Snowdon.
SpiderOak has since shifted their focus away from backup, and their closed-source client hasn't seen an update since [2019](https://spideroak.support/hc/en-us/articles/360023637011).
They haven't discontinued the product, but it obviously doesn't have their focus any longer.

Finding a replacement has been on my todo list for a while, and I have looked at various tools but never gotten further than looking.
Listening to [episode 9](https://linuxmatters.sh/9/) of the Linux Matters podcast titled *Big Backup Bonanza* gave me some much needed motivation to go further.

## Requirements

While I hadn't tried out any new backup tools, I had made a list of loose requirements which my new backup solution should satisfy.

  * Must have an open-source Linux client. Preferably packaged in the official repos of [Fedora](https://fedoraproject.org) and [Debian](https://debian.org).
  * Backups must be stored ofsite, on a service I don't have to run since I'd prefer to not be a sysadmin in my spare time.
  * Preferably multiple providers of ofsite locations to choose from.
  * Data should be stored in the EU. Not out of paranoia, but because I like not having everything in the US.

  Support for other platforms than Linux isn't really relevant for me since I don't use anything else. My wifes laptop runs Windows, but she stores all her data in Google Drive.

## Borg Backup

One of the tools  mentioned by the Linux Matters hosts was [Borg Backup](https://www.borgbackup.org), which I had also seen positive mentions of in other places.

Borg is released under the BSD license, and packages are available in the official repos of both Fedora and Debian.
There are several [service providers](https://www.borgbackup.org/support/commercial.html) which support it, including well-known companies like  Hetzner and rsync.net.


 [Vorta](https://vorta.borgbase.com/), and [BorgBase](https://www.borgbase.com/)

Mostly photos

Testing the backup

Linux Matters Episode 9 Bib Backup Bonanza https://linuxmatters.sh/9/
