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
  - fedora
  - debian
---

For the last six years I have been using a backup product called [SpiderOak One](https://crossclave.com/one/) which puts a large focus on privacy - it even got a recommendation from Edward Snowden.
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
There are several [service providers](https://www.borgbackup.org/support/commercial.html) which support Borg, including well-known companies like  Hetzner and rsync.net.
I ended up creating a trial account with [BorgBase](https://www.borgbase.com/) which operates out of Malta and allows you to choose between storing your data in the EU or the United States.
The same people who are behind BorgBase also behind the GUI client [Vorta](https://vorta.borgbase.com/) which, again, is available in both Fedora and Debian.

Getting it all setup with Vorta and a trial account on BorgBase took around 30 minutes before I was ready to begin adding folders to Vorta's list of sources to back up. 
Vorta offers lots of options to schedule backups, but for now I have mostly stuck with the defaults. 
Only settings I made sure to check were to start Vorta automatically on login and to run missed backups on startup as I don't leave my laptop running 24/7.

BorgBase provides a monitoring feature where it alerts you if a repository hasn't seen a new backup within a certain period.
This is a simple solution to the problem of alerting you if your backup program for some reason is failing to run.

## Next steps...

There is nothing more useless than an untested backup so that is an obvious next step. 
Vorta's documentation includes a nice guide on how to [verify](https://docs.borgbase.com/verify/) your backup which I will also need to take a closer look at.
For now I have backed up about 7,5GB of files to BorgBase, but have yet to try restoring them on another system.

I would also like to take a look at [Borgmatic](https://torsion.org/borgmatic/) which looks like a very simple method of running Borg backups from the commandline.
Could be useful on a server where Vorta wouldn't be of much help.