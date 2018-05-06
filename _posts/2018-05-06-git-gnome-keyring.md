---
layout: post
title: Use gnome-keyring (and others) with git
tags: git Debian
---

Steps on how to use libsecret to store git-login-data.  
libsecret supports implementations of XDG Secret Service API such as the gnome-keyring.

1. install `libsecret` dev libs:  
  ```
  ~ % sudo apt-get install libsecret-1-0 libsecret-1-dev
  ```
2. build the credential manager  
  ```bash
  ~ % cd /usr/share/doc/git/contrib/credential/libsecret
  libsecret % sudo make
  ```
3. let git know what you have prepared:  
  ```
  ~ % git config --global credential.helper /usr/share/doc/git/contrib/credential/libsecret/git-credential-libsecret
  ```


Note: need at least git 2.11
