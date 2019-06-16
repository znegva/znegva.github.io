---
layout: post
title: Update gitea on  uberspace
tags: git uberspace
update: 2019-06-16
---

Just the necessary step for updating gitea on uberspace 6.
Setup-guides (in german) can be found [here](https://blog.manuelhu.de/post/gogs-uberspace/) or [here](https://geeklabor.de/archives/195-Gogs-auf-Uberspace-installieren.html).

1. create a backup, just in case...  
   ```bash
   ~/bin/gitea % ./gitea dump
   ```
2. download the latest gitea binary from <https://dl.gitea.io/gitea/>   
   ```bash
   ~/bin/gitea % wget "https://dl.gitea.io/gitea/1.4.1/gitea-1.4.1-linux-amd64"
   ```

   Test the binary by calling `% ./gitea-1.4.1-linux-amd64 --version`  
   Maybe you will get something like `FATAL: kernel too old`, then you may join [this issue](https://github.com/go-gitea/gitea/issues/4131). 
3. stop the service
   ```bash
   ~/bin/gitea % svc -d ~/service/gitea
   ```
4. rename the old binary, rename the new binary, set executable flag (file permissions should already be ok)
   ```bash
   ~/bin/gitea % mv gitea gitea.old
   ~/bin/gitea % mv gitea-1.4.1-linux-amd64 gitea
   ~/bin/gitea % chmod +x gitea
   ```
5. start the service
   ```bash
   ~/bin/gitea % svc -u ~/service/gitea
   ```
6. test if everything works as expected, then remove the old binary and the dump
