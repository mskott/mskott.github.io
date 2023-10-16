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

[GitHub](https://github.com)

[Raising the bar for software security: next steps for GitHub.com 2FA](https://github.blog/2022-12-14-raising-the-bar-for-software-security-next-steps-for-github-com-2fa/)

[Git Credential Manager Core](https://aka.ms/gcmcore) - replaced Git Credential Manager for Windows

git-credential-oauth

```
[credential]
	helper = cache --timeout 7200
	helper = oauth
```