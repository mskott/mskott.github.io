---
title: "How I Restore Backups"
date: 2024-01-06
coverCaption: "Sails stored on the bow of a sailboat after the 2015 Fastnet race"
coverAlt: "Bags of sails stored on the bow of a sailboat"
tags:
    - backup
    - restore
    - vorta
    - borg
    - borgbackup
    - borgbase
    - linux
    - disaster recovery
---
My [recent post]({{< relref "/posts/2023/how-i-do-backups" >}}) about replacing my backup solution with [BorgBackup](https://www.borgbackup.org) ended with a bit of a cliffhanger...
I still needed to try and restore some data from backup.

I decided to test this by imagining the worst case scenario of my main computer, and all its data, being lost completely thus requiring me to restore data using only a fresh Linux machine and my password manager.

First, I needed to get access to BorgBase again in order to configure access from the fresh Linux machine.
Using the credentials stored in my password manager and my phone for 2FA I managed to log in to BorgBase. 
I then needed to generate a new SSH key, using `ssh-keygen`, and add the public key to BorgBase.
During this I learned an important detail about BorgBase - the SSH key is only used to authenticate the machine to BorgBase and isn't tied to the backup data itself, removing the need to backup the SSH key.

Before being able to use the key I just uploaded with BorgBase I needed to add it to the list of keys with access to a repo with the data I wanted to restore.
This was easily done using the BorgBase UI.

I chose to try two different methods of restoring data - using the BorgBackup CLI, and Vorta.
Before we get to that a short note on Borg repositories and passphrases.
The password of a repository is set when first creating the repository (using `borg init`) and is used to encrypt the repository.
The passphrase has nothing to do with your BorgBase password and should be kept somewhere safe - I store it in a note in my password manager.

## Borg CLI

The `borg(1)` command is pretty straight-forward once you get the hang of it and its extensive man page has lots of details and examples. The website has a very nice [quick start page](https://borgbackup.readthedocs.io/en/stable/quickstart.html) outlining the process from start to finish.

Restoring data to a new Fedora machine takes the following steps:

1. Install BorgBackup `dnf install borgbackup`
2. Store the URL of the repo to restore in a variable, `export BORG_REPO=ssh://...`. BorgBase provides a handy link to copy the URL to the clipboard. `borg` will look for the repo URL in `$BORG_REPO` unless you provide it on the command line
3. `borg info` will list summary information about the repo - useful to verify access
4. `borg list` to view archives in a repo. Each archive points to a point in time when a backup was taken
5. `borg extract` is used to extract data from an archive. To extract all the data in the archive _wombat-2023-12-28-115914_ to the current directory use the command `borg extract ::wombat-2023-12-28-115914`

`borg extract` has a number of useful options like `--dry-run` to simulate extracting data, `--list` to list the files while restoring, and `--exclude` to exclude files matching a certain pattern.

## Vorta

While restoring files using the CLI was quite simple it was a bit more confusing to restore the same data using Vorta.
Notably the distinction between adding a new or existing repository using the same button was a bit confusing, but otherwise the steps were quite similar to the CLI:

1. Install Vorta `dnf install vorta` and launch it
2. Add a new repository using the plus button making sure to select _Existing repository..._ and inputting the repository URL and passphrase
3. Click the _Archives_ tab to browse the archives
4. Select an archive and click _Extract_

## Afterthoughts

Restoring data was reasonably simple and quite snappy on my 1 Gbit/s internet connection, so I deemed the test a success and promptly signed up for the paid version of BorgBase to get space for all my data.
I really like how BorgBase allowed you to send a donation to the BorgBackup project at the same time as paying their subscription fee making it very simple to donate to the project.

The test also made me aware of two more things to consider for my disaster recovery: 

- I am nothing without my password manager, [Bitwarden](https://bitwarden.com), and I ought to consider having a second copy of it
- I need my phone for 2FA. I currently use the app [Authy](https://authy.com) for 2FA which can be restored from the cloud, but I need to look into what I need for doing that
