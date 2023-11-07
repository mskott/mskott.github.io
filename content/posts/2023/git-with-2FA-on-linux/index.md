---
title: "Git With 2FA on Linux"
date: 2023-10-07T18:17:27+02:00
draft: true
tags:
    - git
    - 2fa
    - fedora
    - linux
    - github
    - wsl2
    - windows
---

SSH keys have, for a long time, been the preferred method of authentication when using Git with remote repositories.
These days, with increased security awareness, many organisations are requiring some variant of multi-factor authentication for all services - Git included.

GitHub has announced[^1] that they are taking it one step further, and will require that all users contributing code enable one or more forms of two-factor authentication by the end of 2023.

[^1]:[Raising the bar for software security: next steps for GitHub.com 2FA](https://github.blog/2022-12-14-raising-the-bar-for-software-security-next-steps-for-github-com-2fa/)

That leaves Git users with a problem - if they begin cloning repositories using HTTP instead of SSH they will get a password prompt every time interacting with the remote repository.
And they will have to input a valid GitHub access token instead of their password - every time!

Thankfully there are several solutions to solve this issue, without compromising security 😀

## git-credential-oauth

One option on Linux is [git-credential-oauth](https://github.com/hickford/git-credential-oauth) which is a credential helper for Git which uses OAuth to securely authenticate to a bunch of popular Git hosts.
It requires very little setup to get running, and once done it will open your preferred browser every time Git needs to authenticate with a remote repository.
The retrieved token is cached automatically so it won't open a browser window all the time.

The motivation section of the git-credential-oauth README[^2] file provides a nice comparison between it and other solutions like SSH and personal access tokens.

[^2]: https://github.com/hickford/git-credential-oauth#motivation 

Installing and configuring git-credential-oauth is easy.
The steps below are based on Fedora, but should be similar on any other Linux distribution.

1. Install the package using `dnf install git-credential-oauth`
2. Run `git credential-oauth configure` to configure Git

Step 2 will update your `~/.gitconfig` to include the lines below - you could also add them yourself if you don't want others meddling with your Git configuration.

```
[credential]
	helper = cache --timeout 7200
	helper = oauth
```

Not all systems have have a browser though, so git-credential-oath also supports[^3] the OAuth device flow[^4].
I have yet to try it out, but setup looks very simple.
[^3]:https://github.com/hickford/git-credential-oauth#browserless-systems 
[^4]:[RFC-8628-OAuth 2.0 Device Authorization Grant](https://www.rfc-editor.org/rfc/rfc8628)

## Git Credential Manager Core

[Git Credential Manager Core](https://aka.ms/gcmcore) - replaced Git Credential Manager for Windows
