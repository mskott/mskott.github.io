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
  * Backups must be stored off-site, on a service I don't have to run since I'd prefer to not be a sysadmin in my spare time.
  * Preferably multiple providers of off-site locations to choose from.
  * Data should be stored in the EU. Not out of paranoia, but because I like not having everything in the US.

My wife runs Windows on her laptop but all her data is on Google Drive, and since I only have Linux machines support for other platforms isn't a requirement.

## Borg Backup

One of the tools  mentioned by the Linux Matters hosts was [Borg Backup](https://www.borgbackup.org), which I had also seen positive mentions of in other places.

Borg is released under the BSD license, and packages are available in the official repos of both Fedora and Debian.
It is supported by several [service providers](https://www.borgbackup.org/support/commercial.html), including well-known companies like Hetzner and rsync.net.

I ended up creating an account on [BorgBase](https://www.borgbase.com/) which gives you 10 GB storage for free and offers a choice between storing you data in the EU or the US.
Additionally BorgBase is behind a GUI client for Borg called [Vorta](https://vorta.borgbase.com/).

Setting up a Borg repository on BorgBase was straight forward, and it provided a customised guide for configuring Borg and [Borgmatic](https://torsion.org/borgmatic/).
Strangely enough it didn't include instructions on setting up Vorta to use the new repo, so I had to resort to the [Vorta documentation](https://vorta.borgbase.com/usage/remote/) on how to setup a backup to a remote repository.

BorgBase provides has a useful monitoring feature which will alert you if a repository hasn't seen a new backup within a period of time.
They provide a couple of methods of alerting you, but for now I'm sticking with e-mail.

Mostly photos

Testing the backup

Linux Matters Episode 9 Bib Backup Bonanza https://linuxmatters.sh/9/
